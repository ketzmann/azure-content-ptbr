<properties
	pageTitle="Protocolo SAML de Logout Único do Azure | Microsoft Azure"
	description="Este artigo descreve o protocolo SAML de Logout Único no Azure Active Directory"
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/23/2016"
	ms.author="priyamo"/>


# Protocolo SAML de Logout Único

O Azure AD (Azure Active Directory) dá suporte ao perfil de logout único de navegador Web SAML 2.0. Para que o logout único funcione corretamente, o Azure AD deve registrar sua URL de metadados durante o registro do aplicativo. O Azure AD obtém a URL de logout e a chave de assinatura do serviço de nuvem dos metadados. O Azure AD usa a chave de assinatura para verificar a assinatura do LogoutRequest de entrada e usa a LogoutURL para redirecionar os usuários depois que eles são desconectados.

Se o serviço de nuvem não dá suporte a um ponto de extremidade de metadados, depois que o aplicativo é registrado, o desenvolvedor deve contatar o suporte da Microsoft para fornecer a URL de logout e a chave de assinatura.

Este diagrama mostra o fluxo de trabalho do processo de logout único do Azure AD.

![Fluxo de trabalho do Logout Único](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest

O serviço de nuvem envia uma mensagem `LogoutRequest` ao Azure AD para indicar que uma sessão foi encerrada. O trecho a seguir mostra um exemplo de elemento `LogoutRequest`.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest

O elemento `LogoutRequest` enviado ao Azure AD requer os seguintes atributos:

- `ID` : isso identifica a solicitação de saída. O valor de `ID` não deve começar com um número. A prática comum é acrescentar **id** à representação de cadeia de caracteres de um GUID.

- `Version` : Defina o valor desse elemento como **2.0**. Esse valor é obrigatório.

- `IssueInstant` : Esta é uma cadeia de caracteres `DateTime` com um valor de UTC (Tempo Universal Coordenado) e [formato de ida e volta ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). O Azure AD espera um valor desse tipo, mas não é obrigatório.

- Os atributos `Consent`, `Destination`, `NotOnOrAfter` e `Reason` serão ignorados se forem incluídos em um elemento `LogoutRequest`.

### Emissor

O elemento `Issuer` em uma `LogoutRequest` deve corresponder exatamente a um do **ServicePrincipalNames** no serviço de nuvem no Azure AD. Normalmente, isso é definido como o **URI da ID do aplicativo** que é especificado durante o registro do aplicativo.

### NameID

O valor do elemento `NameID` deve corresponder exatamente ao `NameID` do usuário que está sendo desconectado.
## LogoutResponse

O Azure AD envia uma `LogoutResponse` em resposta a um elemento `LogoutRequest`. O trecho a seguir mostra um exemplo de `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse

O Azure AD define os valores `ID`, `Version` e `IssueInstant` no elemento `LogoutResponse`. Ele também define o elemento `InResponseTo` como o valor do atributo `ID` da `LogoutRequest` que emitiu a resposta.

### Emissor

O Azure AD define esse valor como `https://login.microsoftonline.com/<TenantIdGUID>/`, em que <TenantIdGUID> é a ID de locatário do locatário do Azure AD.

Para avaliar o valor do elemento `Issuer`, use o valor do **URI da ID do aplicativo** fornecido durante o registro do aplicativo.

### Status

O Azure AD usa o elemento `StatusCode` no elemento `Status` para indicar o sucesso ou a falha da desconexão. Quando a tentativa de desconexão falha, o elemento `StatusCode` também pode conter mensagens de erro personalizadas.

<!---HONumber=AcomDC_0629_2016-->