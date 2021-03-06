<properties
	pageTitle="Tutorial de últimas notícias localizadas de Hubs de Notificação para iOS"
	description="Saiba como usar Hubs de notificação do barramento de serviço do Azure para enviar notificações localizadas de últimas notícias (iOS)."
	services="notification-hubs"
	documentationCenter="ios"
	authors="wesmc7777"
	manager="erikre"
	editor=""/>

<tags
	ms.service="notification-hubs"
	ms.workload="mobile"
	ms.tgt_pltfrm="ios"
	ms.devlang="objective-c"
	ms.topic="article"
	ms.date="06/29/2016"
	ms.author="wesmc"/>

# Usar Hubs de Notificação para enviar últimas notícias localizadas para dispositivos iOS

> [AZURE.SELECTOR]
- [C# da Windows Store](notification-hubs-windows-store-dotnet-send-localized-breaking-news)
- [iOS](notification-hubs-ios-send-localized-breaking-news)


##Visão geral

Este tópico mostra como usar o recurso [modelos](notification-hubs-templates-cross-platform-push-messages.md) dos Hubs de Notificação do Azure para difundir notificações de últimas notícias que foram localizadas por idioma e dispositivo. Neste tutorial você começa com o aplicativo iOS criado em [Usar Hubs de Notificação para enviar últimas notícias]. Ao concluir, você poderá se registrar em categorias de seu interesse, especificar um idioma no qual deseja receber as notificações e receber notificações por push para as categorias selecionadas nesse idioma.


Há duas partes que compõem esse cenário:

- o aplicativo iOS permite que dispositivos cliente especifiquem um idioma e se inscrevam em diferentes categorias de últimas notícias;

- o back-end transmite as notificações usando os recursos **tag** e **template** dos Hubs de Notificação do Azure.



##Pré-requisitos

Você já deve ter concluído o tutorial [Usar Hubs de Notificação para envio de últimas notícias] e ter o código disponível, porque este tutorial se baseia diretamente no código.

O uso do Visual Studio 2012 ou posterior é opcional.



##Conceitos de modelo

Em [Usar Hubs de Notificação para envio de últimas notícias] você criou um aplicativo que usou **marcas** para assinar notificações para diferentes categorias de notícias. No entanto, muitos aplicativos são destinados a vários mercados e requerem localização. Isso significa que o próprio conteúdo das notificações deve ser localizado e entregue para o conjunto de dispositivos correto. Neste tópico, mostraremos como usar o recurso **template** de Hubs de Notificação para entregar facilmente notificações de últimas notícias localizadas.

Observação: uma maneira de enviar notificações localizadas é criar várias versões de cada marca. Por exemplo, para oferecer suporte a inglês, francês e mandarim, precisamos de três marcas diferentes para notícias do mundo: "world\_en", "world\_fr" e "world\_ch". Em seguida, precisamos enviar uma versão localizada das notícias do mundo para cada uma dessas marcas. Neste tópico, usamos modelos para evitar a proliferação de marcas e a necessidade de enviar várias mensagens.

Em um alto nível, os modelos são uma maneira de especificar como um dispositivo específico deve receber uma notificação. O modelo especifica o formato exato da carga referindo-se às propriedades que fazem parte da mensagem enviada por seu aplicativo de back-end. Em nosso caso, enviaremos uma mensagem independente de localidade contendo todos os idiomas com suporte:

	{
		"News_English": "...",
		"News_French": "...",
		"News_Mandarin": "..."
	}

Em seguida, verificaremos se os dispositivos se registram com um modelo que faz referência à propriedade correta. Por exemplo, um aplicativo iOS que deseje se registrar para notícias em francês irá registrar o seguinte:

	{
		aps:{
			alert: "$(News_French)"
		}
	}

Os modelos são um recurso muito avançado sobre o qual você pode aprender em nosso artigo [Modelos](notification-hubs-templates-cross-platform-push-messages.md).

##A interface do usuário do aplicativo

Agora vamos modificar o aplicativo Breaking News que você criou no tópico [Usar Hubs de Notificação para envio de últimas notícias] para enviar últimas notícias localizadas usando modelos.


No seu MainStoryboard\_iPhone.storyboard, adicione um controle segmentado com três idiomas para os quais oferecemos suporte: inglês, francês e mandarim.

![][13]

Em seguida, adicione um IBOutlet em seu ViewController.h, conforme mostrado abaixo:

![][14]

##Criando o aplicativo iOS


1. No Notification.h, adicione o método *retrieveLocale* e modifique os métodos store e subscribe conforme mostrado abaixo:

		- (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

		- (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

		- (NSSet*) retrieveCategories;

		- (int) retrieveLocale;

	No Notification.m, modifique o método *storeCategoriesAndSubscribe* adicionando o parâmetro locale e armazenando-o nos padrões do usuário:

		- (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
		    NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
		    [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
		    [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
		    [defaults synchronize];

		    [self subscribeWithLocale: locale categories:categories completion:completion];
		}

	Em seguida, modifique o método *subscribe* para incluir o locale:

		- (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
		    SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

		    NSString* localeString;
		    switch (locale) {
		        case 0:
		            localeString = @"English";
		            break;
		        case 1:
		            localeString = @"French";
		            break;
		        case 2:
		            localeString = @"Mandarin";
		            break;
		    }

		    NSString* template = [NSString stringWithFormat:@"{"aps":{"alert":"$(News_%@)"},"inAppMessage":"$(News_%@)"}", localeString, localeString];

		    [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
		}

	Observe como agora estamos usando o método *registerTemplateWithDeviceToken*, em vez do *registerNativeWithDeviceToken*. Ao registrar um modelo, precisamos fornecer o modelo json e também um nome para o modelo (uma vez que nosso aplicativo talvez queira registrar diferentes modelos). Registre suas categorias como marcas, pois queremos garantir que receberemos as notificações sobre essas notícias.

	Adicione um método para recuperar o locale das configurações padrão do usuário:

		- (int) retrieveLocale {
		    NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

		    int locale = [defaults integerForKey:@"BreakingNewsLocale"];

		    return locale < 0?0:locale;
		}

2. Agora que modificamos nossa classe Notifications, temos que verificar que nosso ViewController use o novo UISegmentControl. Adicione a seguinte linha ao método *viewDidLoad* método para ter certeza de mostrar a localidade que está selecionado no momento:

		self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

	Em seguida, no seu método *subscribe*, altere sua chamada para *storeCategoriesAndSubscribe* ao seguinte:

		[notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
	        if (!error) {
	            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
	                                  @"Subscribed!" delegate:nil cancelButtonTitle:
	                                  @"OK" otherButtonTitles:nil, nil];
	            [alert show];
	        } else {
	            NSLog(@"Error subscribing: %@", error);
	        }
	    }];

3. Finalmente, você tem que atualizar o método *didRegisterForRemoteNotificationsWithDeviceToken* no seu AppDelegate.m, de forma que você possa atualizar corretamente seu registro quando seu aplicativo inicie. Altere sua chamada para o método *subscribe* de notificações com o seguinte:

		NSSet* categories = [self.notifications retrieveCategories];
	    int locale = [self.notifications retrieveLocale];
	    [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
	        if (error != nil) {
	            NSLog(@"Error registering for notifications: %@", error);
	        }
	    }];

##(opcional) Envie notificações de modelo localizado do aplicativo de console do .NET.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##(opcional) Envie notificações de modelo localizado do dispositivo

Se você não tem acesso ao Visual Studio ou deseja apenas testar o envio das notificações de modelo localizado diretamente do aplicativo no dispositivo. Você pode apenas adicionar os parâmetros de modelo localizado ao método `SendNotificationRESTAPI` definido no tutorial anterior.

		- (void)SendNotificationRESTAPI:(NSString*)categoryTag
		{
		    NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
									 defaultSessionConfiguration] delegate:nil delegateQueue:nil];

		    NSString *json;

		    // Construct the messages REST endpoint
		    NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
		                                       HUBNAME, API_VERSION]];

		    // Generated the token to be used in the authorization header.
		    NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

		    //Create the request to add the template notification message to the hub
		    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
		    [request setHTTPMethod:@"POST"];

		    // Add the category as a tag
		    [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

			// Template notification
	        json = [NSString stringWithFormat:@"{"messageParam":"Breaking %@ News : %@","
					"News_English":"Breaking %@ News in English : %@","
					"News_French":"Breaking %@ News in French : %@","
					"News_Mandarin":"Breaking %@ News in Mandarin : %@","
	                categoryTag, self.notificationMessage.text,
	                categoryTag, self.notificationMessage.text,  // insert English localized news here
	                categoryTag, self.notificationMessage.text,  // insert French localized news here
	                categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

	        // Signify template notification format
	        [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

			// JSON Content-Type
			[request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

		    //Authenticate the notification message POST request with the SaS token
		    [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

		    //Add the notification message body
		    [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

		    // Send the REST request
		    NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
		               completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
	           {
	           NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
	               if (error || httpResponse.statusCode != 200)
	               {
	                   NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
	               }
	               if (data != NULL)
	               {
	                   //xmlParser = [[NSXMLParser alloc] initWithData:data];
	                   //[xmlParser setDelegate:self];
	                   //[xmlParser parse];
	               }
	           }];

		    [dataTask resume];
		}




## Próximas etapas

Para obter mais informações sobre o uso de modelos, consulte:

- [Notificar usuários com Hubs de Notificação: ASP.NET]
- [Notificar usuários com Hubs de Notificação: Serviços Móveis]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Usar Hubs de Notificação para enviar últimas notícias]: /manage/services/notification-hubs/breaking-news-ios
[Usar Hubs de Notificação para envio de últimas notícias]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notificar usuários com Hubs de Notificação: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notificar usuários com Hubs de Notificação: Serviços Móveis]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx

<!---HONumber=AcomDC_0706_2016-->