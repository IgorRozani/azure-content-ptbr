<properties
	pageTitle="Todos os tópicos sobre o serviço do SQL Data Warehouse | Microsoft Azure"
	description="Uma tabela de todos os tópicos existentes sobre o serviço do Azure chamada SQL Data Warehouse em http://azure.microsoft.com/documentation/articles/, Título e descrição."
	services="sql-data-warehouse"
	documentationCenter=""
	authors="barbkess"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-data-warehouse"
	ms.workload="sql-data-warehouse"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/21/2016"
	ms.author="barbkess"/>


# Todos os tópicos sobre o serviço do SQL Data Warehouse do Azure

Este tópico lista todos os tópicos que se aplicam diretamente ao serviço do **SQL Data Warehouse** do Azure. Você pode pesquisar palavras-chave nesta página da Web usando **Ctrl + F** para encontrar os tópicos de seu interesse atual.



## Artigos atualizados

Esta seção lista os artigos que foram atualizados recentemente nos quais a atualização foi grande ou significativa. Para cada artigo atualizado, um trecho aproximado do texto markdown adicionado é exibido. Os artigos foram atualizados dentro do intervalo de datas de **26/07/2016** a **21/08/2016**.

| &nbsp; | Artigo | Texto atualizado, trecho de código |
| --: | :-- | :-- |
| 1 | [Gerenciamento de simultaneidade e carga de trabalho no SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) | **Consultas que respeitam os limites de simultaneidade** A maioria das consultas é controlada pelas classes de recursos. Essas consultas devem se ajustar tanto aos limites de consultas de simultaneidade quanto aos de slots de simultaneidade. Um usuário final não pode optar por excluir uma consulta do modelo de slot de simultaneidade. Para reiterar, as seguintes instruções **respeitam** as classes de recursos: / INSERT-SELECT / UPDATE / DELETE / SELECT (ao consultar tabelas de usuário) / ALTER INDEX REBUILD / ALTER INDEX REORGANIZE / ALTER TABLE REBUILD / CREATE INDEX / CREATE CLUSTERED COLUMNSTORE INDEX / CREATE TABLE AS SELECT (CTAS) / Carregamento de dados / Operações de movimentação de dados realizadas pelo Serviço de Movimentação de Dados (DMS) **Consultar exceções aos limites de simultaneidade** |
| 2 | [Detalhes de migração para o Armazenamento Premium](sql-data-warehouse-migrate-to-premium-storage.md) | Com a alteração para o Armazenamento Premium, também aumentamos o número de arquivos de blob do banco de dados na arquitetura subjacente do seu Data Warehouse. Se você encontrar problemas de desempenho, recomendamos que reconstrua seus Índices de Columnstore Clusterizados usando o script a seguir. Isso forçará alguns dos seus dados existentes para os blobs adicionais. Se você não tomar nenhuma ação, os dados serão redistribuídos naturalmente com o tempo, conforme você carregar mais dados nas tabelas do Data Warehouse. **Pré-requisitos:** 1. O Data Warehouse deve ser executado com 1.000 DWUs ou mais (consulte a capacidade de computação de escala) 2. O usuário que executa o script deve estar na função mediumrc ou superior 1. Para adicionar um usuário a essa função, execute o seguinte: 1. ````EXEC sp_addrolemember 'xlargerc', 'MyUser'```` ````sql /------------------------------------------------------------------------------ /- Etapa 1: criar tabela para controlar a recompilação de índice /- Execute como um usuário no mediumrc ou superior /------------------------------------------------------------ |





## Introdução

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 3 | [Autenticação no Azure SQL Data Warehouse](sql-data-warehouse-authentication.md) | Autenticação do AAD (Azure Active Directory) e SQL Server no Azure SQL Data Warehouse. |
| 4 | [Práticas recomendadas para o Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) | Recomendações e práticas recomendadas que você deve saber quando for desenvolver soluções para o SQL Data Warehouse do Azure. Isto irá ajudá-lo a alcançar o sucesso. |
| 5 | [Drivers do Azure SQL Data Warehouse](sql-data-warehouse-connection-strings.md) | Cadeias de conexão e drivers para SQL Data Warehouse |
| 6 | [Conectar-se ao SQL Data Warehouse do Azure](sql-data-warehouse-connect-overview.md) | Visão geral da conexão com o SQL Data Warehouse do Azure |
| 7 | [Analisar dados com o Aprendizado de Máquina do Azure](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Use o Aprendizado de Máquina do Azure para compilar um modelo de aprendizado de máquina preditivo com base nos dados armazenados no SQL Data Warehouse do Azure. |
| 8 | [Consultar o SQL Data Warehouse do Azure (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Como consultar o SQL Data Warehouse do Azure com o Utilitário de linha de comando sqlcmd. |
| 9 | [Criar um banco de dados do SQL Data Warehouse usando TSQL (Transact-SQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Saiba como criar um SQL Data Warehouse do Azure com o TSQL |
| 10 | [Como criar um tíquete de suporte para o SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) | Como criar um tíquete de suporte no Azure SQL Data Warehouse. |
| 11 | [Carregar dados com o Azure Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Saiba como carregar dados com o Azure Data Factory |
| 12 | [Carregar dados com o PolyBase no SQL Data Warehouse](sql-data-warehouse-get-started-load-with-polybase.md) | Saiba o que é o PolyBase e como usá-lo em cenários de data warehouse. |
| 13 | [Configurar um novo servidor lógico](sql-data-warehouse-get-started-new-server.md) | Saiba como criar um Azure SQL Data Warehouse no Portal do Azure |
| 14 | [Criar um Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md) | Saiba como criar um Azure SQL Data Warehouse no Portal do Azure |
| 15 | [Criar um SQL Data Warehouse usando o PowerShell](sql-data-warehouse-get-started-provision-powershell.md) | Criar SQL Data Warehouse usando o PowerShell |
| 16 | [Visualizar os dados com o Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Visualizar os dados do SQL Data Warehouse com o Power BI |
| 17 | [Consultar o SQL Data Warehouse do Azure (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Consultar o SQL Data Warehouse com o Visual Studio. |



## Desenvolver

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 18 | [Otimização de transações para o SQL Data Warehouse](sql-data-warehouse-develop-best-practices-transactions.md) | Diretrizes de práticas recomendadas sobre como gravar atualizações de transação eficientes no SQL Data Warehouse do Azure |
| 19 | [Gerenciamento de simultaneidade e carga de trabalho no SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) | Compreender o gerenciamento de simultaneidade e carga de trabalho no SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 20 | [Instrução Create Table As Select (CTAS) no SQL Data Warehouse](sql-data-warehouse-develop-ctas.md) | Dicas para codificação com a instrução create table as select (CTAS) no SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 21 | [SQL dinâmico no SQL Data Warehouse](sql-data-warehouse-develop-dynamic-sql.md) | Dicas para usar SQL dinâmico no SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 22 | [Agrupar por opções de SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) | Dicas para implementar o agrupamento por opções no SQL Data Warehouse do Azure para desenvolver soluções. |
| 23 | [Usar rótulos para consultas de instrumento no SQL Data Warehouse](sql-data-warehouse-develop-label.md) | Dicas para usar rótulos para consultas de instrumento no SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 24 | [Executar loops no SQL Data Warehouse](sql-data-warehouse-develop-loops.md) | Dicas para execução de loops do Transact\_SQL e substituição de cursores no SQL Data Warehouse Azure para desenvolver soluções. |
| 25 | [Procedimentos armazenados no SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) | Dicas para implementar procedimentos armazenados no SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 26 | [Transações no SQL Data Warehouse](sql-data-warehouse-develop-transactions.md) | Dicas para implementar transações no Azure SQL Data Warehouse para o desenvolvimento de soluções. |
| 27 | [Esquemas definidos pelo usuário no SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) | Dicas para usar esquemas de Transact-SQL no Azure SQL Data Warehouse para desenvolvimento de soluções. |
| 28 | [Atribuir variáveis no SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) | Dicas para atribuir variáveis de Transact-SQL no Azure SQL Data Warehouse para desenvolvimento de soluções. |
| 29 | [Modos de exibição no SQL Data Warehouse](sql-data-warehouse-develop-views.md) | Dicas para usar exibições Transact-SQL no Azure SQL Data Warehouse para desenvolvimento de soluções. |
| 30 | [Decisões de design e técnicas de codificação para o SQL Data Warehouse](sql-data-warehouse-overview-develop.md) | Conceitos de desenvolvimento, decisões de design, recomendações e técnicas de codificação para o SQL Data Warehouse. |



## Gerenciar

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 31 | [Gerenciar poder de computação no SQL Data Warehouse do Azure (Visão Geral)](sql-data-warehouse-manage-compute-overview.md) | Funcionalidades de escala horizontal de desempenho no SQL Data Warehouse do Azure. Escale horizontalmente por meio de ajuste de DWUs ou, para economizar custos, pause e retome os recursos de computação. |
| 32 | [Gerenciar poder de computação no SQL Data Warehouse do Azure (portal do Azure)](sql-data-warehouse-manage-compute-portal.md) | Tarefas do portal do Azure para gerenciar o poder de computação. Dimensionar recursos de computação ajustando as DWUs. Ou, para economizar custos, pause e retome os recursos de computação. |
| 33 | [Gerenciar poder de computação no SQL Data Warehouse do Azure (PowerShell)](sql-data-warehouse-manage-compute-powershell.md) | Tarefas do PowerShell para gerenciar o poder de computação. Dimensionar recursos de computação ajustando as DWUs. Ou, para economizar custos, pause e retome os recursos de computação. |
| 34 | [Gerenciar poder de computação no SQL Data Warehouse do Azure (REST)](sql-data-warehouse-manage-compute-rest-api.md) | Tarefas do PowerShell para gerenciar o poder de computação. Dimensionar recursos de computação ajustando as DWUs. Ou, para economizar custos, pause e retome os recursos de computação. |
| 35 | [Gerenciar poder de computação no SQL Data Warehouse do Azure (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Tarefas de Transact-SQL (T-SQL) para escalar horizontalmente o desempenho ao ajustar DWUs. Reduzir custos por meio da redução durante horários que não sejam de pico. |
| 36 | [Monitore sua carga de trabalho usando DMVs](sql-data-warehouse-manage-monitor.md) | Aprenda a monitorar sua carga de trabalho usando DMVs. |
| 37 | [Gerenciar bancos de dados no SQL Data Warehouse do Azure](sql-data-warehouse-overview-manage.md) | Visão geral do gerenciamento de bancos de dados do SQL Data Warehouse. Inclui ferramentas de gerenciamento, DWUs e escalar o desempenho horizontalmente, solucionar problemas de desempenho de consulta, estabelecer políticas de segurança e restaurar um banco de dados contra dados corrompidos ou de uma interrupção regional. |
| 38 | [Monitorar consultas de usuário no SQL Data Warehouse do Azure](sql-data-warehouse-overview-manage-user-queries.md) | Visão geral sobre considerações, melhores práticas e tarefas para monitoramento de consultas de usuário no SQL Data Warehouse do Azure |
| 11,8 | [Recuperar um SQL Data Warehouse do Azure (Visão geral)](sql-data-warehouse-restore-database-overview.md) | Visão geral das opções de restauração para recuperar um banco de dados no SQL Data Warehouse do Azure. |
| 40 | [Restaurar um Azure SQL Data Warehouse (Portal)](sql-data-warehouse-restore-database-portal.md) | Tarefas do portal do Azure para restaurar um Azure SQL Data Warehouse. |
| 41 | [Restaurar um Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-restore-database-powershell.md) | Tarefas do PowerShell para restaurar um Azure SQL Data Warehouse. |
| 42 | [Recuperar um SQL Data Warehouse do Azure (API REST)](sql-data-warehouse-restore-database-rest-api.md) | Tarefas da API REST para restaurar um Azure SQL Data Warehouse. |



## Tabelas e índices

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 43 | [Tipos de dados para as tabelas no SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) | Introdução aos tipos de dados para as tabelas do Azure SQL Data Warehouse. |
| 44 | [Distribuindo tabelas no SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) | Introdução à distribuição de tabelas no Azure SQL Data Warehouse. |
| 45 | [Indexando tabelas no SQL Data Warehouse](sql-data-warehouse-tables-index.md) | Introdução à indexação de tabela no Azure SQL Data Warehouse. |
| 46 | [Visão geral das tabelas no SQL Data Warehouse](sql-data-warehouse-tables-overview.md) | Introdução às Tabelas do Azure SQL Data Warehouse. |
| 47 | [Particionando tabelas no SQL Data Warehouse](sql-data-warehouse-tables-partition.md) | Introdução ao particionamento de tabela no Azure SQL Data Warehouse. |
| 48 | [Gerenciamento de estatísticas em tabelas no SQL Data Warehouse](sql-data-warehouse-tables-statistics.md) | Introdução às estatísticas em tabelas no Azure SQL Data Warehouse. |
| 49 | [Tabelas temporárias no SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) | Introdução às tabelas temporárias no Azure SQL Data Warehouse. |



## Integração

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 50 | [Use o Azure Data Factory com o SQL Data Warehouse](sql-data-warehouse-integrate-azure-data-factory.md) | Dicas de utilização do Azure Data Factory (ADF) com o Azure SQL Data Warehouse para o desenvolvimento de soluções. |
| 51 | [Use o Aprendizado de Máquina do Azure com o SQL Data Warehouse](sql-data-warehouse-integrate-azure-machine-learning.md) | Tutorial para usar o Aprendizado de Máquina do Azure com o Data Warehouse do SQL Azure para desenvolver soluções. |
| 52 | [Usar o Stream Analytics do Azure com o SQL Data Warehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) | Dicas para usar o Stream Analytics do Azure com o Azure SQL Data Warehouse para desenvolver as soluções. |
| 53 | [Usar o Power BI com o SQL Data Warehouse](sql-data-warehouse-integrate-power-bi.md) | Dicas para usar o Power BI com o SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 54 | [Aproveitar outros serviços com o SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) | Ferramentas e parceiros com soluções que podem ser integradas ao SQL Data Warehouse |



## Carregar

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 55 | [Carregar dados do Armazenamento de Blobs do Azure no Azure SQL Data Warehouse (Azure Data Factory).](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Saiba como carregar dados com o Azure Data Factory |
| 56 | [Carregar dados do armazenamento de blobs do Azure no SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Aprenda a usar o PolyBase para carregar dados do armazenamento de blobs do Azure no SQL Data Warehouse. Carregue algumas tabelas de dados públicos no esquema do Data Warehouse de Varejo da Contoso. |
| 57 | [Carregar dados do SQL Server no Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Usa o bcp para exportar dados do SQL Server para arquivos simples, o AZCopy para importar dados no Armazenamento de Blobs do Azure e o PolyBase para incluir os dados no Azure SQL Data Warehouse. |
| 58 | [Carregar dados do SQL Server no Azure SQL Data Warehouse (arquivos simples)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Para um tamanho de dados pequeno, use bcp para exportar dados do SQL Server para arquivos simples e importar os dados diretamente no Azure SQL Data Warehouse. |
| 59 | [Carregar dados do SQL Server no Azure SQL Data Warehouse (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Mostra a você como criar um pacote do SSIS (SQL Server Integration Services) para mover dados de uma ampla variedade de fontes de dados para o SQL Data Warehouse. |
| 60 | [Carregar dados com o PolyBase no SQL Data Warehouse](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Usa o bcp para exportar dados do SQL Server para arquivos simples, o AZCopy para importar dados no Armazenamento de Blobs do Azure e o PolyBase para incluir os dados no Azure SQL Data Warehouse. |
| 61 | [Guia para usar o PolyBase no SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) | Diretrizes e recomendações para usar o PolyBase em cenários do SQL Data Warehouse. |
| 62 | [Carregar dados de amostra no SQL Data Warehouse](sql-data-warehouse-load-sample-databases.md) | Carregar dados de amostra no SQL Data Warehouse |
| 63 | [Carregar dados com o bcp](sql-data-warehouse-load-with-bcp.md) | Saiba o que é o bcp e como usá-lo em cenários de data warehouse. |
| 64 | [Carregar dados no SQL Data Warehouse do Azure](sql-data-warehouse-overview-load.md) | Aprenda sobre os cenários comuns para carregamento de dados no SQL Data Warehouse. Essas opções incluem usar PolyBase, armazenamento de blobs do Azure, arquivos simples e envio de disco. Você também pode usar ferramentas de terceiros. |



## Migrar

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 65 | [Migrar seu código SQL para o SQL Data Warehouse](sql-data-warehouse-migrate-code.md) | Dicas para migrar seu código SQL para o SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 66 | [Migrar seus dados](sql-data-warehouse-migrate-data.md) | Dicas para migrar seus dados para o SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 67 | [Utilitário de Migração do Data Warehouse (visualização)](sql-data-warehouse-migrate-migration-utility.md) | Migrar para o SQL Data Warehouse. |
| 68 | [Migrar seu esquema para o SQL Data Warehouse](sql-data-warehouse-migrate-schema.md) | Dicas para migrar seu esquema para o SQL Data Warehouse do Azure para desenvolvimento de soluções. |
| 69 | [Migrar sua solução para o SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) | Orientação de migração para levar sua solução até a plataforma SQL Data Warehouse do Azure. |



## Parceiros

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 70 | [Parceiros de business intelligence do SQL Data Warehouse](sql-data-warehouse-partner-business-intelligence.md) | Listas de parceiros terceirizados de business intelligence com soluções que dão suporte ao SQL Data Warehouse. |
| 71 | [Parceiros de integração de dados do SQL Data Warehouse](sql-data-warehouse-partner-data-integration.md) | Listas de parceiros terceirizados com soluções de integração de dados que oferecem suporte ao Azure SQL Data Warehouse. |
| 72 | [Parceiros de gerenciamento de dados do SQL Data Warehouse](sql-data-warehouse-partner-data-management.md) | Lista de parceiros terceirizados de gerenciamento de dados com soluções que oferecem suporte ao SQL Data Warehouse. |



## Referência

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 73 | [Tópicos de referência para o SQL Data Warehouse](sql-data-warehouse-overview-reference.md) | Links de conteúdo de referência para o SQL Data Warehouse. |
| 74 | [Cmdlets do PowerShell e as APIs REST para SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) | Encontre os principais cmdlets do PowerShell para SQL Data Warehouse do Azure, inclusive sobre como pausar e retomar um banco de dados. |
| 75 | [Elementos de linguagem](sql-data-warehouse-reference-tsql-language-elements.md) | Lista de links para conteúdo de referência dos elementos da linguagem Transact-SQL usados para o SQL Data Warehouse. |
| 76 | [Tópicos do Transact-SQL](sql-data-warehouse-reference-tsql-statements.md) | Links para conteúdo de referência dos tópicos do Transact-SQL usados pelo SQL Data Warehouse. |
| 77 | [Exibições do sistema](sql-data-warehouse-reference-tsql-system-views.md) | Os links para o sistema exibem conteúdo para o SQL Data Warehouse. |



## Segurança

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 78 | [SQL Data Warehouse - Suporte a clientes de versão anterior para auditoria e Mascaramento dinâmico de dados](sql-data-warehouse-auditing-downlevel-clients.md) | Saiba mais sobre o suporte a clientes de versão anterior do SQL Data Warehouse para auditoria de dados |
| 79 | [Auditoria no Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) | Introdução à auditoria no SQL Data Warehouse |
| 80 | [Introdução aos dados TDE (Transparent Data Encryption) no SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) | Introdução aos dados TDE (Transparent Data Encryption) no SQL Data Warehouse |
| 81 | [Introdução ao Transparent Data Encryption (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Introdução ao TSQL de Transparent Data Encryption (TDE) do SQL Data Warehouse |
| 82 | [Proteger um banco de dados no SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) | Dicas para proteger um banco de dados no SQL Data Warehouse do Azure para desenvolvimento de soluções. |



## Diversos

| &nbsp; | Title | Descrição |
| --: | :-- | :-- |
| 83 | [Instalar o Visual Studio 2015 e o SSDT para o SQL Data Warehouse](sql-data-warehouse-install-visual-studio.md) | Instalar o Visual Studio e o SSDT (Ferramentas de Desenvolvimento do SQL Server) para o SQL Data Warehouse do Azure |
| 84 | [Detalhes de migração para o Armazenamento Premium](sql-data-warehouse-migrate-to-premium-storage.md) | Instruções para migrar um SQL Data Warehouse existente para o armazenamento premium |
| 85 | [Introdução à detecção de ameaças](sql-data-warehouse-security-threat-detection.md) | Introdução à detecção de ameaças |
| 86 | [Limites de capacidade do SQL Data Warehouse](sql-data-warehouse-service-capacity-limits.md) | Valores máximos para conexões, bancos de dados, tabelas e consultas para o SQL Data Warehouse. |
| 87 | [Solução de problemas do Azure SQL Data Warehouse](sql-data-warehouse-troubleshoot.md) | Solução de problemas do Azure SQL Data Warehouse. |

<!---HONumber=AcomDC_0824_2016-->