<properties
	pageTitle="Dimensionar automaticamente um serviço de nuvem no portal | Microsoft Azure"
	description="Saiba como usar o portal para configurar regras de dimensionamento automático para uma função web ou função de trabalho do serviço de nuvem no Azure."
	services="cloud-services"
	documentationCenter=""
	authors="Thraka"
	manager="timlt"
	editor=""/>

<tags
	ms.service="cloud-services"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="adegeo"/>


# Como dimensionar automaticamente um serviço de nuvem

> [AZURE.SELECTOR]
- [Portal do Azure](cloud-services-how-to-scale-portal.md)
- [Portal clássico do Azure](cloud-services-how-to-scale.md)

As condições podem ser definidas para uma função de trabalho de serviço de nuvem que dispara uma operação para reduzir ou escalar horizontalmente. As condições para a função podem ser baseadas na CPU, no disco ou na carga de rede da função. Você também pode definir uma condição com base em uma fila de mensagens ou da métrica de algum outro recurso do Azure associado à sua assinatura.

>[AZURE.NOTE] Este artigo se concentra nas funções Web e de trabalho do Serviço de Nuvem. Ao criar uma máquina virtual (modelo clássico) diretamente, ela será hospedada em um serviço de nuvem. Você pode dimensionar uma máquina virtual padrão ao associá-la a um [conjunto de disponibilidade](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) e ligá-los ou desligá-los manualmente.

## Considerações

Você deve considerar as seguintes informações antes de configurar a colocação em escala do seu aplicativo:

- A colocação em escala é afetada pelo uso de núcleo. As instâncias de função maiores usam mais núcleos. Você só pode dimensionar um aplicativo dentro do limite de núcleos para sua assinatura. Por exemplo, se sua assinatura tem um limite de vinte núcleos e você executa um aplicativo com os dois serviços de nuvem de tamanho médio (um total de quatro núcleos), você só pode escalar verticalmente o dimensionamento de outras implantações do serviço de nuvem em sua assinatura em dezesseis núcleos. Para saber mais, confira [Tamanhos do Serviço de Nuvem](cloud-services-sizes-specs.md).

- Você pode dimensionar com base em um limite de mensagens da fila. Para obter mais informações sobre como usar as filas, confira [Como usar o serviço de Armazenamento de Filas](../storage/storage-dotnet-how-to-use-queues.md).

- Você também pode dimensionar outros recursos associados à sua assinatura.

- Para habilitar a alta disponibilidade do seu aplicativo, você deverá garantir que ele esteja implantado com duas ou mais instâncias de função. Para obter mais informações, consulte [Contratos de Nível de Serviço](https://azure.microsoft.com/support/legal/sla/).

## Onde a escala está localizada

Após selecionar o serviço de nuvem, a folha de serviço de nuvem deverá estar visível.

1. Na folha de serviço de nuvem, no bloco **Funções e Instâncias**, selecione o nome do serviço de nuvem. **IMPORTANTE**: certifique-se de clicar na função de serviço de nuvem, não na instância de função que está abaixo da função.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Selecione o bloco **escala**.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## Escala automática

Você pode definir as configurações de escala para uma função com o modo **manual** ou **automático**. O modo manual é a forma como você esperaria, você define a contagem absoluta de instâncias. No entanto, o modo automático permite que você defina regras que determinem como e quanto você deverá dimensionar.

Defina a opção **Dimensionar por** para as **regras de planejamento e desempenho**.

![Escala dos serviços de nuvem com perfil e regra](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Um perfil existente.
2. Adicione uma regra para o perfil pai.
3. Adicione outro perfil.

Selecione **Adicionar Perfil**. O perfil determina qual modo você deseja usar para a escala: **sempre**, **recorrência** ou **data fixa**.

Depois de configurar o perfil e as regras, selecione o ícone **Salvar** na parte superior.

#### Perfil

O perfil define as instâncias mínimas e máximas da escala, e também quando esse intervalo de escala estará ativo.

* **Sempre**

    Sempre mantenha esse intervalo de instâncias disponível.

    ![Serviço de nuvem que sempre dimensiona](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Recorrência**

    Escolha um conjunto de dias da semana para dimensionar.

    ![Escala de serviço de nuvem com um agendamento de recorrência](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Data Fixa**

    Um intervalo de datas fixo para dimensionar a função.

    ![Escala de serviço de nuvem com uma data fixa](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Depois de configurar o perfil, selecione o botão **OK** na parte inferior da folha de perfil.

#### Regra

As regras são adicionadas a um perfil e representam uma condição que disparará a escala.

O gatilho de regra se baseia em uma métrica do serviço de nuvem (utilização da CPU, atividade de disco ou atividade de rede) para a qual você pode adicionar um valor condicional. Além disso, o gatilho pode ser baseado em uma fila de mensagens ou na métrica de algum outro recurso do Azure associada à sua assinatura.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Depois de configurar a regra, selecione o botão **OK** na parte inferior da folha de regra.

## Volte à escala manual

Navegue até as [configurações de escala](#where-scale-is-located) e defina a opção **Dimensionar por** para **uma contagem de instâncias que inseri manualmente**.

![Escala dos serviços de nuvem com perfil e regra](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Isso remove a colocação em escala automatizada da função. Em seguida, você poderá definir a contagem de instâncias diretamente.

1. A opção escala (manual ou automática).
2. Um controle deslizante da instância de função para definir as instâncias para dimensionar.
3. Instâncias da função para dimensionar.

Depois de definir as configurações de escala, selecione o ícone **Salvar** na parte superior.

<!---HONumber=AcomDC_0608_2016-->