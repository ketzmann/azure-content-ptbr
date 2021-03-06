<properties
	pageTitle="APIs do SDK para Web do Azure Mobile Engagement | Microsoft Azure"
	description="As atualizações e procedimentos mais recentes do SDK para Web do Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="web"
	ms.devlang="js"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="piyushjo" />

# Usar a API do Azure Mobile Engagement em um aplicativo Web

Este documento é um complemento do documento que descreve como [integrar o Mobile Engagement a um aplicativo Web](mobile-engagement-web-integrate-engagement.md). Ele fornece detalhes aprofundados sobre como usar a API do Azure Mobile Engagement para relatar as estatísticas do aplicativo.

A API do Mobile Engagement é fornecida pelo objeto `engagement.agent`. O alias padrão do SDK Web do Azure Mobile Engagement é `engagement`. Você pode redefinir esse alias na configuração do SDK.

## Conceitos do Mobile Engagement

As partes a seguir refinam os [Conceitos do Mobile Engagement](mobile-engagement-concepts.md) comuns para a plataforma da Web.

### `Session` e `Activity`

Se o usuário permanecer ocioso mais de alguns segundos entre duas atividades, a sua sequência de atividades é dividida em duas sessões diferentes. Esses poucos segundos são chamados de tempo limite da sessão.

Se o aplicativo Web não declarar o fim das atividades do usuário por si só (chamando a função `engagement.agent.endActivity`), o servidor do Mobile Engagement vai expirar automaticamente a sessão do usuário em três minutos depois que a página do aplicativo for fechada. Isso é chamado de tempo limite da sessão do servidor.

### `Crash`

Relatórios automatizados de exceções não identificadas de JavaScript não são criados por padrão. No entanto, você pode relatar falhas manualmente usando a função `sendCrash` (confira a seção sobre relatórios de falhas).

## Relatando atividades

Relatórios sobre a atividade de usuário incluem quando um usuário inicia uma nova atividade e quando o usuário encerra a atividade atual.

### O usuário inicia uma nova atividade

	engagement.agent.startActivity("MyUserActivity");

É necessário chamar `startActivity()` sempre que houver alterações de atividade do usuário. A primeira chamada para essa função inicia uma nova sessão de usuário.

### O usuário encerra a atividade atual

	engagement.agent.endActivity();

É necessário chamar `endActivity()` pelo menos uma vez quando o usuário conclui sua última atividade. Isso informa ao SDK Web do Mobile Engagement que o usuário está ocioso no momento e que a sessão do usuário precisa ser fechada quanto o tempo limite expirar. Se você chamar `startActivity()` antes de expirar o tempo limite da sessão, a sessão será simplesmente retomada.

Como não existe uma chamada confiável quando a janela do navegador é fechada, muitas vezes isso dificulta ou impossibilita capturar o fim das atividades do usuário em ambientes da Web. Por esse motivo, o servidor do Mobile Engagement expira automaticamente a sessão do usuário em três minutos depois que a página do aplicativo é fechada.

## Relatando eventos

Relatórios sobre eventos abordam eventos de sessão e eventos autônomos.

### Eventos de sessão

Eventos de sessão são geralmente usados para relatar as ações executadas por um usuário durante a sessão.

**Exemplo sem dados adicionais:**

	loginButton.onclick = function() {
	  engagement.agent.sendSessionEvent('login');
	  // [...]
	}

**Exemplo com dados adicionais:**

	loginButton.onclick = function() {
	  engagement.agent.sendSessionEvent('login', {user: 'alice'});
	  // [...]
	}

### Eventos autônomos

Ao contrário dos eventos de sessão, os eventos independentes podem ocorrer fora do contexto de uma sessão.

Para isso, use ``engagement.agent.sendEvent`` em vez de ``engagement.agent.sendSessionEvent``.

## Relatando erros

Relatórios de erros abordam erros de sessão e autônomos.

### Erros de sessão

Erros de sessão geralmente são usados para relatar os erros que têm um impacto no usuário durante sua sessão.

**Exemplo sem dados adicionais:**

	var validateForm = function() {
	  // [...]
	  if (password.length < 6) {
	    engagement.agent.sendSessionError('password_too_short');
	  }
	  // [...]
	}

**Exemplo com dados adicionais:**

	var validateForm = function() {
	  // [...]
	  if (password.length < 6) {
	    engagement.agent.sendSessionError('password_too_short', {length: 4});
	  }
	  // [...]
	}

### Erros autônomos

Ao contrário dos erros de sessão, os erros autônomos podem ocorrer fora do contexto de uma sessão.

Para isso, use `engagement.agent.sendError` em vez de `engagement.agent.sendSessionError`.

## Relatando trabalhos

Relatórios sobre trabalhos abrangem erros e eventos que ocorrem durante um trabalho de emissão de relatórios e relatórios de falhas.

**Exemplo:**

Se você quer monitorar uma solicitação AJAX, use o seguinte:

	// [...]
	xhr.onreadystatechange = function() {
	  if (xhr.readyState == 4) {
	  // [...]
	    engagement.agent.endJob('publish');
	  }
	}
	engagement.agent.startJob('publish');
	xhr.send();
	// [...]

### Relatando erros durante um trabalho

Os erros podem estar relacionados a um trabalho em execução, em vez de a uma sessão do usuário atual.

**Exemplo:**

Suponha que você queira relatar um erro se uma solicitação AJAX falhar:

	// [...]
	xhr.onreadystatechange = function() {
	  if (xhr.readyState == 4) {
	    // [...]
	    if (xhr.status == 0 || xhr.status >= 400) {
	      engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
	    }
	    engagement.agent.endJob('publish');
	  }
	}
	engagement.agent.startJob('publish');
	xhr.send();
	// [...]

### Relatar eventos durante um trabalho

Os erros podem estar relacionados a um trabalho em execução, em vez de à sessão do usuário atual graças à função `engagement.agent.sendJobEvent`.

Essa função funciona exatamente como `engagement.agent.sendJobError`.

### Relatando falhas

Use a função `sendCrash` para relatar falhas manualmente.

O `crashid` é uma cadeia de caracteres que identifica o tipo de falha. O `crash` é geralmente o rastreamento de pilha da falha como uma cadeia de caracteres.

	engagement.agent.sendCrash(crashid, crash);

## Parâmetros adicionais

Você pode anexar dados arbitrários a evento, erro, atividade ou trabalho.

Esses dados podem ser qualquer objeto JSON (mas não uma matriz ou tipos primitivos).

**Exemplo:**

	var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
	engagement.agent.sendEvent("video_clicked", extras);

### Limites

Limites que se aplicam a parâmetros extras estão nas áreas de expressões regulares de chaves, tipos de valor e tamanho.

#### simétricas

Cada chave no objeto deve corresponder à seguinte expressão regular:

	^[a-zA-Z][a-zA-Z_0-9]*

Isso significa que as chaves devem começar com pelo menos uma letra, seguida por letras, dígitos ou sublinhados (\_).

#### Valores

Os valores são limitados aos tipos de booliano, número e cadeia de caracteres.

#### Tamanho

Os extras são limitados a 1024 caracteres por chamada (depois que o SDK Web do Mobile Engagement codifica em JSON).

## Relatando informações de aplicativo

Você pode relatar manualmente informações (ou quaisquer outras informações específicas do aplicativo) de controle usando a função `sendAppInfo()`.

Observe que essas informações podem ser enviadas de forma incremental. Somente o último valor para uma chave específica será mantido para cada dispositivo.

Como extras de evento, você pode usar qualquer objeto JSON para abstrair as informações do aplicativo. Observe que matrizes ou subobjetos são tratados como cadeias de caracteres simples (usando a serialização JSON).

**Exemplo:**

Aqui está um exemplo de código para enviar a data de nascimento e o sexo do usuário:

	var appInfos = {"birthdate":"1983-12-07","gender":"female"};
	engagement.agent.sendAppInfo(appInfos);

### Limites

Limites que se aplicam às informações do aplicativo são nas áreas de expressões regulares de chaves e tamanho.

#### simétricas

Cada chave no objeto deve corresponder à seguinte expressão regular:

	^[a-zA-Z][a-zA-Z_0-9]*

Isso significa que as chaves devem começar com pelo menos uma letra, seguida por letras, dígitos ou sublinhados (\_).

#### Tamanho

Informações do aplicativo são limitadas a 1024 caracteres por chamada (depois que o SDK Web do Mobile Engagement codifica em JSON).

No exemplo anterior, o JSON enviado para o servidor tem 44 caracteres:

	{"birthdate":"1983-12-07","gender":"female"}

<!---HONumber=AcomDC_0713_2016-->