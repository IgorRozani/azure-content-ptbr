<properties 
	pageTitle="Como usar o Armazenamento de Fila do Ruby | Microsoft Azure" 
	description="Saiba como usar o serviço Fila do Azure para criar e excluir filas, bem como para inserir, obter e excluir mensagens. Exemplos gravados no Ruby." 
	services="storage" 
	documentationCenter="ruby" 
	authors="rmcmurray" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="storage" 
	ms.workload="storage" 
	ms.tgt_pltfrm="na" 
	ms.devlang="ruby" 
	ms.topic="article" 
	ms.date="08/11/2016" 
	ms.author="cbrooks;robmcm"/>


# Como usar o Armazenamento de fila do Ruby

[AZURE.INCLUDE [armazenamento-seletor-fila-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## Visão geral

Este guia mostra como executar cenários comuns usando o serviço de Armazenamento de Fila do Microsoft Azure. Os exemplos são gravados usando a API do Ruby Azure. Os cenários abrangidos incluem **inserir**, **exibir**, **obter** e **excluir** mensagens da fila, bem como **criar e excluir filas**.

[AZURE.INCLUDE [armazenamento-fila-conceitos-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Criar um aplicativo Ruby

Crie um aplicativo Ruby. Para ver as instruções, confira [Aplicativo Web Ruby on Rails em uma VM do Azure](../virtual-machines/virtual-machines-linux-classic-ruby-rails-web-app.md).

## Configurar seu aplicativo para acessar o armazenamento

Para usar o armazenamento do Azure, você deverá baixar e usar o pacote do Azure do Ruby, que inclui um conjunto de bibliotecas convenientes que se comunicam com os serviços REST do armazenamento.

### Usar RubyGems para obter o pacote

1. Use uma interface da linha de comando como **PowerShell** (Windows), **Terminal** (Mac) ou **Bash** (Unix).

2. Digite "gem install azure" na janela de comandos para instalar a gema e as dependências.

### Importar o pacote

Use seu editor de texto favorito e adicione o seguinte na parte superior do arquivo do Ruby no qual você pretende usar o armazenamento:

	require "azure"

## Configurar uma conexão de armazenamento do Azure

O módulo do Azure lerá as variáveis de ambiente **AZURE\_STORAGE\_ACCOUNT** e **AZURE\_STORAGE\_ACCESS\_KEY** para obter as informações necessárias para se conectar à sua conta de armazenamento do Azure. Se essas variáveis de ambiente não forem definidas, você deverá especificar as informações da conta antes de usar **Azure::QueueService** com o seguinte código:

	Azure.config.storage_account_name = "<your azure storage account>"
	Azure.config.storage_access_key = "<your Azure storage access key>"

 
Para obter esses valores de uma conta de armazenamento clássico ou do Resource Manager no Portal do Azure:

1. Faça logon no [Portal do Azure](https://portal.azure.com).
2. Navegue até a conta de armazenamento que você deseja usar.
3. Na folha Configurações no lado direito, clique em **Chaves de Acesso**.
4. Na folha Acessar chaves exibida, você verá as teclas de acesso 1 e 2. Você pode usar qualquer uma das duas.
5. Clique no ícone de cópia para copiar a chave para a área de transferência.

Para obter esses valores de uma conta de armazenamento clássico no Portal Clássico do Azure:

1. Faça logon no [Portal Clássico do Azure](https://manage.windowsazure.com).
2. Navegue até a conta de armazenamento que você deseja usar.
3. Clique em **GERENCIAR CHAVES DE ACESSO** na parte inferior do painel de navegação.
4. Na caixa de diálogo pop-up, você verá o nome da conta de armazenamento, a chave de acesso primária e a chave de acesso secundária. Para a chave de acesso, você pode usar tanto a primária quanto a secundária.
5. Clique no ícone de cópia para copiar a chave para a área de transferência.

## Como criar uma fila

O código a seguir cria o objeto **Azure::QueueService**, permitindo que você trabalhe com filas.

	azure_queue_service = Azure::QueueService.new

Use o método **create\_queue()** para criar uma fila com o nome especificado.

	begin
	  azure_queue_service.create_queue("test-queue")
	rescue
	  puts $!
	end

## Como inserir uma mensagem em uma fila

Para inserir uma mensagem em uma fila, use o método **create\_message()** para criar uma nova mensagem e adicione-a à fila.

	azure_queue_service.create_message("test-queue", "test message")

## Como: espiar a próxima mensagem

Você pode espiar a mensagem na frente de uma fila sem removê-la da fila chamando o método **peek\_messages()**. Por padrão, **peek\_messages()** exibe uma única mensagem. Você também pode especificar quantas mensagens deseja inspecionar.

	result = azure_queue_service.peek_messages("test-queue",
	  {:number_of_messages => 10})

## Como: remover a próxima mensagem da fila

É possível remover uma mensagem da fila em duas etapas.

1. Quando você chamar **list\_messages()**, receberá a próxima mensagem em uma fila por padrão. Você também pode especificar quantas mensagens deseja obter. As mensagens retornadas de **list\_messages()** se tornam invisíveis para todas as outras mensagens de leitura de código da fila. Passe o tempo limite de visibilidade em segundos como um parâmetro.

2. Para concluir a remoção da mensagem da fila, chame também **delete\_message()**.

Este processo de duas etapas de remover uma mensagem garante que quando o código não processa uma mensagem devido à falhas de hardware ou de software, outra instância do seu código pode receber a mesma mensagem e tentar novamente. O código chama **delete\_message()** logo depois que a mensagem é processada.

	messages = azure_queue_service.list_messages("test-queue", 30)
	azure_queue_service.delete_message("test-queue", 
	  messages[0].id, messages[0].pop_receipt)

## Como: alterar o conteúdo de uma mensagem em fila

Você pode alterar o conteúdo de uma mensagem in-loco na fila. O código a seguir usa o método **update\_message()** para atualizar uma mensagem. O método retornará uma tupla que contém o recebimento pop da mensagem da fila e um valor de data/hora UTC que representa quando a mensagem estará visível na fila.

	message = azure_queue_service.list_messages("test-queue", 30)
	pop_receipt, time_next_visible = azure_queue_service.update_message(
	  "test-queue", message.id, message.pop_receipt, "updated test message", 
	  30)

## Como adicionar opções para remover mensagens da fila

Há duas maneiras de personalizar a recuperação da mensagem de uma fila.

1. Você pode obter um lote de mensagens.

2. Você pode definir um tempo limite de invisibilidade mais longo ou mais curto, permitindo mais ou menos tempo para seu código processar totalmente cada mensagem.

O exemplo de código a seguir usa o método **list\_messages()** para receber 15 mensagens em uma chamada. Em seguida, ele lê e exclui cada mensagem. Ele também define o tempo limite de invisibilidade de cinco minutos para cada mensagem.

	azure_queue_service.list_messages("test-queue", 300
	  {:number_of_messages => 15}).each do |m|
	  puts m.message_text
	  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
	end

## Como obter o comprimento da fila

Você pode obter uma estimativa do número de mensagens na fila. O método **get\_queue\_metadata()** solicita que o serviço Fila retorne os metadados e a contagem de mensagens aproximados sobre a fila.

	message_count, metadata = azure_queue_service.get_queue_metadata(
	  "test-queue")

## Como excluir uma fila

Para excluir uma fila e todas as mensagens contidas nela, chame o método **delete\_queue()** no objeto de fila.

	azure_queue_service.delete_queue("test-queue")

## Próximas etapas

Agora que você aprendeu os conceitos básicos do armazenamento de fila, siga estes links para saber mais sobre tarefas de armazenamento mais complexas.

- Visite o [Blog da Equipe de Armazenamento do Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- Visite o repositório [SDK do Azure para o nó](https://github.com/WindowsAzure/azure-sdk-for-ruby) no GitHub.

Para fazer uma comparação entre o serviço Fila do Azure discutido neste artigo e as filas do Barramento de Serviço do Azure discutidas no artigo [How to use Service Bus Queues](/develop/ruby/how-to-guides/service-bus-queues/) (Como usar o Barramento de Serviço), confira [Filas do Azure e filas do Barramento de Serviço — comparações e contrastes](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 

<!---HONumber=AcomDC_0928_2016-->