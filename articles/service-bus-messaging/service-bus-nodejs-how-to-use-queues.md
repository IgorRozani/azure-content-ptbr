<properties 
	pageTitle="Como usar as filas de Barramento de Serviço no Node.js | Microsoft Azure" 
	description="Aprenda a usar as filas do Barramento de Serviço no Azure a partir de um aplicativo Node.js." 
	services="service-bus-messaging" 
	documentationCenter="nodejs" 
	authors="sethmanheim" 
	manager="timlt" 
	editor=""/>

<tags 
	ms.service="service-bus-messaging" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="nodejs" 
	ms.topic="article" 
	ms.date="05/06/2016" 
	ms.author="sethm"/>

# Como usar filas do Barramento de Serviço

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Este artigo descreve como usar as filas do Barramento de Serviço no Node.js. As amostras são escritas em JavaScript e usam o módulo Node.js do Azure. Os cenários abordados incluem a **criação de filas**, o **envio e o recebimento de mensagens** e a **exclusão de filas**. Para obter mais informações sobre filas, veja a seção [Próximas etapas][].

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## Criar um aplicativo do Node.js

Criar um aplicativo Node.js em branco. Para obter instruções sobre como criar um aplicativo Node.js, veja a seção [Criar e implantar um aplicativo Node.js em um site do Azure][] ou [Serviço de Nuvem do Node.js ][]\(usando o Windows PowerShell).

## Configurar seu aplicativo para usar o Barramento de serviço

Para usar o Barramento de Serviço do Azure, baixe e use o pacote do Azure Node.js. Este pacote inclui um conjunto de bibliotecas que se comunicam com os serviços REST do barramento de serviço.

### Usar o NPM (gerenciador de pacotes de nós) para obter o pacote

1. Use a janela de comando **Windows PowerShell para Node.js** para navegar até a pasta **c:\\node\\sbqueues\\WebRole1** na qual você criou o aplicativo de exemplo.

2. Digite **npm install azure** na janela Comando, o que deve resultar em uma saída semelhante à seguinte:

	```
	azure@0.7.5 node_modules\azure
		├── dateformat@1.0.2-1.2.3
		├── xmlbuilder@0.4.2
		├── node-uuid@1.2.0
		├── mime@1.2.9
		├── underscore@1.4.4
		├── validator@1.1.1
		├── tunnel@0.0.2
		├── wns@0.5.3
		├── xml2js@0.2.7 (sax@0.5.2)
		└── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
	```

3. Você pode executar o comando **ls** manualmente para verificar se uma pasta **node\_modules** foi criada. Dentro dessa pasta, você encontrará o pacote **azure**, que contém as bibliotecas necessárias para acessar as filas do Barramento de Serviço.

### Importar o módulo

Usando o Bloco de Notas ou outro editor de texto, adicione o seguinte ao início do arquivo **server.js** do aplicativo:

```
var azure = require('azure');
```

### Configurar uma conexão do barramento de serviço do Azure

O módulo Azure lê as variáveis de ambiente AZURE\_SERVICEBUS\_NAMESPACE e AZURE\_SERVICEBUS\_ACCESS\_KEY para obter as informações necessárias para se conectar ao Barramento de Serviço. Se essas variáveis de ambiente não estiverem definidas, você deverá especificar as informações da conta chamando **createServiceBusService**.

Para obter um exemplo de como definir as variáveis de ambiente em um arquivo de configuração para um Serviço de Nuvem do Azure, consulte [Serviço de Nuvem do Node.js com Armazenamento][].

Para ver um exemplo de como definir variáveis de ambiente no [portal clássico do Azure][] para um Site do Azure, veja [Aplicativo Web do Node.js com armazenamento][].

## Criar uma fila

O objeto **ServiceBusService** permite que você trabalhe com filas de barramento de serviço. O código a seguir cria um objeto **ServiceBusService**. Adicione-o próximo ao início do arquivo **server.js**, após a instrução de importação do módulo Azure:

```
var serviceBusService = azure.createServiceBusService();
```

Ao chamar **createQueueIfNotExists** no objeto **ServiceBusService**, a fila especificada (se houver) é retornada ou uma nova fila com o nome especificado é criada. O seguinte código usa o **createQueueIfNotExists** para criar ou conectar-se à fila denominada `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** também oferece suporte para opções adicionais, que permitem a substituição de configurações padrão da fila, como a vida útil da mensagem ou o tamanho máximo da fila. O exemplo a seguir define o tamanho máximo da fila como 5 GB e a vida útil (TTL) como um minuto:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### Filtros

É possível aplicar operações de filtragem opcionais às operações executadas usando **ServiceBusService**. As operações de filtragem podem incluir registro em log, repetição automática etc. Filtros são objetos que implementam um método com a assinatura:

```
function handle (requestOptions, next)
```

Após fazer seu pré-processamento nas opções de solicitação, o método precisará chamar `next`, passando um retorno de chamada com a assinatura a seguir:

```
function (returnObject, finalCallback, next)
```

Nesse retorno de chamada, e após processar o **returnObject** (a resposta da solicitação ao servidor), o retorno de chamada precisará invocar `next`, se ele existir, para continuar processando outros filtros ou simplesmente invocar `finalCallback` para terminar a invocação de serviço.

Dois filtros que implementam a lógica de repetição estão incluídos no SDK do Azure para Node.js, **ExponentialRetryPolicyFilter** e **LinearRetryPolicyFilter**. O seguinte código cria um objeto **ServiceBusService** que usa **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## Enviar mensagens a uma fila

Para enviar uma mensagem para uma fila do Barramento de Serviço, seu aplicativo chamará o método **sendQueueMessage** no objeto **ServiceBusService**. Mensagens enviadas para (e recebidas de) as filas do Barramento de Serviço são objetos **BrokeredMessage** e têm um conjunto de propriedades padrão (como **Label** e **TimeToLive**), um dicionário que é usado para manter propriedades personalizadas específicas ao aplicativo e um corpo de dados arbitrários do aplicativo. Um aplicativo pode definir o corpo da mensagem passando uma cadeia de caracteres como a mensagem. As propriedades padrão necessárias são preenchidas com valores padrão.

O exemplo a seguir demonstra como enviar uma mensagem de teste à fila chamada `myqueue` usando **sendQueueMessage**:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

As filas do Barramento de Serviço dão suporte ao tamanho máximo de mensagem de 256 KB na [camada Standard](service-bus-premium-messaging.md) e 1 MB na [camada Premium](service-bus-premium-messaging.md). O cabeçalho, que inclui as propriedades de aplicativo padrão e personalizadas, pode ter um tamanho máximo de 64 KB. Não há nenhum limite no número de mensagens mantidas em uma fila mas há uma capacidade do tamanho total das mensagens mantidas por uma fila. O tamanho da fila é definido no momento da criação, com um limite superior de 5 GB. Para saber mais sobre cotas, consulte [Cotas do Barramento de Serviço][].

## Receber mensagens de uma fila

As mensagens são recebidas de uma fila usando o método **receiveQueueMessage** no objeto **ServiceBusService**. Por padrão, as mensagens são excluídas da fila à medida que são lidas; no entanto, você pode ler (pico) e bloquear a mensagem sem excluí-la da fila configurando o parâmetro opcional **isPeekLock** como **true**.

O comportamento padrão da leitura e da exclusão da mensagem como parte da operação de recebimento é o modelo mais simples e funciona melhor em cenários nos quais um aplicativo possa tolerar o não processamento de uma mensagem em caso de falha. Para compreender isso, considere um cenário no qual o consumidor emite a solicitação de recebimento e então falha antes de processá-la. Como o Barramento de Serviço terá marcado a mensagem como sendo consumida, quando o aplicativo for reiniciado e começar a consumir mensagens novamente, ele terá perdido a mensagem que foi consumida antes da falha.

Se o parâmetro **isPeekLock** estiver definido como **true**, o processo de recebimento se torna uma operação de duas etapas, o que torna possível o suporte a aplicativos que não toleram mensagens ausentes. Quando o Barramento de Serviço recebe uma solicitação, ele encontra a próxima mensagem a ser consumida, a bloqueia para evitar que outros clientes a recebam e a retorna para o aplicativo. Depois que o aplicativo termina de processar a mensagem (ou a armazena de forma segura para um processamento futuro), ele conclui o segundo estágio do processo de recebimento, chamando o método **deleteMessage** e fornecendo a mensagem a ser excluída como um parâmetro. O método **deleteMessage** marcará a mensagem como sendo consumida e a removerá da fila.

O exemplo a seguir demonstra como receber e processar mensagens usando **receiveQueueMessage**. Primeiro, o exemplo recebe e exclui uma mensagem, em seguida recebe a mensagem usando **isPeekLock** definido como **true** e então exclui a mensagem usando **deleteMessage**:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## Como tratar falhas do aplicativo e mensagens ilegíveis

O Barramento de Serviço proporciona funcionalidade para ajudá-lo a se recuperar normalmente dos erros no seu aplicativo ou das dificuldades no processamento de uma mensagem. Se um aplicativo receptor não puder processar a mensagem por algum motivo, ele chamará o método **unlockMessage** no objeto **ServiceBusService**. Isso fará com que o Barramento de Serviço desbloqueie a mensagem na fila e disponibilize-a para ser recebida novamente, pelo mesmo aplicativo de consumo ou por outro.

Também há um tempo limite associado a uma mensagem bloqueada na fila e, se o aplicativo não conseguir processar a mensagem antes da expiração do tempo limite do bloqueio (por exemplo, em caso de falha do aplicativo), o Service Bus desbloqueará a mensagem automaticamente e a disponibilizará para ser recebida novamente.

Caso o aplicativo falhe após o processamento da mensagem, mas antes que o método **deleteMessage** seja chamado, a mensagem será fornecida novamente ao aplicativo quando reiniciar. Isso é frequentemente chamado de **Processamento de pelo menos uma vez**, ou seja, cada mensagem será processada pelo menos uma vez mas, em algumas situações, a mesma mensagem poderá ser entregue novamente. Se o cenário não tolerar o processamento duplicado, os desenvolvedores de aplicativos deverão adicionar lógica extra ao aplicativo para tratar a entrega de mensagem duplicada. Isso geralmente é obtido com a propriedade **MessageId** da mensagem, que permanecerá constante nas tentativas de entrega.

## Próximas etapas

Para saber mais sobre filas, veja os seguintes recursos.

-   [Filas, tópicos e assinaturas][]
-   Repositório do [SDK do Azure para Node][] no GitHub
-   [Centro de Desenvolvedores do Node.js](/develop/nodejs/)

  [SDK do Azure para Node]: https://github.com/Azure/azure-sdk-for-node
  [portal clássico do Azure]: http://manage.windowsazure.com
  
  [Serviço de Nuvem do Node.js ]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Filas, tópicos e assinaturas]: service-bus-queues-topics-subscriptions.md
  [Criar e implantar um aplicativo Node.js em um site do Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Serviço de Nuvem do Node.js com Armazenamento]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Aplicativo Web do Node.js com armazenamento]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Cotas do Barramento de Serviço]: service-bus-quotas.md
 

<!---HONumber=AcomDC_0928_2016-->