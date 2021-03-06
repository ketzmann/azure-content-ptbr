<properties 
	pageTitle="Como gerenciar contas de usuário no Gerenciamento de API do Azure | Microsoft Azure" 
	description="Saiba como criar ou convidar usuários no Gerenciamento de API do Azure" 
	services="api-management" 
	documentationCenter="" 
	authors="steved0x" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/09/2016" 
	ms.author="sdanie"/>

# Como gerenciar contas de usuário no Gerenciamento de API do Azure

No Gerenciamento de API, os desenvolvedores são os usuários das APIs que você expõe utilizando o Gerenciamento d API. Este guia mostra como criar e convidar desenvolvedores a utilizarem as APIs e os produtos que você disponibilizar para eles com sua instância do Gerenciamento da API. Para saber mais sobre como gerenciar contas de usuário por meio de programação, confira a documentação sobre [Entidade Usuário](https://msdn.microsoft.com/library/azure/dn776330.aspx) na referência [REST de Gerenciamento de API](https://msdn.microsoft.com/library/azure/dn776326.aspx).

## <a name="create-developer"> </a>Criar um novo desenvolvedor

Para criar um novo desenvolvedor, clique em **Gerenciar** no Portal Clássico do Azure para acessar o serviço de Gerenciamento de API. Isso levará você ao portal do editor de Gerenciamento de API. Se você ainda não criou uma instância de serviço de Gerenciamento de API, consulte [Criar uma instância de serviço de Gerenciamento de API][] no tutorial [Introdução ao Gerenciamento de API do Azure][].

![Portal do editor][api-management-management-console]

Clique em **Usuários** no menu **Gerenciamento de API** à esquerda e depois clique em **adicionar usuário**.

![Criar desenvolvedor][api-management-create-developer]

Insira o **Email**, a **Senha** e o **Nome** do novo desenvolvedor e clique em **Salvar**.

![Criar desenvolvedor][api-management-add-new-user]

Por padrão, as contas de desenvolvedor criadas recentemente têm o estado **Ativa** e são associadas ao grupo **Desenvolvedores**.

![Novo desenvolvedor][api-management-new-developer]

As contas de desenvolvedor que estão com estado **ativa** podem ser utilizadas para acessar todas as APIs nas quais estão inscritas. Para associar um desenvolvedor recém-criado a grupos adicionais, consulte [Como associar grupos a desenvolvedores][].

## <a name="invite-developer"> </a>Convidar um desenvolvedor

Para convidar um desenvolvedor, clique em **Usuários** no menu **Gerenciamento de API** à esquerda e depois clique em **Convidar Usuário**.

![Convidar desenvolvedor][api-management-invite-developer]

Insira o nome e o endereço de email do desenvolvedor e clique em **Convidar**.

![Convidar desenvolvedor][api-management-invite-developer-window]

Uma mensagem de confirmação é exibida, mas o desenvolvedor recém-convidado não aparecerá na lista até que ele aceite o convite.

![Confirmação de convite][api-management-invite-developer-confirmation]

Quando um desenvolvedor é convidado, um email é enviado a ele. O email é gerado utilizando um modelo e pode ser personalizado. Para obter mais informações, consulte [Configurar modelos de email][].

Após o convite ser aceito, a conta se torna ativa.

## <a name="block-developer"> </a>Desativar ou reativar uma conta de desenvolvedor

Por padrão, as contas de desenvolvedor criadas ou convidadas recentemente têm o estado **Ativa**. Para desativar uma conta de desenvolvedor, clique em **Bloquear**. Para reativar uma conta de desenvolvedor bloqueada, clique em **Ativar**. Uma conta de desenvolvedor bloqueada não pode acessar o portal do desenvolvedor ou chamar quaisquer APIs. Para excluir uma conta de usuário, clique em **Excluir**.

![Bloquear desenvolvedor][api-management-new-developer]

## Redefinir a senha de um usuário

Para redefinir a senha de uma conta de usuário, clique no nome da conta.

![Redefinir senha][api-management-view-developer]

Clique em **Redefinir senha** para enviar um link para o usuário a fim de redefinir sua senha.

![Redefinir senha][api-management-reset-password]

Para trabalhar de forma programática com contas de usuário, confira a documentação sobre [Entidade Usuário](https://msdn.microsoft.com/library/azure/dn776330.aspx) na referência [REST de Gerenciamento de API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Para redefinir uma senha de conta de usuário para um valor específico, você pode usar a operação [Atualizar um usuário](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) e especificar a senha desejada.

## Verificação pendente

![Verificação pendente][api-management-pending-verification]

## <a name="next-steps"> </a>Próximas etapas

Após criar uma conta de desenvolvedor, você pode associá-la a funções e inscrevê-la em produtos e APIs. Para obter mais informações, consulte [Como criar e utilizar grupos][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Como criar e utilizar grupos]: api-management-howto-create-groups.md
[Como associar grupos a desenvolvedores]: api-management-howto-create-groups.md#associate-group-developer

[Introdução ao Gerenciamento de API do Azure]: api-management-get-started.md
[Criar uma instância de serviço de Gerenciamento de API]: api-management-get-started.md#create-service-instance
[Configurar modelos de email]: api-management-howto-configure-notifications.md#email-templates

<!---HONumber=AcomDC_0810_2016-->