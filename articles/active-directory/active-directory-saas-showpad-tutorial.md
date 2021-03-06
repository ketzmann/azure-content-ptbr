<properties
	pageTitle="Tutorial: Integração do Azure Active Directory ao Showpad | Microsoft Azure"
	description="Saiba como configurar o logon único entre o Azure Active Directory e o Showpad."
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
	ms.date="06/06/2016"
	ms.author="jeedes"/>


# Tutorial: Integração do Azure Active Directory com o Showpad

O objetivo deste tutorial é mostrar como integrar o Showpad ao Azure AD (Azure Active Directory).

A integração do Showpad ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Showpad
- Você pode permitir que os usuários façam logon automaticamente no Showpad (Logon Único) com suas contas do AD do Azure
- Você pode gerenciar suas contas em um local central – o Portal do Active Directory do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## Pré-requisitos

Para configurar a integração do Azure AD com o Showpad, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Showpad


> [AZURE.NOTE] Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## Descrição do cenário
O objetivo deste tutorial é permitir que você teste o logon único do Azure AD em um ambiente de teste.

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do Showpad da galeria
2. Configurar e testar o logon único do AD do Azure


## Adição do Showpad da galeria
Para configurar a integração do Showpad ao Azure AD, você precisará adicionar o Showpad à sua lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Showpad da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 

	![Aplicativos][1]

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

	![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.

	![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
 
	![Aplicativos][4]

6. Na caixa de pesquisa, digite **Showpad**.

	![Aplicativos](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_01.png)

7. No painel de resultados, selecione **Showpad** e clique em **Concluir** para adicionar o aplicativo.

	![Aplicativos](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_02.png)

##  Configurar e testar o logon único do AD do Azure
O objetivo desta seção é mostrar como configurar e testar o logon único do Azure AD com o Showpad, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Showpad é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Showpad.

Essa relação de vínculo é estabelecida atribuindo o valor de **nome de usuário** no Azure AD como o valor de **Nome de Usuário** no Showpad.

Para configurar e testar o logon único do Azure AD com o Showpad, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o Logon único do AD do Azure](#configuring-azure-ad-single-single-sign-on)**: para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)**: para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criar um usuário de teste do Showpad](#creating-a-showpad-test-user)** – para ter um equivalente de Brenda Fernandes no Showpad que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)**: para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)**: para verificar se a configuração funciona.

### Configuração do logon único do AD do Azure

O objetivo desta seção é habilitar o logon único do Azure AD no portal clássico do Azure e configurar o logon único em seu aplicativo do Showpad.



**Para configurar o logon único do Azure AD com o Showpad, execute as seguintes etapas:**

1. No portal clássico do Azure, na página de integração do aplicativo **Showpad**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.

	![Configurar o logon único][6]

2. Na página **Como você deseja que os usuários façam logon no Showpad**, selecione **Logon Único do Azure AD** e clique em **Avançar**.

	![Configurar o logon único](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_03.png)

3. Na página de diálogo **Definir Configurações do Aplicativo**, execute as seguintes etapas e, em seguida, clique em **Avançar**:

	![Configurar o logon único](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_04.png)


    a. Na caixa de texto **URL de Entrada**, digite a URL usada pelos usuários para fazer logon em seu aplicativo do Showpad usando o seguinte padrão: `https://<company name>.showpad.biz/login`

	b. Na caixa de texto **Identificador**, digite a URL no seguinte padrão: `https://<company name>.showpad.biz`

	c. Clique em **Próximo**.


4. Na página **Configurar logon único no Showpad**, execute as seguintes etapas e clique em **Avançar**:

	![Configurar o logon único](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_05.png)

    a. Clique em **Baixar metadados** e salve o arquivo no computador.

    b. Clique em **Próximo**.


5. Faça logon no seu locatário do Showpad como administrador.

6. No menu na parte superior, clique em **Configurações**.

	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png)

7. Navegue até "**Logon Único**"e clique em"**Habilitar**".
 
	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

8. No diálogo **Adicionar um Serviço SAML 2.0**, execute as seguintes etapas:

	![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png)

	a. Na caixa de texto **Nome**, digite o nome do Provedor do Identificador (por exemplo: o nome da sua empresa).

	b. Como **Fonte de Metadados**, selecione **XML**.

	c. Copie o conteúdo do arquivo de metadados baixado e cole-o na caixa de texto **XML de Metadados**.

	d. Selecione **Provisionar automaticamente contas para novos usuários quando eles fizerem logon**.

	e. Clique em **Enviar**.


10. No portal clássico do Azure, selecione a confirmação da configuração de logon único e, em seguida, clique em **Avançar**.

	![Logon único do AD do Azure][10]


11. Na página **Confirmação de logon único**, clique em **Concluir**.
  
	![Logon único do AD do Azure][11]






### Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste do Showpad no Azure AD, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-showpad-tutorial/create_aaduser_09.png)

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png)

4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png)

5. Na página do diálogo **Conte-nos sobre este usuário**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-showpad-tutorial/create_aaduser_05.png)

    a. Na caixa de texto **Nome de Usuário**, digite **BrendaFernandes**.

    b. Clique em **Próximo**.

6.  Na página da caixa de diálogo **Perfil do Usuário**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-showpad-tutorial/create_aaduser_06.png)

    a. Na caixa de texto **Nome**, digite **Brenda**.

    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.

    c. Na caixa de texto **Nome de exibição**, digite **Brenda Fernandes**.

    d. Para **Função**, selecione **Usuário**.

    e. Clique em **Próximo**.

7. Na página do diálogo **Obter senha temporária**, clique em **Criar**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-showpad-tutorial/create_aaduser_07.png)

8. Na página da caixa de diálogo **Obter senha temporária**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-showpad-tutorial/create_aaduser_08.png)

    a. Anote o valor da **Nova Senha**.

    b. Clique em **Concluído**.


### Criar um usuário de teste do Showpad

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Showpad.

O Showpad dá suporte ao provisionamento just-in-time. Você já habilitou o provisionamento em **[Configurando o Logon Único do Azure AD](#configuring-azure-ad-single-single-sign-on)**.

Não há itens de ação para você nesta seção.




### Atribuição do usuário de teste do AD do Azure

O objetivo desta seção é permitir que Brenda Fernandes use o logon único do Azure, concedendo a ela acesso ao Showpad.

![Atribuir usuário][200]

**Para atribuir Brenda Fernandes ao Showpad, execute as seguintes etapas:**

1. No portal clássico do Azure, no menu na parte superior, clique em **Aplicativos**.

	![Atribuir usuário][201]

2. Na lista de aplicativos, selecione **Showpad**.

	![Configurar o logon único](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_50.png)

1. No menu na parte superior, clique em **Usuários**.

	![Atribuir usuário][203]

1. Na lista de usuários, selecione **Brenda Fernandes**.

2. Na barra de ferramentas na parte inferior, clique em **Atribuir**.

	![Atribuir usuário][205]




### Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clica no bloco **Showpad** no Painel de Acesso, deve ser conectado automaticamente ao seu aplicativo Showpad.


## Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0608_2016-->