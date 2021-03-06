<properties
	pageTitle="Registro de dispositivo automático com o Active Directory do Azure para dispositivos de domínio associado do Windows| Microsoft Azure"
	description="Administradores de TI podem optar para que seus dispositivos Windows ingressados no domínio registrem-se automaticamente e silenciosamente com o AD do Azure (Active Directory do Azure)."
	services="active-directory"
	documentationCenter=""
	authors="Markvi"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/27/2016"
	ms.author="femila"/>

# Registro de dispositivo automático com o Azure Active Directory para dispositivos ingressados no domínio do Windows

Como um administrador de TI, você pode escolher registrar automaticamente e silenciosamente seus dispositivos do Windows ingressados no domínio com o AD do Azure (Active Directory do Azure). Isso pode ser útil se você tiver configurado acesso condicional do dispositivo com base em políticas para aplicativos do Office365 ou aplicativos gerenciados localmente pelo AD FS. Você pode aprender mais sobre os cenários de registro de dispositivo lendo a [Visão geral de registro de dispositivo do Active Directory do Azure](active-directory-conditional-access-device-registration-overview.md).

>AZURE.OBSERVAÇÃO Para obter instruções mais recentes sobre como configurar o registro automático de dispositivos, veja [Como configurar o registro automático de domínio do Windows associado a dispositivos com o Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

O Registro Automático de Dispositivo com o Active Directory do Azure está disponível para computadores com Windows 7 e Windows 8.1 que ingressaram em um domínio do Active Directory. Esses computadores são normalmente máquinas corporativas fornecidas para os operadores de informações.

Para começar a registrar seus dispositivos do Windows ingressados no domínio com o AD do Azure, siga os pré-requisitos a seguir. Depois de concluir os pré-requisitos, configure o registro de dispositivos automático para os dispositivos do Windows ingressados no seu domínio.

## Pré-requisitos para registro automático de dispositivo de dispositivos do Windows ingressados no domínio com o Azure Active Directory

Implantar o AD FS e conectar ao Active Directory do Azure usando o Azure Active Directory Connect
----------------------------------------------------------------------------------------------
1. Use o Azure Active Directory Connect para implantar os AD FS (Serviços de Federação do Active Directory) com o Windows Server 2012 R2 e configurar uma relação de federação com o Active Directory do Azure.
2. Configure uma regra de declaração de objeto de confiança de terceira parte confiável adicional do Active Directory do Azure.
3. Abra o console de gerenciamento do AD FS e navegue até **AD FS**>**Relações de Confiança > Objetos de confiança de terceira parte confiável**. Clique com o botão direito do mouse no objeto de confiança de terceira parte confiável da Plataforma de Identidade do Microsoft Office 365 e selecione **Editar Regras de Declaração...**
4. Na guia **Regras de Transformação de Emissão**, selecione **Adicionar Regra**.
5. Selecione **Enviar declarações usando uma regra personalizada** na caixa suspensa do modelo **Regra de declaração**. Selecione **Avançar**.
6. Digite *Regra de Declaração de Método de Autenticação* na caixa de texto **Nome da regra de declaração:**.
7. Digite a regra de declaração a seguir na caixa de texto: **Regra de declaração:**

        c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
        => issue(claim = c);

8. Clique em **OK** duas vezes para concluir a caixa de diálogo.

Configurar uma Referência de Classe de Autenticação de objeto de confiança de terceira parte confiável do Active Directory do Azure
-----------------------------------------------------------------------------------------------------
No servidor de federação, abra uma janela de comando do Windows PowerShell e digite:


  `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

Em que <NomeDoObjetoRPO> é o nome do objeto de terceira parte confiável para seu objeto de confiança de terceira parte confiável do Azure Active Directory. Esse objeto é tipicamente nomeado Plataforma de Identidade do Microsoft Office 365.

Política de Autenticação Global do AD FS
-----------------------------------------------------------------------------
Configure a Política de Autenticação Primária Global do AD FS para permitir a Autenticação Integrada do Windows para a Intranet (esse é o padrão).


Configuração do Internet Explorer
------------------------------------------------------------------------------
Defina as seguintes configurações no Internet Explorer em seus dispositivos do Windows para a zona de segurança de intranet local:

- Não solicite a seleção de certificado do cliente quando existe apenas um certificado: **Habilitar**
- Permitir scripts: **Habilitar**
- Logon automático somente na zona da Intranet: **marcado**

Essas são as configurações padrão para a zona de segurança de intranet Local do Internet Explorer. Você pode exibir ou gerenciar essas configurações no Internet Explorer navegando até **Opções da Internet** > **Segurança** > Intranet Local > Nível personalizado. Você também pode definir essas configurações usando a Política de Grupo do Active Directory.

Conectividade de rede
-------------------------------------------------------------
Os dispositivos Windows ingressados devem ter conectividade com o AD FS e um Controlador de Domínio do Active Directory para registrar-se automaticamente com o AD do Azure. Isso normalmente significa que o computador deve estar conectado à rede corporativa. Isso pode incluir uma conexão com fio, uma conexão Wi-Fi, DirectAccess ou VPN.

## Configurar a descoberta de registro de dispositivos do Active Directory do Azure
Os dispositivos com Windows 7 e Windows 8.1 vão descobrir o servidor de registro de dispositivo combinando o nome da conta de usuário com um nome do servidor de registro de dispositivos conhecido. Você deve criar um registro DNS CNAME que aponta para o registro A associado com o serviço de registro de dispositivo do Active Directory do Azure. O registro CNAME deve usar o prefixo **enterpriseregistration** conhecido seguido pelo sufixo UPN usado pelas contas de usuário em sua organização. Se sua organização usa vários sufixos UPN, vários registros CNAME devem ser criados no DNS.

Por exemplo, se você usar dois sufixos UPN na sua organização chamada @contoso.com e @region.contoso.com, você criará os seguintes registros DNS.

| Entrada | Tipo | Endereço |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com | CNAME | enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com | CNAME | enterpriseregistration.windows.net |

##Configurar o Registro Automático de Dispositivo para dispositivos ingressados no domínio do Windows 7 e Windows 8.1

Configure o registro automático de dispositivo para os dispositivos ingressados no domínio com Windows 7 e Windows 8.1 usando os links abaixo. Certifique-se de que você concluiu os pré-requisitos acima antes de continuar.

* [Configurar o Registro Automático de Dispositivo para dispositivos ingressados no domínio do Windows 8.1](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)

* [Configurar o Registro Automático de Dispositivo para dispositivos ingressados no domínio do Windows 7](active-directory-conditional-access-automatic-device-registration-windows7.md)

* [Registro de dispositivo automático com o Azure Active Directory para Dispositivos Ingressados no Domínio do Windows 10](active-directory-azureadjoin-devices-group-policy.md)

Observações Adicionais
--------------------------------------------------------------------

O registro de dispositivos com o AD do Azure fornece o conjunto mais amplo de recursos de dispositivo. Com o Registro de Dispositivo do AD do Azure, você pode registrar tanto dispositivos móveis pessoais (BYOD) quando de propriedade da empresa, ambos dispositivos ingressados no domínio. Os dispositivos podem ser usados com os dois serviços hospedados tais como o Office365 e serviços gerenciados locais com o AD FS.

Empresas que usam ambos os dispositivos móveis e tradicionais ou que usam o Office365, AD do Azure, ou outros serviços da Microsoft devem registrar os dispositivos no AD do Azure usando o serviço de Registro de Dispositivo do AD do Azure. Se sua empresa não usar dispositivos móveis, nem usar nenhum serviço da Microsoft como o Office365, o AD do Azure ou o Microsoft Intune e hospedar somente aplicativos locais, você pode optar por registrar dispositivos no Active Directory usando o AD FS.

Você pode saber mais sobre como implantar o registro de dispositivos com o AD FS [aqui](https://technet.microsoft.com/library/dn486831.aspx).

## Tópicos adicionais

- [Visão geral do registro de dispositivos do Active Directory do Azure](active-directory-conditional-access-device-registration-overview.md)
- [Configurar o registro automático de dispositivo para dispositivos ingressados no domínio do Windows 7](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Configurar o registro automático de dispositivo para dispositivos ingressados no domínio do Windows 8.1](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Registro de dispositivo automático com o Azure Active Directory para dispositivos ingressados no domínio do Windows 10](active-directory-azureadjoin-devices-group-policy.md)

<!---HONumber=AcomDC_0928_2016-->