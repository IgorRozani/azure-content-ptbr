<properties
	pageTitle="Gerenciar os grupos nos quais seu grupo é um membro na visualização do Azure Active Directory | Microsoft Azure"
	description="Os grupos podem conter outros grupos no Azure Active Directory. Veja como gerenciar essas associações."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/12/2016"
	ms.author="curtand"/>


# Gerenciar os grupos nos quais seu grupo é um membro na visualização do Azure Active Directory

Os grupos podem conter outros grupos na visualização do Azure Active Directory. [O que há na visualização?](active-directory-preview-explainer.md) Veja como gerenciar essas associações.

## Como localizo os grupos dos quais meu grupo é um membro?

1.  Entre no [Portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.

2.  Escolha **Mais serviços**, insira **Usuários e grupos** na caixa de texto e selecione **Enter**.

  ![Abrir o gerenciamento de usuários](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Na folha **Usuários e grupos**, escolha **Todos os grupos**.

  ![Abrir a folha de grupos](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Na folha **Usuários e grupos - Todos os grupos**, escolha um grupo.

5. Na folha **Grupo - *nome do grupo***, escolha **Associações de grupo**.

  ![Abrir a folha de associações de grupo](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Para adicionar seu grupo como um membro de outro grupo, na folha **Grupo - Associações de grupo**, escolha o comando **Adicionar**.

7. Escolha um grupo na folha **Selecionar Grupo** e escolha o botão **Selecionar** na parte inferior da folha. Você só pode adicionar o grupo a um grupo por vez. A caixa **Usuário** filtra a exibição com base na correspondência de sua entrada com qualquer parte de um nome de usuário ou dispositivo. Caracteres curinga não são aceitos nessa caixa.

  ![Adicionar uma associação de grupo](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Para remover seu grupo como um membro de outro grupo, na folha **Grupo - Associações de grupo**, escolha um grupo.

9. Na folha ***nome do grupo***, escolha o comando **Remover** e confirme sua opção na solicitação.

  ![comando remover associação](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Quando terminar de alterar as associações de grupo para o grupo, escolha **Salvar**.


## Informações adicionais

Esses artigos fornecem mais informações sobre o Active Directory do Azure.

* [Ver grupos existentes](active-directory-groups-view-azure-portal.md)
* [Criar um novo grupo e adicionando membros](active-directory-groups-create-azure-portal.md)
* [Gerenciar configurações de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerenciar membros de um grupo](active-directory-groups-members-azure-portal.md)
* [Gerenciar regras dinâmicas para usuários em um grupo](active-directory-groups-dynamic-membership-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->