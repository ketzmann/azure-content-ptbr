<properties
	pageTitle="Tutorial: Integração do Active Directory do Azure ao Flatter Files | Microsoft Azure"
	description="Saiba como configurar o logon único entre o Active Directory do Azure e o Flatter Files."
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
	ms.date="07/11/2016"
	ms.author="jeedes"/>


# Tutorial: Integração do Active Directory do Azure ao Flatter Files

O objetivo deste tutorial é mostrar como integrar o Flatter Files ao Azure AD (Azure Active Directory). A integração do Flatter Files ao Azure AD oferece os seguintes benefícios:

- No AD do Azure, você pode controlar quem tem acesso ao Flatter Files
- Você pode permitir que seus usuários façam logon automaticamente no Flatter Files (logon único) com suas contas do AD do Azure
- Você pode gerenciar suas contas em um local central, o portal clássico do Active Directory do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## Pré-requisitos 

Para configurar a integração do AD do Azure ao Flatter Files, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Flatter Files com logon único habilitado


> [AZURE.NOTE] Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

 
## Descrição do cenário
O objetivo deste tutorial é permitir que você teste o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Flatter Files da galeria
2. Configurar e testar o logon único do AD do Azure


## Adicionando Flatter Files da galeria
Para configurar a integração do Flatter Files ao AD do Azure, você precisará adicionar o Flatter Files da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar o Flatter Files da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Active Directory][1]

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

	![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.

	![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

	![Aplicativos][4]

6. Na caixa de pesquisa, digite **Flatter Files**.


	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. No painel de resultados, escolha **Flatter Files** e clique em **Concluir** para adicionar o aplicativo.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  Configurar e testar o logon único do AD do Azure
O objetivo desta seção é mostrar como configurar e testar logon único do AD do Azure com o Flatter Files, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o AD do Azure precisa saber qual usuário do Flatter Files é equivalente a um usuário do AD do Azure. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Flatter Files. Essa relação de vínculo é estabelecida atribuindo o valor de **nome de usuário** no AD do Azure ao valor de **Nome de Usuário** no Flatter Files.
 
Para configurar e testar o logon único do AD do Azure com o Flatter Files, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o Logon único do AD do Azure](#configuring-azure-ad-single-single-sign-on)**: para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)**: para testar o logon único do AD do Azure com Brenda Fernandes.
4. **[Criando um usuário de teste do Flatter Files](#creating-a-halogen-software-test-user)**: para ter um equivalente de Brenda Fernandes no Flatter Files que esteja vinculado à representação dela no AD do Azure.
5. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)**: para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)**: para verificar se a configuração funciona.

### Configuração do logon único do Azure AD

O objetivo desta seção é habilitar o logon único do AD do Azure no portal clássico do AD do Azure e configurar o logon único em seu aplicativo Flatter Files. Como parte deste procedimento, será necessário criar um arquivo de certificado codificado em base 64. Se você não estiver familiarizado com este procedimento, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

Para configurar o logon único para o Flatter Files, você precisará de um domínio registrado. Se não tiver um domínio registrado, entre em contato com a equipe de suporte do Flatter Files pelo email [support@flatterfiles.com](mailto:support@flatterfiles.com).



**Para configurar o logon único do AD do Azure com o Flatter Files, execute as seguintes etapas:**

1. No portal clássico do Azure AD, na página de integração de aplicativos do **Flatter Files**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.

	![Configurar o logon único][6]

2. Na página **Como você deseja que os usuários façam logon no Flatter Files**, escolha **Logon único do Azure AD** e clique em **Avançar**.
 
	![Configurar o logon único](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png)

3. Na página de diálogo **Definir Configurações do Aplicativo**, clique em **Avançar**.

	![Configurar o logon único](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png)

    > [AZURE.NOTE] O Flatter Files usa a mesma URL de SSO para todos os clientes: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
 
 
4. Na página **Configurar logon único no Flatter Files**, execute as seguintes etapas:

	![Configurar o logon único](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)

    a. Clique em **Baixar certificado** e salve o arquivo em seu computador.

    b. Clique em **Próximo**.


1. Faça logon no aplicativo Flatter Files como administrador.

2. Clique em Painel.

	![Configurar o logon único](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)



2. Clique em **Configurações** e execute as seguintes etapas na guia **Empresa**:

	![Configurar o logon único](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)

    a. Selecione **Usar SAML 2.0 para Autenticação**.

    b. Clique em **Configurar SAML**.



2. No diálogo **Configuração de SAML**, execute as seguintes etapas:

	![Configurar o logon único](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)

    a. Na caixa de texto Domínio, digite seu domínio registrado.

    > [AZURE.NOTE] Se não tiver um domínio registrado, entre em contato com a equipe de suporte do Flatter Files pelo email [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. No portal clássico do Azure, na caixa de diálogo Configurar logon único no Flatter Files, copie a URL do Serviço de Logon Único e cole-a na caixa de texto URL do Provedor de Identidade.

    c. Crie um arquivo **codificado em base 64** usando o certificado baixado.

    >[AZURE.TIP] Para obter mais detalhes, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)

    d. Abra seu certificado codificado em Base 64 no bloco de notas, copie o conteúdo dele na área de transferência e cole-o na caixa de texto **Certificado do Provedor de Identidade do Flatter Files**.

    e. Clique em **Atualizar**.

6. No portal clássico do Azure AD, selecione a confirmação de configuração de logon único e clique em **Avançar**.

	![Logon único do AD do Azure][10]

7. Na página **Confirmação de logon único**, clique em **Concluir**.

	![Logon único do AD do Azure][11]




### Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png)

2. Na lista **Diretório**, selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png)
 
4. Para abrir o diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png)

5. Na página de diálogo **Conte-nos sobre este usuário**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)

    a. Em Tipo de Usuário, selecione Novo usuário na organização.

    b. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.

    c. Clique em **Próximo**.

6.  Na página de diálogo **Perfil do Usuário**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png)
 
    a. Na caixa de texto **Nome**, digite **Brenda**.

    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.

    c. Na caixa de texto **Nome de exibição**, digite **Brenda Fernandes**.

    d. Na lista **Função**, selecione **Usuário**. e. Clique em **Próximo**.

7. Na página de diálogo **Obter senha temporária**, clique em **criar**.

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png)
 
8. Na página de diálogo **Obter senha temporária**, execute as seguintes etapas:

	![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png)
  
    a. Anote o valor da **Nova Senha**.

    b. Clique em **Concluído**.

  
 
### Criação de um usuário de teste do Flatter Files

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Flatter Files.

**Para criar um usuário chamado Brenda Fernandes no Flatter Files, execute as seguintes etapas:**

1. Faça logon no site da empresa **Flatter Files** como administrador.

2. No painel de navegação à esquerda, clique em **Configurações** e na guia **Usuários**.

	![Criar um Usuário do Flatter Files](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Clique em **Adicionar Usuário**.

4. No diálogo **Adicionar Usuário**, realize as seguintes etapas:

	![Criar um Usuário do Flatter Files](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. Na caixa de texto **Nome**, digite **Brenda**.

    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.

    c. Na caixa de texto **Endereço de Email**, digite o endereço de email de Brenda no portal clássico do Azure.

    d. Clique em **Enviar**.


### Atribuição do usuário de teste do AD do Azure

O objetivo desta seção é permitir que Brenda Fernandes use o logon único do Azure, concedendo a ela acesso ao Flatter Files.

![Atribuir usuário][200]

**Para atribuir Brenda Fernandes ao Flatter Files, execute as seguintes etapas:**

1. No portal clássico do Azure, para abrir o modo de exibição de aplicativos, na exibição de diretório, clique em **Aplicativos** no menu superior.

	![Atribuir usuário][201]

2. Na lista de aplicativos, escolha **Flatter Files**.

	![Atribuir usuário](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png)

1. No menu na parte superior, clique em **Usuários**.

	![Atribuir usuário][203]

1. Na lista de usuários, selecione **Brenda Fernandes**.

2. Na barra de ferramentas na parte inferior, clique em **Atribuir**.

	![Atribuir usuário][205]



### Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso. Quando você clica no bloco Flatter Files no Painel de Acesso, deve fazer logon automaticamente no seu aplicativo Flatter Files.


## Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0713_2016-->