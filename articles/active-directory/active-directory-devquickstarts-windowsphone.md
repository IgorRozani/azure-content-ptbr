<properties
	pageTitle="Introdução ao Windows Phone no AD do Azure | Microsoft Azure"
	description="Como criar um aplicativo do Windows Phone que se integre ao AD do Azure para entrar e chame APIs protegidas do AD do Azure usando OAuth."
	services="active-directory"
	documentationCenter="windows"
	authors="dstrockis"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="mobile-windows-phone"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="dastrock"/>



# Integrar o AD do Azure com um aplicativo do Windows Phone

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Se você estiver desenvolvendo um aplicativo do Windows Phone 8.1, o Azure AD torna simples e direto autenticar os usuários com suas contas do Active Directory. Ele também permite que seu aplicativo consuma com segurança qualquer API da Web protegida pelo AD do Azure, como as APIs do Office 365 ou a API do Azure.

> [AZURE.NOTE] Este exemplo de código usa ADAL v2.0. Para a tecnologia mais recente, é aconselhável experimentar nosso [Tutorial Universal do Windows usando o ADAL v3.0](active-directory-devquickstarts-windowsstore.md). Se você realmente estiver criando um aplicativo para Windows Phone 8.1, esse é o lugar certo. O ADAL v2.0 ainda é totalmente suportado e é a maneira recomendada de desenvolvimento de aplicativos para o Windows Phone 8.1 usando o Azure AD.

Para clientes nativos .NET que precisam acessar recursos protegidos, o AD do Azure fornece a biblioteca de autenticação do Active Directory, ou ADAL. Única finalidade da ADAL é tornar mais fácil para seu aplicativo obter tokens de acesso. Para demonstrar como isso é fácil, aqui vamos criar um aplicativo "Directory Searcher" do Windows Phone 8.1 que:

-	Obtém tokens de acesso para chamar a Graph API do AD do Azure usando o [protocolo de autenticação OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-	Pesquisa um diretório para usuários com um determinado UPN.
-	Desconecta usuários.

Para compilar o aplicativo em funcionamento completo, você precisará:

2. Registrar seu aplicativo no Azure AD.
3. Instalar e configurar a ADAL.
5. Usar a ADAL para obter tokens do AD do Azure.

Para começar, [baixe um projeto de esqueleto](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) ou [baixe o exemplo concluído](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip). Cada um é uma solução do Visual Studio 2013. Você também precisará de um locatário do AD do Azure no qual você possa criar usuários e registrar um aplicativo. Se você ainda não tiver um locatário [saiba como obter um](active-directory-howto-tenant.md).

## *1. Registrar o aplicativo Directory Searcher*
Para habilitar seu aplicativo para obter tokens, primeiro será necessário registrá-lo no seu locatário do AD do Azure e conceder permissão para acessar a Graph API do AD do Azure:

-	Entre no [Portal de Gerenciamento do Azure](https://manage.windowsazure.com)
-	Clique em **Active Directory** no painel de navegação à esquerda
-	Selecione um locatário no qual registrar o aplicativo.
-	Clique na guia **Aplicativos** e clique em **Adicionar** na lista de botões.
-	Siga os prompts e crie um novo **Aplicativo cliente nativo**.  

    -	O **Nome** do aplicativo descreverá seu aplicativo para os usuários finais
    -	O **URI de redirecionamento** é uma combinação de esquema e de cadeia de caracteres que o AD do Azure usará para retornar respostas de tokens. Insira um valor de espaço reservado por enquanto, por exemplo, `http://DirectorySearcher`. Substituiremos este valor posteriormente.
-	Depois de concluir o registro, o AAD atribuirá a seu aplicativo um identificador de cliente único. Você precisará desse valor nas próximas seções, então copie-o da guia **Configurar**.
- Também na guia **Configurar**, clique na seção “Permissões para outros aplicativos”. Para o aplicativo "Active Directory do Azure", adicione a permissão **Acessar o diretório de sua organização** em **Permissões delegadas**. Isso permitirá que seu aplicativo consulte a Graph API para usuários.

## *2. Instalar e configurar a ADAL*
Agora que você tem um aplicativo no AD do Azure, você pode instalar a ADAL e escrever seu código relacionado à identidade. Para que a ADAL possa se comunicar com o Azure AD, é necessário fornecer a ela algumas informações sobre o registro do aplicativo.
-	Comece adicionando o ADAL ao projeto DirectorySearcher usando o Console do Gerenciador de Pacotes.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-	No projeto DirectorySearcher, abra `MainPage.xaml.cs`. Substitua os valores na região `Config Values` para refletir os valores inseridos por você no Portal do Azure. Seu código fará referência a esses valores sempre que ele usar a ADAL.
    -	O `tenant` é o domínio do seu locatário do AD do Azure, por exemplo, contoso.onmicrosoft.com.
    -	O `clientId` é a clientId do seu aplicativo que você copiou do portal.
-	Agora você precisa descobrir o URI de retorno de chamada para seu aplicativo do Windows Phone. Defina um ponto de interrupção nessa linha no método `MainPage`:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Execute o aplicativo e copie separadamente o valor de `redirectUri` quando o ponto de interrupção for atingido. O resultado deve ser semelhante a

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- De volta à guia **Configurar** do seu aplicativo no Portal de Gerenciamento do Azure, substitua o valor de **RedirectUri** por esse valor.

## *3. Usar a ADAL para obter tokens do AD do Azure*
O princípio básico da ADAL é que sempre que seu aplicativo precisa de um token de acesso, ele simplesmente chama `authContext.AcquireToken(…)`, e a ADAL faz o resto.

-	A primeira etapa é inicializar o `AuthenticationContext` de seu aplicativo - classe principal da ADAL. É aqui que você passa à ADAL as coordenadas necessárias para se comunicar com o AD do Azure e informar a ele como armazenar tokens em cache.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

- Agora localize o método `Search(...)`, que será chamado quando os usuário clicar no botão "Pesquisar" na interface do usuário do aplicativo. Esse método faz uma solicitação GET para que a Graph API do AD do Azure procure por usuários cujo UPN começa com o termo de pesquisa fornecido. Mas para consultar a Graph API, você precisa incluir um access\_token no cabeçalho `Authorization` da solicitação - é aí que entra a ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
- Se for necessário usar autenticação interativa, a ADAL usará o WAB (Web Authentication Broker, agenciador de autenticação da Web) do Windows Phone e [modelo de continuação](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) para exibir a página de logon do AD do Azure. Quando o usuário faz logon, seu aplicativo precisa passar à ADAL os resultados da interação com o WAB. Isso é tão simples quanto implementar a interface `ContinueWebAuthentication`:

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- Agora é hora de usar o `AuthenticationResult` que a ADAL retornou ao seu aplicativo. No retorno de chamada `QueryGraph(...)`, anexe o access\_token que você adquiriu à solicitação GET, no cabeçalho Autorização:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
- Você também pode usar o objeto `AuthenticationResult` para exibir informações sobre o usuário em seu aplicativo. No método `QueryGraph(...)`, use o resultado para mostrar a id do usuário na página:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Por fim, você também pode usar a ADAL para realizar o logout do usuário do aplicativo. Quando o usuário clica no botão "Sair", queremos garantir que a próxima chamada para `AcquireTokenSilentAsync(...)` falhará. Com a ADAL, isso é tão fácil quanto limpar o cache de tokens:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Parabéns! Agora você tem um aplicativo do Windows Phone que tem a capacidade de autenticar usuários, chamar APIs da Web com segurança usando OAuth 2.0 e obter informações básicas sobre o usuário. Se você ainda não fez isso, agora é o momento de preencher seu locatário com alguns usuários. Execute o aplicativo DirectorySearcher e faça logon com um desses usuários. Procure por outros usuários com base em seus UPNs. Feche o aplicativo e execute-o novamente. Observe como a sessão do usuário permanece intacta. Saia e faça logon novamente como outro usuário.

A ADAL facilita a incorporar todos esses recursos comuns de identidade em seu aplicativo. Ele se encarrega de todo o trabalho difícil para você - gerenciamento de cache, suporte a protocolo OAuth, apresentação de uma IU de logon ao usuário, atualização de tokens expirados e mais. Tudo o que você realmente precisa saber é uma única chamada à API, `authContext.AcquireToken*(…)`.

Para referência, o exemplo concluído (sem seus valores de configuração) é fornecido [aqui](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip). Agora você pode passar para cenários de identidade adicionais. Você pode desejar experimentar:

[Proteger uma API da Web .NET com o AD do Azure >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]

<!---HONumber=AcomDC_0921_2016-->