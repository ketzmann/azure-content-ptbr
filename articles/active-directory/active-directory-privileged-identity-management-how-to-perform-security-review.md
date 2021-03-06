<properties
   pageTitle="Como executar uma análise de acesso | Microsoft Azure"
   description="Saiba como executar uma revisão com o aplicativo Azure Privileged Identity Management."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# Como executar uma análise de acesso no Azure AD Privileged Identity Management

O Azure AD (Active Directory) Privileged Identity Management simplifica como as empresas gerenciam as identidades com privilégios e o acesso a recursos no Azure AD e em outros Microsoft Online Services, como o Office 365 ou o Microsoft Intune.

Se uma função administrativa tiver sido atribuída a você, o administrador de função com privilégios da sua organização poderá solicitar que você revise e confirme regularmente a necessidade dessa função para seu trabalho. Você pode receber um email que inclui um link ou acessar diretamente o [portal do Azure](https://portal.azure.com). Siga as etapas neste artigo para executar a autorrevisão das suas funções atribuídas.

Se você for um administrador de função com privilégios interessado nas revisões de segurança, obtenha mais detalhes em [Como iniciar uma análise de segurança](active-directory-privileged-identity-management-how-to-start-security-review.md).

## Adicionar o aplicativo Privileged Identity Management

Você pode usar o aplicativo Azure AD PIM (Privileged Identity Management) no [portal do Azure](https://portal.azure.com/) para executar a revisão. Se você não tiver o aplicativo Azure AD Privileged Identity Management em seu portal, siga estas etapas para começar.

1. Entre no [Portal do Azure](https://portal.azure.com/).
2. Se sua organização tiver mais de um diretório, clique em seu nome de usuário no canto superior direito do portal do Azure e selecione o diretório no qual você vai operar.
3. Selecione **Novo** > **Segurança + Identidade** > **Azure AD Privileged Identity Management**.

	![Habilitar o PIM no portal][1]

4. Marque a opção **Fixar no painel** e clique no botão **Criar**. O Painel Privileged Identity Management será aberto.


## Aprovar ou negar acesso

Ao aprovar ou negar o acesso, você está apenas dizendo ao revisor se ainda usa essa função ou não. Escolha **Aprovar** se deseja permanecer na função ou **Negar** se você não precisa mais do acesso. Seu status não mudará imediatamente até que o revisor aplique os resultados. Siga estas etapas para localizar e concluir a análise de acesso:

1. No aplicativo PIM, selecione **Examinar o acesso com privilégios**. Se você tiver quaisquer análises de acesso pendentes, elas aparecerão na folha de análises do Acesso do Azure AD.
2. Selecione a análise que deseja concluir.
3. A menos que tenha criado a análise, você aparecerá como o único usuário na análise. Selecione a marca de seleção ao lado de seu nome.
4. Escolha **Aprovar** ou **Negar**. Talvez seja necessário incluir um motivo para a sua decisão na caixa de texto **Fornecer um motivo**.
5. Feche a folha **Funções de análise do AD do Azure**.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Próximas etapas
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

<!---HONumber=AcomDC_0706_2016-->