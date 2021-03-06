<properties 
	pageTitle="Recursos vinculados e relacionados na galeria de blocos" 
	description="Saiba mais sobre recursos relacionados e vinculados que são exibidos na galeria de blocos do portal de visualização do Azure." 
	services="azure-portal" 
	documentationCenter="" 
	authors="adamabdelhamed" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="azure-portal" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/16/2015" 
	ms.author="adamab"/>

# Recursos vinculados e relacionados na galeria de blocos

A galeria de blocos permite que você encontre blocos para um determinado recurso e arraste-os para sua folha atual. Usando a galeria de blocos, você pode criar exibições de gerenciamento que abrangem os recursos. Para qualquer recurso especificado, os recursos relacionados incluem todos os recursos que compartilham o mesmo grupo de recursos que o recurso e todos os recursos vinculados para ou por meio do recurso.

## Recursos vinculados no Gerenciador de Recursos do Azure

A vinculação é um recurso do Gerenciador de Recursos do Azure. Ela permite que você declare relações entre recursos mesmo se eles não residirem no mesmo grupo de recursos. A vinculação não tem impacto sobre o tempo de execução de seus recursos, sem afetar a cobrança e sem afetar o acesso baseado em função. Ela é simplesmente um mecanismo que você pode usar para representar as relações para que as ferramentas, como a galeria de blocos, possam fornecer uma experiência de gerenciamento avançada. As ferramentas podem inspecionar os links usando a API de links e fornecer experiências de gerenciamento de relacionamento personalizadas também.

## Como vinculo meus recursos?

Quando você criar recursos por meio do portal ou implantando um modelo por meio do Azure PowerShell ou CLI do Azure, os links são criados automaticamente para alguns recursos dependentes. Você também pode vincular programaticamente os recursos usando a [API REST de recursos vinculados](https://msdn.microsoft.com/library/azure/mt238499.aspx) ou declarando as relações no modelo. Para obter uma discussão completa sobre como trabalhar com recursos vinculados, consulte [Vinculação de recursos no Gerenciador de Recursos do Azure](../resource-group-link-resources.md).

## Próximas etapas

- Se você precisar de uma introdução à criação de modelos do Gerenciador de Recursos do Azure, consulte [Criação de modelos](../resource-group-authoring-templates.md).
- Para obter mais detalhes sobre como criar links entre recursos, consulte [Vinculação de recursos no Gerenciador de Recursos do Azure](../resource-group-link-resources.md).
- Para saber mais sobre como trabalhar com grupos de recursos por meio do portal de visualização, consulte [Usando o Portal de visualização do Azure para gerenciar seus recursos do Azure](resource-group-portal.md).

<!---HONumber=Oct15_HO3-->