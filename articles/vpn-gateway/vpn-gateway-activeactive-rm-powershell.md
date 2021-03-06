<properties
   pageTitle="Como configurar conexões VPN S2S ativo-ativo com gateways de VPN do Azure usando o Azure Resource Manager e o PowerShell | Microsoft Azure"
   description="Este artigo mostra como configurar conexões ativo-ativo com gateways de VPN do Azure usando o Azure Resource Manager e o PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# Configurar conexões VPN S2S ativa-ativa com Gateways de VPN do Azure usando o Azure Resource Manager e o PowerShell

Este artigo explica as etapas para criar conexões VNet para VNet e entre instalações ativo-ativo usando o modelo de implantação do Resource Manager e o PowerShell.


**Sobre modelos de implantação do Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Sobre conexões altamente disponíveis entre instalações

Para obter alta disponibilidade para conectividade entre instalações e VNet para VNet, você deve implantar vários gateways de VPN e estabelecer várias conexões paralelas entre suas redes e o Azure. Consulte [Conectividade Altamente Disponível entre os Locais e VNet para VNet](./vpn-gateway-highlyavailable.md) para obter uma visão geral das opções de conectividade e topologia.

Este artigo fornece as instruções para configurar uma conexão VPN entre os locais ativo-ativo e uma conexão ativo-ativo entre duas redes virtuais:

- [Parte 1 – Criar e configurar seu gateway de VPN do Azure no modo ativo-ativo](#aagateway)

- [Parte 2 – Estabelecer conexões entre os locais ativo-ativo](#aacrossprem)

- [Parte 3 – Estabelecer conexões VNet para VNet ativo-ativo](#aav2v)

- [Parte 4 – Atualizar o gateway existente entre ativo-ativo e ativo-em espera](#aaupdate)

Você pode combiná-los para criar uma topologia de rede mais complexa e altamente disponível que atenda às suas necessidades.

>[AZURE.IMPORTANT] Observe que o modo ativo-ativo funciona somente no SKU de HighPerformance


## <a name ="aagateway"></a>Parte 1 – Criar e configurar os gateways de VPN ativo-ativo

As etapas a seguir configurarão seu gateway de VPN do Azure nos modos ativo-ativo. As principais diferenças entre os gateways ativo-ativo e ativo-em espera:

- Você precisa criar duas configurações de IP do gateway com dois endereços IP públicos
- Você precisa definir o sinalizador EnableActiveActiveFeature
- O SKU do gateway deve ser HighPerformance

As outras propriedades são as mesmas que os gateways não ativo-ativo.

### Antes de começar

- Verifique se você tem uma assinatura do Azure. Se ainda não tiver uma assinatura do Azure, você poderá ativar os [Benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou inscrever-se para obter uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).
	
- Você precisará instalar os cmdlets do Azure Resource Manager PowerShell. Confira [Como instalar e configurar o Azure PowerShell](../powershell-install-configure.md) para saber mais sobre como instalar os cmdlets do PowerShell.

### Etapa 1 - Criar e configurar VNet1

#### 1\. Declare as variáveis

Para este exercício, começaremos declarando nossa variáveis. O exemplo a seguir declara as variáveis usando os valores para este exercício. Certifique-se de substituir os valores pelos seus próprios ao configurar para a produção. Se você estiver executando as etapas para se familiarizar com esse tipo de configuração, poderá usar essas variáveis. Modifique as variáveis e, em seguida, copie e cole em seu console do PowerShell.

	$Sub1          = "Ross"
	$RG1           = "TestAARG1"
	$Location1     = "West US"
	$VNetName1     = "TestVNet1"
	$FESubName1    = "FrontEnd"
	$BESubName1    = "Backend"
	$GWSubName1    = "GatewaySubnet"
	$VNetPrefix11  = "10.11.0.0/16"
	$VNetPrefix12  = "10.12.0.0/16"
	$FESubPrefix1  = "10.11.0.0/24"
	$BESubPrefix1  = "10.12.0.0/24"
	$GWSubPrefix1  = "10.12.255.0/27"
	$VNet1ASN      = 65010
	$DNS1          = "8.8.8.8"
	$GWName1       = "VNet1GW"
	$GW1IPName1    = "VNet1GWIP1"
	$GW1IPName2    = "VNet1GWIP2"
	$GW1IPconf1    = "gw1ipconf1"
	$GW1IPconf2    = "gw1ipconf2"
	$Connection12  = "VNet1toVNet2"
	$Connection151 = "VNet1toSite5_1"
	$Connection152 = "VNet1toSite5_2"

#### 2\. Conectar à sua assinatura do Azure e criar um novo grupo de recursos

Alterne para o modo do PowerShell para usar os cmdlets do Gerenciador de Recursos. Para obter mais informações, consulte [Usando o Windows PowerShell com o Gerenciador de Recursos](../powershell-azure-resource-manager.md).

Abra o console do PowerShell e conecte-se à sua conta. Use o exemplo a seguir para ajudar com a conexão:

	Login-AzureRmAccount
	Select-AzureRmSubscription -SubscriptionName $Sub1
	New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### 3\. Criar TestVNet1

O exemplo abaixo cria uma rede virtual denominada TestVNet1 e três sub-redes: GatewaySubnet, FrontEnd e Backend. Ao substituir valores, é importante você sempre nomear sua sub-rede de gateway especificamente como GatewaySubnet. Se você usar outro nome, a criação do gateway falhará.

	$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
	$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
	$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

	New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### Etapa 2 – Criar o gateway de VPN para TestVNet1 com modo ativo-ativo

#### 1\. Criar os endereços IP públicos e as configurações de IP do gateway

Solicite dois endereços IP públicos para serem alocados ao gateway que você criará para sua VNet. Você também definirá as configurações de IP e sub-rede necessárias.

	$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
	$gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

	$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
	$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
	$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
	$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### 2\. Criar o gateway de VPN com configuração ativo-ativo

Crie o gateway de rede virtual para TestVNet1. Observe que há duas entradas GatewayIpConfig, e o sinalizador EnableActiveActiveFeature foi definido. O modo ativo-ativo requer um gateway de VPN baseado em rota do SKU de HighPerformance. Criar um gateway pode demorar um pouco (30 minutos ou mais para ser concluído).

	New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### 3\. Obter os endereços IP públicos do gateway e o endereço IP do par no nível de protocolo BGP

Depois de criar o gateway, você precisará obter o endereço IP do par no nível de protocolo BGP no Gateway de VPN do Azure. Esse endereço é necessário para configurar o Gateway de VPN do Azure como um par no nível de protocolo BGP para os dispositivos VPN locais.

	$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
	$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Use os seguintes cmdlets para mostrar os dois endereços IP públicos alocados para seu gateway de VPN e seus endereços IP do par no nível de protocolo BGP correspondentes para cada instância do gateway:

	PS D:> $gw1pip1.IpAddress
	40.112.190.5

	PS D:> $gw1pip1.IpAddress
	138.91.156.129

	PS D:> $vnet1gw.BgpSettingsText
	{
	  "Asn": 65010,
	  "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
	  "PeerWeight": 0
	}

A ordem dos endereços IP públicos para as instâncias do gateway e os endereços de emparelhamento via protocolo BGP correspondentes são os mesmos. Neste exemplo, a VM do gateway com o IP público 40.112.190.5 usará 10.12.255.4 como seu endereço de emparelhamento via protocolo BGP, e o gateway com 138.91.156.129 usará 10.12.255.5. Essas informações são necessárias quando você configura os dispositivos VPN locais que se conectam ao gateway ativo-ativo. O gateway é mostrado no diagrama abaixo, com todos os endereços:

![gateway ativo-ativo](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Depois de criar o gateway, você poderá usá-lo para estabelecer a conexão entre os locais ativo-ativo ou a conexão VNet para VNet. As seções a seguir explicam as etapas para concluir o exercício.


## <a name ="aacrossprem"></a>Parte 2 – Estabelecer uma conexão entre os locais ativo-ativo

Para estabelecer uma conexão entre instalações, você precisará criar um Gateway de Rede Local para representar o dispositivo VPN local e uma Conexão para conectar o gateway de VPN do Azure com ao gateway de rede local. Neste exemplo, o gateway de VPN do Azure está no modo ativo-ativo. Como resultado, mesmo que haja apenas um dispositivo VPN local (gateway de rede local) e um recurso de conexão, as duas instâncias de gateway de VPN do Azure estabelecerão túneis de VPN S2S com o dispositivo local.

Antes de prosseguir, verifique se você concluiu a [Parte 1](#aagateway) deste exercício.

### Etapa 1 - Criar e configurar o gateway de rede local

#### 1\. Declare as variáveis

Este exercício continuará a compilar a configuração mostrada no diagrama. Não se esqueça de substituir os valores com aqueles que você deseja usar para sua configuração.

	$RG5           = "TestAARG5"
	$Location5     = "West US"
	$LNGName51     = "Site5_1"
	$LNGPrefix51   = "10.52.255.253/32"
	$LNGIP51       = "131.107.72.22"
	$LNGASN5       = 65050
	$BGPPeerIP51   = "10.52.255.253"

Algumas coisas para observar relacionadas aos parâmetros de gateway de rede:

- O gateway de rede local pode estar no mesmo ou em outro local e grupo de recursos como o gateway de VPN. Este exemplo os mostra em grupos de recursos diferentes, mas na mesma localização do Azure.

- Se houver apenas um dispositivo VPN local, conforme mostrado acima, a conexão ativo-ativo poderá funcionar com ou sem o protocolo BGP. Este exemplo usa o BGP para a conexão entre os locais.

- Se o BGP for habilitado, o prefixo que você precisará declarar para o gateway de rede local é o endereço de host do seu endereço IP do par no nível de protocolo BGP em seu dispositivo VPN. Nesse caso, é um prefixo /32 de "10.52.255.253/32".

- Como lembrete, você deve usar diferentes ASNs BGP entre suas redes locais e a VNet do Azure. Se eles forem iguais, você precisará alterar seu ASN VNet se o dispositivo VPN local já usar o ASN para emparelhar com outros vizinhos de BGP.

#### 2\. Criar o gateway de rede local para Site5
	
Antes de continuar, verifique se que você ainda está conectado à Assinatura 1. Crie o grupo de recursos se ele ainda não tiver sido criado.

	New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
	New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### Etapa 2 - Conectar o gateway de VNet e o gateway de rede local

#### 1\. Obter os dois gateways

	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
	$lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### 2\. Criar a conexão TestVNet1 para Site5

Nesta etapa, você criará a conexão de TestVNet1 para Site5\_1 com "EnableBGP" definido como $True.

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### 3\. Parâmetros VPN e BGP para seu dispositivo VPN local

O exemplo a seguir lista os parâmetros que você inserirá na seção de configuração de BGP em seu dispositivo VPN local para este exercício:

	- Site5 ASN            : 65050
	- Site5 BGP IP         : 10.52.255.253
	- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
	- Azure VNet ASN       : 65010
	- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
	- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
	- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
	                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
	- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed

A conexão deve ser estabelecida após alguns minutos, e a sessão de emparelhamento via protocolo BGP iniciará uma vez estabelecida a conexão IPsec. Até agora, este exemplo configurou apenas um dispositivo VPN local, resultando no diagrama mostrado abaixo:

![ativo-ativo-entre-os-locais](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### Etapa 3 – Conectar dois dispositivos VPN locais ao gateway de VPN ativo-ativo

Se você tiver dois dispositivos VPN na mesma rede local, você poderá obter redundância dupla ao conectar o gateway de VPN do Azure ao segundo dispositivo VPN.

#### 1\. Criar o segundo gateway de rede local para Site5

Observe que o endereço IP do gateway, o prefixo do endereço e o endereço de emparelhamento via protocolo BGP para o segundo gateway de rede local não devem se sobrepor ao gateway de rede local anterior para a mesma rede local.

	$LNGName52     = "Site5_2"
	$LNGPrefix52   = "10.52.255.254/32"
	$LNGIP52       = "131.107.72.23"
	$BGPPeerIP52   = "10.52.255.254"

	New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### 2\. Conectar o gateway da VNet e segundo gateway de rede local

Crie a conexão de TestVNet1 para Site5\_2 com "EnableBGP" definido como $True

	$lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### 3\. Parâmetros VPN e BGP para seu segundo dispositivo VPN local

Da mesma forma, encontram-se abaixo os parâmetros que você digitará no segundo dispositivo VPN:

	- Site5 ASN            : 65050
	- Site5 BGP IP         : 10.52.255.254
	- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
	- Azure VNet ASN       : 65010
	- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel to 40.112.190.5
	- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel to 138.91.156.129
	- Static routes        : Destination 10.12.255.4/32, nexthop the VPN tunnel interface to 40.112.190.5
	                         Destination 10.12.255.5/32, nexthop the VPN tunnel interface to 138.91.156.129
	- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed

Depois que (os túneis da) conexão forem estabelecidos, você terá túneis e dispositivos VPN redundantes duplos conectando-se à sua rede local e ao Azure:

![redundância-dupla-entre-os-locais](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Parte 3 – Estabelecer uma conexão VNet para VNet ativo-ativo

Esta seção cria uma conexão VNet para VNet ativo-ativo com o BGP.

As instruções abaixo continuam das etapas anteriores listadas acima. Você deve concluir a [Parte 1](#aagateway) para criar e configurar TestVNet1 e o Gateway de VPN com BGP.

### Etapa 1 - Criar TestVNet2 e gateway de VPN

É importante certificar-se de que o espaço de endereço IP da nova rede virtual, TestVNet2, não se sobreponha a nenhum dos seus intervalos de VNet.

Neste exemplo, as redes virtuais pertencem à mesma assinatura. Você pode configurar conexões de VNet para VNet entre assinaturas diferentes; consulte [Configurar uma conexão VNet para VNet](./vpn-gateway-vnet-vnet-rm-ps.md) para saber mais detalhes. Verifique se você adicionou "-EnableBgp $True" ao criar conexões para habilitar o BGP.

#### 1\. Declare as variáveis

Não se esqueça de substituir os valores com aqueles que você deseja usar para sua configuração.

	$RG2           = "TestAARG2"
	$Location2     = "East US"
	$VNetName2     = "TestVNet2"
	$FESubName2    = "FrontEnd"
	$BESubName2    = "Backend"
	$GWSubName2    = "GatewaySubnet"
	$VNetPrefix21  = "10.21.0.0/16"
	$VNetPrefix22  = "10.22.0.0/16"
	$FESubPrefix2  = "10.21.0.0/24"
	$BESubPrefix2  = "10.22.0.0/24"
	$GWSubPrefix2  = "10.22.255.0/27"
	$VNet2ASN      = 65020
	$DNS2          = "8.8.8.8"
	$GWName2       = "VNet2GW"
	$GW2IPName1    = "VNet2GWIP1"
	$GW2IPconf1    = "gw2ipconf1"
	$GW2IPName2    = "VNet2GWIP2"
	$GW2IPconf2    = "gw2ipconf2"
	$Connection21  = "VNet2toVNet1"
	$Connection12  = "VNet1toVNet2"

#### 2\. Criar TestVNet2 no novo grupo de recursos

	New-AzureRmResourceGroup -Name $RG2 -Location $Location2

	$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
	$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
	$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

	New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### 3\. Criar um gateway de VPN ativo-ativo para TestVNet2

Solicite dois endereços IP públicos para serem alocados ao gateway que você criará para sua VNet. Você também definirá as configurações de IP e sub-rede necessárias.

	$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
	$gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

	$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
	$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
	$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
	$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Crie o gateway de VPN com o número AS e o sinalizador "EnableActiveActiveFeature". Observe que você deve substituir o ASN padrão em seus gateways de VPN do Azure. As ASNs para as VNets conectadas devem ser diferentes para habilitar o BGP e roteamento de tráfego.

	New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### Etapa 2 - Conectar os gateways de TestVNet1 e TestVNet2

Neste exemplo, ambos os gateways estão na mesma assinatura. Você pode concluir essa etapa na mesma sessão do PowerShell.

#### 1\. Obter ambos os gateways

Certifique-se de fazer logon e se conectar à Assinatura 1.

	$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
	$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
	
#### 2\. Criar ambas as conexões

Nesta etapa, você criará a conexão de TestVNet1 para TestVNet2 e a conexão de TestVNet2 to TestVNet1.

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

	New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Certifique-se de habilitar o BGP para AMBAS as conexões.

Após concluir essas etapas, a conexão será estabelecida em alguns minutos e a sessão de emparelhamento via protocolo BGP estará em funcionamento quando a conexão VNet para VNet for concluída com redundância dupla:

![ativo-ativo-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Parte 4 – Atualizar o gateway existente entre ativo-ativo e ativo-em espera

A última seção descreverá como você pode configurar um gateway de VPN do Azure existente por meio do modo ativo-em espera ou ativo-ativo ou vice-versa.

>[AZURE.IMPORTANT] Observe que o modo ativo-ativo funciona somente no SKU de HighPerformance

### Configurar um gateway ativo-em espera para um gateway ativo-ativo

#### 1\. Parâmetros do gateway

O exemplo a seguir converte um gateway ativo-em espera em um gateway ativo-ativo. Você precisa criar outro endereço IP público e adicionar uma configuração do segundo IP do Gateway. Abaixo, encontram-se os parâmetros usados:

	$GWName     = "TestVNetAA1GW"
	$VNetName   = "TestVNetAA1"
	$RG         = "TestVPNActiveActive01"
	$GWIPName2  = "gwpip2"
	$GWIPconf2  = "gw1ipconf2"

	$vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
	$subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
	$gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
	$location   = $gw.Location

#### 2\. Crie o endereço IP público e adicione a configuração do segundo IP do Gateway

	$gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
	Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### 3\. Habilitar o modo ativo-ativo e atualizar o gateway

Você deve definir o objeto do gateway no PowerShell para disparar a atualização real.

	Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature

Esta atualização pode demorar de 30 a 45 minutos.

### Configurar um gateway ativo-ativo para um gateway ativo-em espera

#### 1\. Parâmetros do gateway

Usando os mesmos parâmetros acima, obtenha o nome da configuração de IP que você deseja remover.

	$GWName     = "TestVNetAA1GW"
	$RG         = "TestVPNActiveActive01"

	$gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
	$ipconfname = $gw.IpConfigurations[1].Name

#### 2\. Remover a configuração de IP do gateway e desabilitar o modo ativo-ativo

Da mesma forma, você deverá definir o objeto do gateway no PowerShell para disparar a atualização real.

	Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
	Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Esta atualização pode demorar até 30 a 45 minutos.


## Próximas etapas

Quando sua conexão for concluída, você poderá adicionar máquinas virtuais às suas redes virtuais. Veja [Criar uma máquina virtual](../virtual-machines/virtual-machines-windows-hero-tutorial.md) para obter as etapas.

<!---HONumber=AcomDC_0928_2016-->