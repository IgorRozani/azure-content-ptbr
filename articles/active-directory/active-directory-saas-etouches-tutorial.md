<properties
	pageTitle="Tutorial: Integração do Azure Active Directory com o eTouches | Microsoft Azure"
	description="Saiba como configurar o logon único entre o Azure Active Directory e o eTouches."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/03/2016"
	ms.author="jeedes"/>


# Tutorial: integração do Azure Active Directory com o eTouches

Neste tutorial, você aprenderá a integrar o eTouches ao Azure AD (Azure Active Directory).

A integração do eTouches ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao eTouches
- Você pode permitir que os usuários façam logon automaticamente no eTouches (Logon Único) com suas contas do Azure AD
- Gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## Pré-requisitos

Para configurar a integração do Azure AD com o eTouches, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do eTouches


> [AZURE.NOTE] Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste.

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do eTouches da galeria
2. Configurar e testar o logon único do AD do Azure


## Adição do eTouches da galeria
Para configurar a integração do eTouches ao Azure AD, você precisará adicionar o eTouches da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o eTouches por meio da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Active Directory][1]
2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

	![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.

	![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

	![Aplicativos][4]

6. Na caixa de pesquisa, digite **eTouches**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_01.png)

7. No painel de resultados, selecione **eTouches** e clique em **Concluir** para adicionar o aplicativo.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_02.png)

##  Configurar e testar o logon único do AD do Azure

Nesta seção, você configurará e testará o logon único do Azure AD com o eTouches, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do eTouches é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do eTouches.

Essa relação de vínculo é estabelecida atribuindo o valor de **nome de usuário** ao Azure AD como sendo o valor de **Nome de usuário** no eTouches.

Para configurar e testar o logon único do Azure AD com o eTouches, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o Logon único do AD do Azure](#configuring-azure-ad-single-sign-on)**: para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)**: para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criar um usuário de teste do eTouches](#creating-a-predictix-price-reporting-test-user)** - para ter um equivalente de Brenda Fernandes no eTouches que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)**: para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)**: para verificar se a configuração funciona.

### Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal clássico e configurará o logon único em seu aplicativo eTouches.

O aplicativo eTouches espera que as declarações SAML estejam em um formato específico. Configure as seguintes declarações para o aplicativo. Você pode gerenciar o valor dos atributos na guia **"Atributo"** do aplicativo. A captura de tela a seguir mostra um exemplo disso.

![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_07.png)

**Para configurar o logon único do Azure AD com o eTouches, execute as seguintes etapas:**


1. No portal clássico do Azure, na página de integração do aplicativo **eTouches**, no menu superior, clique em **Atributos**.

	![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_general_80.png)


2. Na caixa de diálogo **Atributos de token SAML**, para cada linha mostrada na tabela a seguir, execute as seguintes etapas:

	| Nome do atributo | Valor do atributo |
	| --- | --- |    
	| Email | user.mail |

	a. Clique em **adicionar atributo de usuário** para abrir o diálogo **Adicionar Atributo de Usuário**.

	![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_general_81.png)


	b. Na caixa de texto **Nome do Atributo**, digite o nome do atributo mostrado para essa linha.

    c. Na lista **Valor do Atributo**, selecione o valor do atributo mostrado para essa linha.

    d. Clique em **Concluído**.
	

3. No portal clássico, na página de integração de aplicativos do **eTouches**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
	 
	![Configurar o logon único][6]

4. Na página **Como você deseja que os usuários façam logon no eTouches**, selecione **Logon Único do Azure AD** e clique em **Avançar**.

	![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_03.png)

5. Na página de diálogo **Definir Configurações de Aplicativo**, execute as seguintes etapas:

	![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_04.png)

    a. Na caixa de texto **URL de Entrada**, digite a URL usada pelos usuários para entrar em seu aplicativo eTouches usando o seguinte padrão: **https://www.eiseverywhere.com/saml/accounts/?sso&accountid=\<iddaconta>**.
	
	b. Clique em **Avançar**
 
6. Na página **Configurar logon único no eTouches**, execute as seguintes etapas:

	![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_05.png)

    a. Clique em **Baixar metadados** e salve o arquivo no computador.

    b. Clique em **Próximo**.


7. Para que o SSO seja configurado para o aplicativo, execute as seguintes etapas no aplicativo eTouches:

	a. Faça logon no aplicativo **eTouches** usando os direitos de Administrador.
	
	b. Vá para a configuração de **SAML**

	c. Na seção **General Settings**, cole o conteúdo dos Metadados de Federação do Azure AD na caixa de texto.

	d. Clique no botão **Save & Stay**

	e. Clique no botão **Update Metadata** na seção SAML Metadata.

	f. Isso abrirá a página e executará o SSO. Depois que o SSO estiver funcionando, você poderá configurar o nome de usuário

	g. No campo **Username**, selecione o **emailaddress** como mostrado na imagem abaixo.

	h. Copie o valor de **SSO URL / ACS** e coloque-o na caixa de texto URL de Entrada do assistente de configuração de aplicativos do Azure AD.

	![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png)

8. No portal clássico, selecione a confirmação da configuração de logon único e clique em **Avançar**.
	
	![Logon único do AD do Azure][10]

9. Na página **Confirmação de logon único**, clique em **Concluir**.
	
 
	![Logon único do AD do Azure][11]


### Criação de um usuário de teste do AD do Azure
Nesta seção, você criará uma usuária de teste no portal clássico chamada Brenda Fernandes.


![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/create_aaduser_09.png)

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png)

4. Para abrir o diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png)

5. Na página de diálogo **Conte-nos sobre este usuário**, execute as seguintes etapas: ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/create_aaduser_05.png)

    a. Em Tipo de Usuário, selecione Novo usuário na organização.

    b. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.

    c. Clique em **Próximo**.

6.  Na página de diálogo **Perfil do Usuário**, execute as seguintes etapas: ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/create_aaduser_06.png)

    a. Na caixa de texto **Nome**, digite **Brenda**.

    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.

    c. Na caixa de texto **Nome de exibição**, digite **Brenda Fernandes**.

    d. Na lista **Função**, selecione **Usuário**.

    e. Clique em **Próximo**.

7. Na página de diálogo **Obter senha temporária**, clique em **criar**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/create_aaduser_07.png)

8. Na página de diálogo **Obter senha temporária**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-etouches-tutorial/create_aaduser_08.png)

    a. Anote o valor da **Nova Senha**.

    b. Clique em **Concluído**.



### Criação de um usuário de teste no eTouches

Nesta seção, você criará um usuário chamado Brenda Fernandes no eTouches. Trabalhe com a equipe de suporte do eTouches para adicionar usuários na plataforma eTouches.


### Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao eTouches.

![Atribuir usuário][200]

**Para atribuir Brenda Fernandes ao eTouches, execute as seguintes etapas:**

1. No portal clássico, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.

	![Atribuir usuário][201]

2. Na lista de aplicativos, escolha **eTouches**.

	![Configurar o logon único](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_50.png)

3. No menu na parte superior, clique em **Usuários**.

	![Atribuir usuário][203]

4. Na lista de usuários, selecione **Brenda Fernandes**.

5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.

	![Atribuir usuário][205]


### Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco eTouches no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo eTouches.


## Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0803_2016-->