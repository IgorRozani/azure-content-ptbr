<properties
	pageTitle="Gerenciar o Banco de Dados SQL do Azure com o PowerShell | Microsoft Azure"
	description="Gerenciamento de banco de dados SQL do Azure com o PowerShell."
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/13/2016"
	ms.author="sstein"/>

# Gerenciar o Banco de Dados SQL do Azure com o PowerShell


> [AZURE.SELECTOR]
- [Portal do Azure](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-command-line-tools.md)

Este tópico mostra os cmdlets do PowerShell que são usados para executar várias tarefas do Banco de Dados SQL do Azure. Para ver uma lista completa, consulte [Cmdlets do Banco de Dados SQL do Azure](https://msdn.microsoft.com/library/mt574084.aspx).


## Criar um grupos de recursos

Crie um grupo de recursos para nosso Banco de Dados SQL e para recursos relacionados do Azure com o cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837.aspx).

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Para obter mais informações, consulte [Usando o PowerShell do Azure com o Gerenciador de Recursos do Azure](../powershell-azure-resource-manager.md). Para ver um script de exemplo, consulte [Criar um script do PowerShell do banco de dados SQL](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## Criar um servidor de Banco de Dados SQL

Crie um servidor de Banco de Dados SQL com o cmdlet [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx). Substitua *server1* pelo nome do seu servidor. Os nomes dos servidores devem ser exclusivos para todos os servidores do Banco de Dados SQL do Azure. Se o nome do servidor já existir, você obterá um erro. Esse comando pode levar vários minutos para ser concluído. O grupo de recursos já precisa existir na sua assinatura.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Para obter mais informações, consulte [O que é o Banco de Dados SQL](sql-database-technical-overview.md). Para ver um script de exemplo, consulte [Criar um script do PowerShell do banco de dados SQL](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## Criar uma regra de firewall do servidor do Banco de Dados SQL

Crie uma regra de firewall para acessar o servidor com o cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx). Execute o comando a seguir substituindo os endereços IP inicial e final pelos valores válidos para o seu cliente. O grupo de recursos e o servidor já precisam existir na sua assinatura.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Para permitir que outros serviços do Azure acessem seu servidor, crie uma regra de firewall e defina `-StartIpAddress` e `-EndIpAddress` como **0.0.0.0**. Essa regra de firewall especial permite que todo o tráfego do Azure acesse o servidor.

Para saber mais, confira [Firewall do Banco de Dados SQL do Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx). Para ver um script de exemplo, consulte [Criar um script do PowerShell do banco de dados SQL](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## Criar um banco de dados SQL (em branco)

Crie um banco de dados com o cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx). O grupo de recursos e o servidor já precisam existir na sua assinatura.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Para obter mais informações, consulte [O que é o Banco de Dados SQL](sql-database-technical-overview.md). Para ver um script de exemplo, consulte [Criar um script do PowerShell do banco de dados SQL](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## Alterar o nível de desempenho do banco de dados SQL

Escale e reduza verticalmente seu banco de dados com o cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx). O grupo de recursos, o servidor e o banco de dados já precisam existir na sua assinatura. Defina `-RequestedServiceObjectiveName` como um espaço simples (como o trecho a seguir) para a camada Básica. Defina como *S0*, *S1*, *P1*, *P6* etc., como no exemplo anterior, para outras camadas.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Para obter mais informações, consulte [Opções e desempenho de Banco de Dados SQL: saiba o que está disponível em cada camada de serviço](sql-database-service-tiers.md). Para ver um script de exemplo, consulte [Amostra de script do PowerShell para alterar a camada de serviços e o nível de desempenho do banco de dados SQL](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## Copiar um banco de dados SQL para o mesmo servidor

Copie um banco de dados SQL para o mesmo servidor com o cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644.aspx). Defina `-CopyServerName` e `-CopyResourceGroupName` com os mesmos valores que seu grupo de recursos e servidor de banco de dados de origem.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Para saber mais, confira [Copiar um Banco de Dados SQL do Azure](sql-database-copy.md). Para ver um script de exemplo, consulte [Copiar um script do PowerShell do banco de dados SQL](sql-database-copy-powershell.md#example-powershell-script).


## Excluir um banco de dados SQL

Exclua um banco de dados SQL com o cmdlet [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx). O grupo de recursos, o servidor e o banco de dados já precisam existir na sua assinatura.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## Excluir um servidor de Banco de Dados SQL

Exclua um servidor com o cmdlet [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx).

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## Criar e gerenciar pools de bancos de dados elásticos usando o PowerShell

Para ver detalhes de como criar pools de bancos de dados elásticos usando o PowerShell, consulte [Criar um novo pool de banco de dados elásticos com o PowerShell](sql-database-elastic-pool-create-powershell.md).

Para ver detalhes de como gerenciar pools de bancos de dados elásticos usando o PowerShell, consulte [Monitorar e gerenciar um pool de banco de dados elástico com o PowerShell](sql-database-elastic-pool-manage-powershell.md).



## Informações relacionadas

- [Cmdlets do Banco de Dados SQL do Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx)
- [Referência de Cmdlets do Azure](https://msdn.microsoft.com/library/azure/dn708514.aspx)

<!---HONumber=AcomDC_0914_2016-->