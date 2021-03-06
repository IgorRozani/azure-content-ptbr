<properties
   pageTitle="Entendendo sua fatura | Microsoft Azure"
   description="Saiba como ler e entender o uso e a fatura da sua assinatura do Azure"
   services=""
   documentationCenter=""
   authors="erihur"
   manager="stevenpo"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="erihur;genli"/>


# Entenda sua fatura do Microsoft Azure

> [AZURE.NOTE] Se você precisar de mais ajuda a qualquer momento neste artigo, [contate o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver seu problema rapidamente.

Os encargos das assinaturas do Microsoft Azure variam de acordo com o plano de tarifas. Alguns planos de tarifas, como as assinaturas do MPN (Visual Studio Enterprise), incluem créditos mensais que podem ser usados em qualquer serviço do Azure com base em suas necessidades.

Observe que até 24 horas do uso latente do seu período de cobrança anterior podem ser registradas em seu período atual de cobrança.

Para obter mais informações sobre os planos de taxa e consumo, confira a [página Opções de Compra do Microsoft Azure](https://azure.microsoft.com/pricing/purchase-options/).

<!-- The below links cover a complete list of all Microsoft Azure services.

<!-- - [Service Details list (csv1)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
<!-- - [Service Details list (csv2)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)

<!-- *NOTE: The **csv1** link refers to the column header names for csv version 1 and **csv2** link refers to the new column header names for csv version 2.  These files are updated monthly.*-->

### Exibir ou baixar uma fatura do Microsoft Azure:

1. Entre no [Centro de Contas](https://account.windowsazure.com/subscriptions) usando sua Conta da Microsoft ou ID Organizacional.

2. Clique na assinatura na qual você gostaria de ver os detalhes e o uso.

3. Clique em **Histórico de Cobrança**

    ![Resumo - histórico de cobrança -1](./media/billing-understand-your-bill/ContentViewaBillforMA1.png)

4. A seção **Histórico de Cobrança** lista os demonstrativos dos períodos de cobrança anteriores, além do período não cobrado atual. O demonstrativo do período atual é uma estimativa dos seus encargos no momento em que a estimativa foi gerada. Essas informações são atualizadas diariamente e talvez não incluam todo o uso incorrido até o momento. Sua fatura mensal pode ser diferente dessa estimativa.

    ![Resumo - histórico de cobrança 2](./media/billing-understand-your-bill/ContentViewaBillforMA2.png)

5. Clique em **Exibir Demonstrativo Atual** para exibir uma estimativa dos seus encargos no momento em que a estimativa foi gerada. Essas informações são atualizadas diariamente e talvez não incluam todo o uso incorrido até o momento. Sua fatura mensal pode ser diferente dessa estimativa.

    ![Resumo - histórico de cobrança 3](./media/billing-understand-your-bill/ContentViewaBillforMA3.png)

    ![Resumo - histórico de cobrança 4](./media/billing-understand-your-bill/ContentViewaBillforMA4.png)

6. Clique em **Baixar Fatura** para exibir uma cópia da sua fatura anterior.

    ![Resumo -histórico de cobrança 5](./media/billing-understand-your-bill/ContentViewaBillforMA5.png)


> [AZURE.NOTE] Os encargos listados nos demonstrativos de cobrança para clientes internacionais servem somente para fins de estimativa, já que os bancos têm custos diferentes para as taxas de conversão.

Veja a seguir algumas instruções de exemplo para duas ofertas diferentes disponíveis no Microsoft Azure.

 Tipo de oferta | Descrição | Baixar |
 :--------- |:-------- | :-------|
Pré-paga | Pagar com atraso de pagamento mensal | [Arquivo de exemplo](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_ccinvoice_Sample.pdf)
Oferta de compromisso | Gastar com dedução do seu compromisso pré-pago | [Arquivo de exemplo](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_invoice_Sample.pdf)

## Informações da conta

A seção de informações da conta identifica informações pertinentes relacionadas ao seu uso e perfil.

![cabeçalho](./media/billing-understand-your-bill/Header.png)

| Termo | Descrição |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Nº da Fatura | Um identificador de fatura único para fins de acompanhamento |
| Ciclo de cobrança | O intervalo de tempo em que ocorreu o uso |
| Data da fatura | A data em que a fatura foi gerada |
| Método de pagamento | Tipo de pagamento usado na conta (fatura ou cartão de crédito) |
| Enviar cobrança para | Endereço de pagamentos do Microsoft Azure |
| Oferta de assinatura | Tipo de oferta de assinatura que tiver sido adquirida (Pré-pago, BizSpark Plus, Azure Pass etc.) |
| Email do proprietário da conta | O endereço de email da conta no qual a conta do Microsoft Azure está registrada |

## Entender o resumo da fatura

A seção **Resumo da Fatura** da fatura resume as transações desde sua última fatura e os encargos de uso atuais.

![resumo da fatura](./media/billing-understand-your-bill/InvoiceSummary.png)

A seção saldo anterior, pagamentos e saldo pendente da fatura resume as transações desde sua última fatura.

| Termo | Descrição |
|---------------------------------------------------|------------------------------------------------------------------------------------------|
| Saldo anterior | O valor total devido em sua última fatura |
| Pagamentos | Os pagamentos totais aplicados à sua última fatura |
| Saldo pendente (do ciclo de cobrança anterior) | Qualquer ajuste de fatura (créditos ou saldos) aplicado à sua conta desde sua última fatura |

## Entender os encargos atuais
A seção Encargos Atuais da fatura contém detalhes sobre seus encargos mensais. Os links são organizados nas subseções a seguir.

| Termo | Descrição |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Encargos de uso | Encargos de uso são o total de encargos mensais em uma assinatura. Você é cobrado com atraso de pagamento para uso do seu mês anterior. |
| Descontos | Descontos de serviço aplicados à sua fatura atual são refletidos nesse item de linha. |
| Ajustes | Ajustes diversos são créditos diversos ou encargos pendentes aplicados à sua fatura atual. Por exemplo, se você tivesse a oferta Visual Studio Enterprise com MSDN, veria um crédito mensal neste item de linha. Se cancelar sua assinatura, você verá encargos para o uso mensal que excede o crédito mensal incluído na sua oferta desde o início do período de cobrança atual até a data de cancelamento da assinatura.|

## Informações do rodapé
![rodapé](./media/billing-understand-your-bill/footerinformation.png)

## Entender as informações adicionais
A página de informações adicionais fornece referências a outros recursos para explicar sua fatura e links para exibir seu uso, bem como outras informações relevantes da sua fatura.

![informações adicionais](./media/billing-understand-your-bill/AdditionalInformation.png)

### Uso detalhado
Um link na descrição em **Uso Detalhado** direciona você para o Centro de Contas, em que é possível exibir o uso detalhado da assinatura. Agora há duas versões disponíveis para download: **. csv versão 1** contém os campos de uso e a antiga convenção de nomenclatura e **. csv versão 2** contém nomes amigáveis de cliente para cada uma das categorias mais campos adicionais que ajudam você a compreender quais serviços está usando no Microsoft Azure. Observe que no. csv versão 1 que não há detalhe algum do Azure Resource Manager. Informações do Azure Resource Manager podem ser encontradas no. csv versão 2.

### Informações adicionais e recursos úteis
Esta seção contém links para perguntas simples sobre tamanhos de instância de computação, encargos de Banco de Dados SQL e links úteis para ajudá-lo a responder a perguntas adicionais.

| Termo | Descrição |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Vendido para | Isso é pré-preenchido com o endereço do perfil na conta |
| Instruções de pagamento | Esta seção é composta pelas instruções de pagamento. Por exemplo, para onde enviar cheques, transferências eletrônicas ou pagamentos em 24 horas se o método de pagamento for fatura |

## Entender os encargos de uso detalhados

Como parte do nosso compromisso contínuo para ajudar os clientes a gerenciar facilmente o uso do Azure, aprimoramos o arquivo de uso de download com os relatórios sobre o uso dos serviços do Azure e os custos. O link de download contém duas versões do arquivo de uso:

- A **Versão 1** usa o formato preexistente

- A **Versão 2** inclui informações adicionais e nomes de coluna atualizadas na seção Uso Diário.

Encargos de uso são o total de encargos **mensais** em uma assinatura, menos créditos ou descontos. Você é cobrado com atraso de pagamento para uso do seu mês anterior. A seção superior do arquivo exibe os detalhes sobre os serviços que estão sendo cobrados durante o ciclo de cobrança do mês anterior. A tabela anterior lista os nomes das colunas para cada um dos arquivos da versão .csv.

Versão 1 | Versão 2 | Descrição|
:---------------| :---------------- | --------|
Período de Cobrança | Período de Cobrança | O período de cobrança quando o recurso foi consumido.
Nome | Categoria de medidor | Identifica o serviço de nível superior ao qual esse uso pertence.
Tipo | Subcategoria de medidor | O serviço do Azure pode ser ainda mais definido pelo tipo nesta coluna, que pode afetar a tarifa.
Recurso | Nome do medidor | Identifica a unidade de medida para o recurso sendo consumido.
Região | Região do medidor | Identifica o local do datacenter para determinados serviços que são cobrados com base no local do datacenter.
SKU | SKU | Identifica o identificador de sistema exclusivo para cada recurso do Azure.
Unidade | Unidade | Identifica a unidade em que o serviço é cobrado. Por exemplo, GB, horas, 10.000s.
Consumido | Quantidade consumida | Contém a quantidade do recurso que foi consumida durante o período de cobrança.
Incluso | Quantidade incluída | Contém a quantidade do recurso que está incluída gratuitamente em seu período de cobrança atual.
Faturável | Quantidade de excesso | Se a quantidade consumida exceder a quantidade incluída, esta coluna exibirá a diferença. Você é cobrado por essa quantidade. Para ofertas pré-pagas sem quantidade incluída na oferta, esse total será igual à quantidade consumida.
Dentro do Compromisso | Dentro do Compromisso | Contém os encargos do recurso que são reduzidos do valor de compromisso associado à sua oferta de 6 ou 12 meses. Os encargos de recurso são reduzidos do valor de compromisso em ordem cronológica.
Moeda | Moeda | Identifica a moeda refletida no período de cobrança atual.
Excedente | Excedente | Contém os encargos de recurso que excedem o valor de compromisso associado à sua oferta de 6 ou 12 meses.
Tarifa de Compromisso | Tarifa de Compromisso | Contém a tarifa de compromisso com base no valor de compromisso total associado à sua oferta de 6 ou 12 meses.
Tarifa | Tarifa | Tarifa exibe a tarifa com a qual você é cobrado por unidade faturável.
Valor | Valor | Exibe o resultado da multiplicação da coluna Faturável pela coluna Tarifa. Se a quantidade consumida não exceder a quantidade incluída, não haverá cobrança nessa coluna.

## Analisar os dados de uso diário
Dependendo de seu uso, pode haver milhares de linhas de dados de uso diário. Se você quiser analisar esses dados, clique em **Baixar uso** e escolha uma versão de arquivo de variáveis separado por vírgulas (. csv) para ver seus dados de uso diário do período de faturamento apropriado. Para referência, você pode baixar um arquivo. csv de exemplo para cada versão abaixo.

 Nome | Baixar |
 :----------:| :-------: |
 Uso detalhado. csv Versão 1| [Arquivo de exemplo](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
 Uso detalhado. csv Versão 2 | [Arquivo de exemplo](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)



![csv2screenshot](./media/billing-understand-your-bill/csv2screenshot.png)



No arquivo .csv, os itens são divididos para exibir uma lista de quanto de cada recurso foi consumido dentro do período de cobrança atual.

![instantâneo de csv](./media/billing-understand-your-bill/csvsnapshotportal.png)

As colunas a seguir exibem detalhes que afetam as tarifas no início do período de cobrança:

Versão 1 | Versão 2 | Descrição |
:---------------| :----------------| -----|
Data de Uso | Data de Uso | A data em que o recurso foi emitido.
Nome | Categoria de medidor | Identifica o serviço de nível superior ao qual esse uso pertence.
GUID de recurso | ID de medidor | O identificador de medidor cobrado. Isso é usado como o identificador usado para cobrança do uso de preços.
Tipo | Subcategoria de medidor | O serviço do Azure pode ser ainda mais definido pelo tipo nesta coluna, que pode afetar a tarifa.
Recurso | Nome do medidor | Identifica a unidade de medida para o recurso sendo consumido.
Região | Região do medidor | Identifica o local do datacenter para determinados serviços que são cobrados com base no local do datacenter.
Unidade | Unidade | Identifica a unidade em que o serviço é cobrado. Por exemplo, GB, horas, 10.000s.
Consumido | Quantidade consumida | Contém a quantidade do recurso que foi consumida naquele dia.
Sub-região | Local do recurso | Identifica o datacenter onde o recurso está sendo executado.
O Barramento de | Serviço consumido | Esta coluna é utilizada para controlar o serviço da plataforma do Azure individual que não pode ser especificamente identificado na coluna Nome. Essa coluna Serviço indica a qual serviço específico o uso pertence.
N/D | Grupo de recursos | _**Adição de nova coluna.**_ O grupo de recursos no qual o recurso implantado está sendo executado. Confira [Visão geral do Azure Resource Manager](resource-group-overview.md)
Componente | ID da instância | O identificador do recurso em execução. O identificador contém o nome especificado para o recurso quando ele foi criado.
N/D | Marcas | _**Adição de nova coluna.**_ Novos tipos de recursos no Azure permitem recursos de marca. Confira [Organize your Azure resources with tags (Organizar recursos do Azure com marcações)](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/)
Informações Adicionais | Informações Adicionais | Metadados adicionais relacionados ao serviço.
Informações do Serviço 1 | Informações do Serviço 1 | Essa coluna fornece o nome do projeto ao qual o serviço pertence na sua assinatura.
Informações do Serviço 2 | Informações do Serviço 2 | Esse é um campo herdado que captura os metadados específicos do serviço opcional.

Além de alguns novos campos e as alterações de nome do arquivo csv versão 2, haverá uma formatação padronizada para os dados nos campos abaixo:

- **ID da instância**: o campo ID de instância representa o identificador especificado pelo usuário para o serviço provisionado. Atualmente, há dois formatos em que a ID de instância é representada: é o nome do recurso ou ID do recurso totalmente qualificado. Os serviços do Microsoft Azure estão fazendo a transição para representar a ID de Instância em um formato padronizado da ID do Recurso totalmente qualificado _**(/subscriptions/<iddaassinatura>/resourcegroups/<nomedogrupoderecursos>/providers/<nomedoprovedor>/<nomedorecurso>)**_. Durante a transição para o novo formato de serviços você verá o campo de dados da ID da instância mudar de apenas o nome do recurso para a ID do recurso. A ID do Recurso é o formato usado pela [API do Azure Resource Manager](https://msdn.microsoft.com/library/azure/dn790567.aspx) para identificar recursos em um uma assinatura.

![instanceId](./media/billing-understand-your-bill/instanceid.png)

- **Informações adicionais**: a coluna Informações Adicionais no .csv de uso especifica os metadados específicos do serviço. Por exemplo, um tipo de imagem para uma VM. Atualmente, um serviço emite metadados específicos do serviço em várias colunas: campos Informações Adicionais, Informações do Serviço 1 e Informações do Serviço 2. Os serviços do Microsoft Azure vão padronizar a emissão de metadados específicos do serviço somente na coluna Informações Adicionais. Consulte o instantâneo abaixo do formato padronizado:

![additionalinfo\_csv2](./media/billing-understand-your-bill/AdditionaInfo_csv2.png)

- **Marcas**: Esta coluna contém as marcas de recurso especificadas pelo usuário. As marcas podem ser usadas para agrupar registros de cobrança. Por exemplo, você pode usar marcas para distribuir os custos por departamento usando o serviço. Saiba mais sobre [Usando marcas para organizar os recursos do Azure](./resource-group-using-tags.md). Serviços que oferecem suporte a marcas de emissão são:

    - Máquinas Virtuais

    - Armazenamento e

    - Serviços de rede fornecidos com o [API do Gerenciador de Recursos do Azure](https://msdn.microsoft.com/library/azure/dn790567.aspx)

![marcas](./media/billing-understand-your-bill/tags.png)


## Próximas etapas

- [Configurar alertas de cobrança](billing-set-up-alerts.md)

- [Manage your payment methods (Gerenciar seus métodos de pagamento)](billing-how-to-change-credit-card.md)

- [Entenda os encargos do Azure Marketplace](billing-understand-your-azure-marketplace-charges.md)

- [Perguntas frequentes sobre cobrança e assinatura do Azure](billing-subscription-faq.md)

> [AZURE.NOTE] Se ainda tiver dúvidas, [contate o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver seu problema rapidamente.

<!--
OLD MSDN Articles
- [What do I do if my Azure subscription become disabled?](https://msdn.microsoft.com/library/azure/dn736049.aspx)
- [Edit payment information for an existing credit card](https://msdn.microsoft.com/library/azure/dn736053.aspx)
- [Add a new credit card to use as a payment method](https://msdn.microsoft.com/library/azure/dn736057.aspx)
- [Change the credit card on your Microsoft Azure account](https://msdn.microsoft.com/library/azure/dn736050.aspx)
- [Manage your payment method](https://msdn.microsoft.com/library/azure/dn736054.aspx)
-->



<!--Image references-->

<!---HONumber=AcomDC_0914_2016-->