<properties 
	pageTitle="Tutorial: criar um pipeline usando o Assistente de Cópia" 
	description="Neste tutorial, você cria um pipeline do Azure Data Factory com uma Atividade de Cópia usando o Assistente de Cópia com suporte do Data Factory" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="09/16/2016" 
	ms.author="spelluru"/>

# Tutorial: Criar um pipeline com a Atividade de Cópia usando o Assistente de Cópia do Data Factory
> [AZURE.SELECTOR]
- [Visão geral e pré-requisitos](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistente de Cópia](data-factory-copy-data-wizard-tutorial.md)
- [Portal do Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [API do .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


O **Assistente de Cópia** do Azure Data Factory permite que você crie um pipeline para implementar o cenário de ingestão/movimentação de dados de forma rápida e fácil. Portanto, recomendamos que você use o assistente como uma primeira etapa para criar um pipeline de exemplo no cenário de movimentação de dados. Este tutorial mostra como criar um Azure Data Factory, iniciar o Assistente de Cópia, seguir uma série de etapas para fornecer detalhes sobre seu cenário de ingestão/movimentação de dados. Quando você concluir as etapas no assistente, ele criará um pipeline com Atividade de Cópia a fim de copiar dados de um armazenamento de blobs do Azure para um banco de dados SQL do Azure automaticamente. Veja o artigo [Atividades de movimentação de dados](data-factory-data-movement-activities.md) para obter detalhes sobre a Atividade de Cópia.

> [AZURE.IMPORTANT] Percorra o artigo [Visão geral e pré-requisitos do tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) para ter uma visão geral do tutorial e concluir as etapas de **pré-requisito** antes de executar este tutorial.

## Criar um data factory
Nesta etapa, você usa o Portal do Azure para criar um data factory do Azure denominado **ADFTutorialDataFactory**.

1.	Depois de fazer logon no [portal do Azure](https://portal.azure.com), clique em **+NOVO**, clique em **Intelligence + Analytics** e clique em **Data Factory**.

	![Novo -> DataFactory](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)

6. Na folha **Nova data factory**:
	1. Digite **ADFTutorialDataFactory** para o **nome**. O nome da data factory do Azure deve ser globalmente exclusivo. Se você receber o erro: **O nome da data factory "ADFTutorialDataFactory" não está disponível**, altere o nome da data factory (por exemplo, yournameADFTutorialDataFactory) e tente criá-la novamente. Consulte o tópico [Data Factory - regras de nomenclatura](data-factory-naming-rules.md) para ver as regras de nomenclatura para artefatos de Data Factory.
	 
		![Nome da data factory indisponível](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
	
		> [AZURE.NOTE] O nome do data factory pode ser registrado futuramente como um nome DNS e tornar-se publicamente visível.
	2. Selecione sua **assinatura** do Azure.
	3. Em relação ao Grupo de Recursos, execute uma das seguintes etapas:
		1. Selecione **Usar existente** para selecionar um grupo de recursos existente.
		2. Selecione **Criar novo** e insira um nome para um grupo de recursos.

			Algumas das etapas neste tutorial supõem que você usa o nome: **ADFTutorialResourceGroup** para o grupo de recursos. Para saber mais sobre grupos de recursos, consulte [Usando grupos de recursos para gerenciar recursos do Azure](../resource-group-overview.md).
	3. Selecione um **local** para o data factory.
	4. Marque a caixa de seleção **Fixar no painel** na parte inferior da folha.
	5. Clique em **Criar**.
	
		![Folha Nova data factory](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)
10. Depois que a criação for concluída, você verá a folha **Data Factory**, conforme mostrado na seguinte imagem:

    ![Página inicial da data factory](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## Iniciar e usar o Assistente de Cópia

1. Na home page do Data Factory, clique no bloco **Copiar dados** para iniciar o **Assistente de Cópia**.

	> [AZURE.NOTE] Se você vir que o navegador da Web está bloqueado em "Autorizando...", desabilite/desmarque a configuração **Bloquear cookies de terceiros e dados de site** (ou) mantenha-a habilitada, crie uma exceção para **login.microsoftonline.com** e tente iniciar o assistente novamente.
2. Na página **Propriedades**:
	1. Insira **CopyFromBlobToAzureSql** para o **Nome da tarefa**
	2. Insira uma **descrição** (opcional).
	3. Altere a **data/hora inicial** e a **data/hora de término** para que a data de término seja definida como a data de hoje e a de início, cinco dias antes da data de hoje.
	3. Clique em **Avançar**.

	![Ferramenta de Cópia - página Propriedades](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png)
3. Na página **Repositório de dados de origem**, clique no bloco **Armazenamento de Blobs do Azure**. Use essa página para especificar o repositório de dados de origem para a tarefa de cópia. Você pode usar um serviço de repositório de dados vinculado existente ou especificar um novo repositório de dados. Para usar um serviço vinculado existente, clique em **DE SERVIÇOS VINCULADOS EXISTENTES** e selecione o serviço vinculado correto.

	![Ferramenta de Cópia - página de repositório de dados de origem](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
5. Na página **Especificar a conta de armazenamento de Blobs do Azure**:
	1. Insira **AzureStorageLinkedService** para o **Nome do serviço vinculado**.
	2. Confirme se a opção **De assinaturas do Azure** foi selecionada em **Método de seleção de conta**.
	3. Selecione sua **assinatura** do Azure.
	3. Selecione uma **Conta de armazenamento do Azure** na lista de contas de armazenamento do Azure disponíveis na assinatura selecionada. Você também pode inserir as configurações de conta de armazenamento manualmente selecionando a opção **Inserir manualmente** para o **Método de seleção de conta** e clicando em **Avançar**.

	![Ferramenta de Cópia - especifique a conta de armazenamento de Blobs do Azure](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
6. Na página **Escolher o arquivo de entrada ou a pasta**:
	1. Navegue até a pasta **adftutorial**.
	2. Selecione **emp.txt** e clique em **Escolher**
	3. Clique em **Avançar**.

	![Ferramenta de Cópia - escolha a pasta ou o arquivo de entrada](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
7. Na página **Escolha o arquivo ou a pasta de entrada**, clique em **Avançar**. Não selecione **Cópia binária**.

	![Ferramenta de Cópia - escolha a pasta ou o arquivo de entrada](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png)
8. Na página **Configurações de formato de arquivo**, você vê os delimitadores e o esquema é detectado automaticamente pelo assistente na análise do arquivo. Você também pode inserir os delimitadores manualmente para que o assistente de cópia pare de detectar automaticamente ou substitua. Clique em **Avançar** depois de revisar os delimitadores e visualizar os dados.

	![Ferramenta de Cópia - configurações de formato de arquivo](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)
8. Na página Repositório de dados de destino, selecione **Banco de Dados SQL do Azure** e clique em **Avançar**.

	![Ferramenta de Cópia - escolha o repositório de destino](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. Na página **Especificar o banco de dados SQL do Azure**:
	1. Digite **AzureSqlLinkedService** no campo **Nome da conexão**.
	2. Confirme se a opção **De assinaturas do Azure** foi selecionada em **Método de seleção de servidor/banco de dados**.
	3. Selecione sua **assinatura** do Azure.
	2. Selecione **Nome do servidor** e **Banco de Dados**.
	4. Insira o **Nome de usuário** e a **Senha**.
	5. Clique em **Avançar**.

	![Ferramenta de Cópia - especifique o banco de dados SQL do Azure](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
9. Na página **Mapeamento de tabela**, selecione **emp** para o campo **Destino** na lista suspensa e clique em **seta para baixo** (opcional) para ver o esquema e visualizar os dados.

	![Ferramenta de Cópia - mapeamento de tabela](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png)
10. Na página **Mapeamento de esquema**, clique em **Avançar**.

	![Ferramenta de Cópia - mapeamento de esquema](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
11. Na página **Configurações de desempenho**, clique em **Avançar**.

	![Ferramenta de cópia - configurações de desempenho](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
11. Examine as informações na página **Resumo** e clique em **Concluir**. Esse assistente cria dois serviços vinculados, dois conjuntos de dados (entrada e saída) e um pipeline no data factory (de onde você iniciou o Assistente de Cópia).

	![Ferramenta de cópia - configurações de desempenho](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## Iniciar o monitor e gerenciar aplicativo 
12. Na página **Implantação**, clique no link: **Clique aqui para monitorar o pipeline de cópia**.

	![Ferramenta de Cópia - implantação bem-sucedida](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)
13. Use as instruções de [Monitorar e gerenciar o pipeline usando o Aplicativo de Monitoramento](data-factory-monitor-manage-app.md) para saber mais sobre como monitorar o pipeline que você criou. Clique no ícone **Atualizar** na lista **JANELAS DE ATIVIDADE** para ver a fatia.

	![Aplicativo de Monitoramento](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)
 	
	> [AZURE.NOTE] Clique no botão **Atualizar** da lista **JANELAS DE ATIVIDADE** na parte inferior para ver o status mais recente. Ela não é atualizada automaticamente.

## Consulte também
| Tópico | Descrição |
| :---- | :---- |
| [Atividades de movimentação de dados](data-factory-data-movement-activities.md) | Este artigo fornece informações detalhadas sobre a Atividade de Cópia utilizada neste tutorial. |
| [Agendamento e execução](data-factory-scheduling-and-execution.md) | Este artigo explica os aspectos de agendamento e execução do modelo de aplicativo do Azure Data Factory. |
| [Pipelines](data-factory-create-pipelines.md) | Este artigo o ajuda a compreender pipelines e atividades no Azure Data Factory e como usá-los para construir fluxos de trabalho orientados a dados de ponta a ponta para seu cenário ou negócio. |
| [Conjunto de dados](data-factory-create-datasets.md) | Este artigo o ajuda a entender os conjuntos de dados no Azure Data Factory.
| [Monitorar e gerenciar pipelines usando o Aplicativo de Monitoramento](data-factory-monitor-manage-app.md) | Este artigo descreve como monitorar, gerenciar e depurar seus pipelines usando o Aplicativo de Monitoramento e Gerenciamento. 

<!---HONumber=AcomDC_1005_2016-->