<properties
	pageTitle="Atribuindo funções de administrador no Active Directory do Azure | Microsoft Azure"
	description="Explica quais funções de administrador estão disponíveis com o Active Directory do Azure e como atribuí-las."
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
	ms.date="06/29/2016"
	ms.author="curtand"/>

# Atribuindo funções de administrador no Azure Active Directory

Usando o Azure Active Directory (Azure AD), você pode designar administradores separados para atender a diferentes funções. Esses administradores terão acesso a vários recursos no portal do Azure ou portal clássico do Azure e, dependendo da sua função, poderão criar ou editar usuários, atribuir funções administrativas a outros usuários, redefinir senhas de usuários, gerenciar licenças de usuários e gerenciar domínios, entre outras coisas. Um usuário ao qual é atribuída uma função administrativa terá as mesmas permissões em todos os serviços de nuvem que sua organização tenha assinado, independentemente de você ter atribuído a função no portal do Office 365 ou no portal clássico do Azure ou usando o módulo do Azure AD para Windows PowerShell.

As seguintes funções de administrador estão disponíveis:

- **Administrador de cobrança**: faz compras, gerencia as assinaturas, gerencia tíquetes de suporte e monitora a integridade do serviço.

- **Administrador global**: tem acesso a todos os recursos administrativos. A pessoa que se inscreve para a conta do Azure torna-se um administrador global. Somente os administradores globais podem atribuir outras funções de administrador. Pode haver mais de um administrador global na sua empresa.

	> [AZURE.NOTE] Na API do Graph da Microsoft, na API do Graph do Azure AD e no Azure AD PowerShell, essa função é identificada como "Administrador da Empresa".

- **Administrador de senha**: redefine as senhas, gerencia as solicitações de serviço e monitora a integridade do serviço. Administradores de senha podem redefinir senhas somente para os usuários e outros administradores de senha.

	> [AZURE.NOTE] Na API do Graph da Microsoft, na API do Graph do Azure AD e no Azure AD PowerShell, essa função é identificada como "Administrador da Assistência Técnica".

- **Administrador de serviço**: gerencia as solicitações de serviço e monitora a integridade do serviço.

	> [AZURE.NOTE] Para atribuir a função de administrador de serviços a um usuário, o administrador global deve primeiro atribuir permissões administrativas para o usuário no serviço, como o Exchange Online, e, em seguida, atribuir a função de administrador de serviço para o usuário no Portal de Gerenciamento do Azure.

- **Administrador de usuários**: redefine as senhas, monitora a integridade do serviço e gerencia contas de usuário, grupos de usuários e solicitações de serviço. Algumas limitações se aplicam às permissões de um administrador de gerenciamento de usuário. Por exemplo, eles não podem excluir um administrador global ou criar outros administradores. Além disso, eles não podem redefinir senhas para cobrança, globais e administradores de serviço.

- **Leitor de segurança**: acesso somente leitura a um número de recursos de segurança do Identity Protection Center, Privileged Identity Management, Monitorar Integridade de Serviço do Office 365 e Centro de Proteção do Office 365.

- **Administrador de segurança**: todas as permissões somente leitura da função **Leitor de segurança**, mais um número de permissões administrativas adicionais para os mesmos serviços: Identity Protection Center, Privileged Identity Management, Monitorar Integridade de Serviço do Office 365 e Centro de Proteção do Office 365.

## Permissões de administrador

### Administrador de cobrança

O que ele pode fazer | O que não pode fazer
------------- | -------------
<p>Exibir informações da empresa e do usuário</p><p>Gerenciar tíquetes de suporte do Office gerenciar</p><p>Executar operações de faturamento e compra para produtos do Office</p> | <p>Redefinir senhas de usuário</p><p>Criar e gerenciar modos de exibição do usuário</p><p>Criar, editar e excluir usuários e grupos e gerenciar licenças de usuário</p><p>Gerenciar domínios</p><p>Gerenciar informações da empresa</p><p>Delegar funções administrativas a outros</p><p>Usar a sincronização de diretório</p>

### Administrador global

O que ele pode fazer | O que não pode fazer
------------- | -------------
<p>Exibir informações da empresa e do usuário</p><p>Gerenciar tíquetes de suporte do Office</p><p>Executar operações de faturamento e compra para produtos do Office</p> <p>Redefinir senhas de usuário</p><p>Criar e gerenciar modos de exibição do usuário</p><p>Criar, editar e excluir usuários e grupos e gerenciar licenças de usuário</p><p>Gerenciar domínios</p><p>Gerenciar informações da empresa</p><p>Delegar funções administrativas a outros</p><p>Usar a sincronização de diretório</p><p>Habilitar ou desabilitar a autenticação multifator</p> | N/D

### Administrador de senha

O que ele pode fazer | O que não pode fazer
------------- | -------------
<p>Exibir informações da empresa e do usuário</p><p>Gerenciar tíquetes de suporte do Office</p><p>Redefinir senhas de usuário</p> | <p>Realizar operações de faturamento e compra para produtos do Office</p><p>Criar e gerenciar modos de exibição do usuário</p><p>Criar, editar e excluir usuários e grupos e gerenciar licenças de usuário</p><p>Gerenciar domínios</p><p>Gerenciar informações da empresa</p><p>Delegar funções administrativas a outros</p><p>Usar a sincronização de diretório</p>

### Administrador de serviço

O que ele pode fazer | O que não pode fazer
------------- | -------------
<p>Exibir informações da empresa e do usuário</p><p>Gerenciar tíquetes de suporte do Office</p> | <p>Redefinir senhas de usuários</p><p>Realizar operações de faturamento e compra para produtos do Office</p><p>Criar e gerenciar modos de exibição do usuário</p><p>Criar, editar e excluir usuários e grupos e gerenciar licenças de usuário</p><p>Gerenciar domínios</p><p>Gerenciar informações da empresa</p><p>Delegar funções administrativas a outros</p><p>Usar a sincronização de diretório</p>

### Administrador de usuários

O que ele pode fazer | O que não pode fazer
------------- | -------------
<p>Exibir informações da empresa e do usuário</p><p>Gerenciar tíquetes de suporte do Office</p><p>Redefinir senhas de usuário, com limitações. Eles não podem redefinir senhas para administradores de cobrança, globais e de serviço.</p><p>Criar e gerenciar modos de exibição do usuário</p><p>Criar, editar e excluir usuários e grupos e gerenciar licenças de usuário, com limitações. Eles não podem excluir um administrador global ou criar outros administradores.</p> | <p>Realizar operações de faturamento e de compra para produtos do Office</p><p>Gerenciar domínios</p><p>Gerenciar informações da empresa</p><p>Delegar funções administrativas a outros</p><p>Usar a sincronização de diretório</p><p>Habilitar ou desabilitar a autenticação multifator</p>

### Leitor de segurança

No | O que ele pode fazer
------------- | -------------
Identity Protection Center | Ler todos os relatórios de segurança e informações de configurações para recursos de segurança<ul><li>Antispam<li>Criptografia<li>Prevenção contra perda de dados<li>Antimalware<li>Proteção avançada contra ameaças<li>Anti-phishing<li>Regras de fluxo de mensagens
Privileged Identity Management | <p>Tem acesso somente leitura a todas as informações exibidas no Azure AD PIM: políticas e relatórios de atribuições de função do Azure AD, análises de segurança e, no futuro, acesso de leitura aos dados de política e relatórios para cenários além da atribuição de função do Azure AD.<p>**Não pode** se inscrever no Azure AD PIM nem fazer alterações nele. No portal do PIM ou por meio do PowerShell, alguém nesta função poderá ativar funções adicionais (por exemplo, administrador global ou administrador com função com privilégios) se o usuário for um candidato a elas.
<p>Monitorar Integridade de Serviço do Office 365</p><p>Centro de proteção do Office 365</p> | <ul><li>Ler e gerenciar alertas<li>ler políticas de segurança<li>Ler inteligência de ameaças, Cloud App Discovery e Quarentena ao Pesquisar e Investigar<li>Ler todos os relatórios

### Administrador de segurança

No | O que ele pode fazer
------------- | -------------
Identity Protection Center | <ul><li>Todas as permissões da função de Leitor de Segurança.<li>Além disso, a capacidade de executar todas as operações de IPC, exceto para a redefinição de senhas.
Privileged Identity Management | <ul><li>Todas as permissões da função de leitor de segurança.<li>**Não é possível** gerenciar associações de função ou configurações do Azure AD.
<p>Monitorar Integridade de Serviço do Office 365</p><p>Proteção do Office 365 | <ul><li>Todas as permissões da função de Leitor de Segurança.<li>Pode configurar todas as configurações no recurso de Proteção avançada contra ameaças (proteção contra malware e vírus, configuração de URL mal-intencionado, rastreamento de URL, etc).

## Detalhes sobre a função de administrador global

O administrador global tem acesso a todos os recursos administrativos. Por padrão, a pessoa que se inscreve para uma assinatura do Azure recebe a função de administrador global para o diretório. Somente os administradores globais podem atribuir outras funções de administrador.

## Atribuir ou remover funções de administrador

1. No portal clássico do Azure, clique em **Active Directory** e no nome do diretório de sua organização.

2. Na página **Usuários**, clique no nome de exibição do usuário que deseja editar.

3. Na lista **Função Organizacional**, selecione a função de administrador que você deseja atribuir a este usuário ou selecione **Usuário** se você quiser remover uma função de administrador existente.

4. No campo **Endereço de email alternativo**, digite um endereço de email. Este endereço de email é usado para notificações importantes, incluindo redefinição automática de senha, por isso, o usuário deve ser capaz de acessar a conta de email independentemente de poder acessar o Azure ou não.

5. Selecione **Permitir** ou **Bloquear** para especificar se deseja permitir que o usuário entre e acesse os serviços.

6. Especifique um local na lista suspensa **Local de Uso**.

7. Ao terminar, clique em **Salvar**.

## Próximas etapas

- Para saber mais sobre como alterar administradores para uma assinatura do Azure, veja [Como adicionar ou alterar as funções de administrador do Azure](../billing-add-change-azure-subscription-administrator.md)

- Para saber mais sobre como o acesso aos recursos é controlado no Microsoft Azure, confira [Noções básicas sobre o acesso aos recursos do Azure](active-directory-understanding-resource-access.md)

- Para obter mais informações sobre como o Azure Active Directory está relacionado à sua assinatura do Azure, consulte [Como as assinaturas do Azure estão associadas ao Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Gerenciar usuários](active-directory-create-users.md)

- [Gerenciar senhas](active-directory-manage-passwords.md)

- [Gerenciar grupos](active-directory-manage-groups.md)

<!---HONumber=AcomDC_0706_2016-->