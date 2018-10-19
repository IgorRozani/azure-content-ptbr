<properties
   pageTitle="Tutorial principal 1 de DotNet da máquina virtual do Azure | Microsoft Azure"
   description="Tutorial principal de DotNet da máquina virtual do Azure"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# Automatização das implantações de aplicativos em máquinas virtuais do Azure

Esta série de quatro partes detalha como implantar e configurar recursos e aplicativos do Azure que usam modelos do Azure Resource Manager. Nesta série, um exemplo de modelo é implantado e o modelo de implantação é examinado. O objetivo desta série é orientar na relação entre os recursos do Azure e fornecer experiência prática de implantação de modelos do Azure Resource Manager totalmente integrados. Este documento presume um nível básico de conhecimento com o Azure Resource Manager. Antes de iniciar este tutorial, familiarize-se com os conceitos básicos do Azure Resource Manager.

## Aplicativo de loja de música

O exemplo usado nesta série é um aplicativo .Net Core simulando uma experiência de compra na Loja de Música. Esse aplicativo pode ser implantado em um sistema virtual Windows ou Linux, implantações de exemplo foram criadas para ambos. O aplicativo inclui um aplicativo Web e um banco de dados SQL. Antes de ler os artigos desta série, implante o aplicativo usando o botão de implantação encontrado nesta página. Quando totalmente implantada, a arquitetura do aplicativo/Azure se parece com o diagrama a seguir.

O modelo do Resource Manager da Loja de Música pode ser encontrado aqui, [Modelos Linux da Loja de Música](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db)

![Aplicativo de loja de música](./media/virtual-machines-linux-dotnet-core/music-store.png)

Cada um desses componentes, incluindo o modelo associado JSON é examinado nos quatro artigos a seguir.

- [**Arquitetura do aplicativo**](./virtual-machines-linux-dotnet-core-2-architecture.md) – Componentes de aplicativos, como sites da Web e bancos de dados precisam ser hospedados nos recursos do computador do Azure como máquinas virtuais e bancos de dados SQL do Azure. Este documento orienta sobre a necessidade de computação de mapeamento, para recursos do Azure e a implantação desses recursos com um modelo do Azure Resource Manager.

- [**Acesso e segurança**](./virtual-machines-linux-dotnet-core-3-access-security.md) – Para hospedar aplicativos no Azure, é necessário considerar como o aplicativo é acessado e como é o acesso de componentes de aplicativo diferentes entre si. Este documento detalha como fornecer e proteger o acesso via Internet a um aplicativo e o acesso entre componentes de aplicativos.

- [**Disponibilidade e escala**](./virtual-machines-linux-dotnet-core-4-avalibility-scale.md) – Referem-se à capacidade dos aplicativos de permanecer em execução durante o tempo de inatividade da infraestrutura e à capacidade de dimensionar os recursos de computação para atender à demanda do aplicativo. Este documento detalha os componentes necessários para implantar um aplicativo com balanceamento de carga e altamente disponível.

- [**Implantação de aplicativos**](./virtual-machines-linux-dotnet-core-5-app-deployment.md) – Ao implantar aplicativos em máquinas virtuais do Azure, o método pelo qual os binários do aplicativo são instalados na máquina virtual deve ser considerado. Este documento detalha como automatizar a instalação de aplicativos usando extensões de script personalizado da máquina virtual do Azure.

A meta ao desenvolver modelos do Azure Resource Manager é automatizar a implantação da infraestrutura do Azure e a instalação e configuração de todos os aplicativos que estão sendo hospedados nessa infraestrutura do Azure. Trabalhar com estes artigos fornece um exemplo dessa experiência.

## Implantar o aplicativo de loja de música

O aplicativo Loja de Música pode ser implantado com este botão.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-linux%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

O modelo do Azure Resource Manager requer os seguintes valores de parâmetro.

|Nome do Parâmetro |Descrição |
|---|---|
|SSHKEYDATA | Dados da chave SSH usados para proteger o acesso à máquina virtual. Para obter informações sobre como criar uma chave aérea SSH, consulte [Criação de chaves SSH para VMs do Linux no Azure](./ virtual-machines-linux-mac-create-ssh-keys.md). |
|ADMINUSERNAME | Nome de usuário de administrador usado na máquina virtual e o Banco de Dados SQL do Azure. |
|SQLADMINPASSWORD | Senha usada no banco de dados SQL do Azure. |
|NUMBEROFINSTANCES | O número de máquinas virtuais a serem criadas. Cada uma dessas máquinas virtuais hospeda o aplicativo Web da Loja de Música, e todo o tráfego tem carga balanceada entre elas. |
|PUBLICIPADDRESSDNSNAME | Nome DNS globalmente exclusivo associado ao endereço IP público. |

Quando a implantação do modelos for concluída, navegue até o endereço IP público usando qualquer navegador da Internet. O site .Net Core Music será apresentado.

## Próximas etapas

<hr>

[Etapa 1 – Arquitetura de aplicativos com modelos do Azure Resource Manager](./virtual-machines-linux-dotnet-core-2-architecture.md)

[Etapa 2 – Acesso e segurança em modelos do Azure Resource Manager](./virtual-machines-linux-dotnet-core-3-access-security.md)

[Etapa 3 – Disponibilidade e escala em modelos do Azure Resource Manager](./virtual-machines-linux-dotnet-core-4-avalibility-scale.md)

[Etapa 4 – Implantação de aplicativos com modelos do Azure Resource Manager](./virtual-machines-linux-dotnet-core-5-app-deployment.md)

<!---HONumber=AcomDC_0928_2016-->