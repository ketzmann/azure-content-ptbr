<properties
   pageTitle="Uso de Azure Functions com Aplicativos Lógicos | Microsoft Azure"
   description="Veja como usar Azure Functions com Aplicativos Lógicos"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>

# Usando Azure Functions com Aplicativos Lógicos

Você pode executar trechos personalizados do C# ou do node.js utilizando as Azure Functions de um aplicativo lógico. As [Azure Functions](../azure-functions/functions-overview.md) oferecem uma computação sem servidor no Microsoft Azure. Isso é útil para executar as seguintes tarefas:

* Formatar o valor de uma ação (por exemplo, converter de DateTime em uma cadeia de caracteres de data)
* Executar cálculos em um fluxo de trabalho
* Estender a funcionalidade dos Aplicativos Lógicos com funções com suporte no C# ou no node.js

## Criar uma função para os Aplicativos Lógicos

É recomendável que você crie uma nova função no portal das Azure Functions usando os modelos **Webhook Genérico do Nó** ou **Webhook Genérico do C# **. Isso preenche automaticamente um modelo que aceita `application/json` de um aplicativo lógico. As funções que usam esses modelos são automaticamente detectadas e listadas no designer dos Aplicativos Lógicos em **Azure Functions em minha região.**

As funções do Webhook aceitam uma solicitação e passam-na para o método por meio de uma variável `data`. Você pode acessar as propriedades do conteúdo usando uma notação de ponto como `data.foo`. Por exemplo, uma função Javascript simples, que converte um valor DateTime em uma cadeia de caracteres de data, terá a seguinte aparência:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## Chamar Azure Functions a partir de um aplicativo lógico

No designer, se você clicar no menu **Ações**, poderá selecionar **Azure Functions em minha Região**. Isso lista os contêineres em sua assinatura e permitirá que você escolha a função que deseja chamar.

Depois de escolher a função, você será solicitado a especificar um objeto de conteúdo de entrada. Esta é a mensagem que o aplicativo lógico envia para a função e deve ser um objeto JSON. Por exemplo, se você quiser passar a data da **Última Modificação** do gatilho Salesforce, o conteúdo da função poderá ser assim:

![Data da última modificação][1]

## Disparar aplicativos lógicos a partir de uma função

Também é possível disparar um aplicativo lógico de dentro de uma função. Para fazer isso, basta criar um aplicativo lógico com um gatilho manual. Para obter mais informações, consulte [Aplicativos lógicos como pontos de extremidade chamados](app-service-logic-http-endpoint.md). Em seguida, em sua função, gere um POST HTTP para a URL do gatilho manual com o conteúdo que você deseja enviar para o aplicativo lógico.

### Criar uma função no designer

Você também pode criar uma função de webhook do node.js de dentro do designer. Primeiro, selecione **Azure Functions em minha região**, em seguida, escolha um contêiner de sua função. Se você ainda não tiver um contêiner, precisará criar um a partir do [portal das Azure Functions](https://functions.azure.com/signin). Em seguida, selecione **Criar Novo**.

Para gerar um modelo com base nos dados que você deseja calcular, especifique o objeto de contexto que planeja passar para uma função. Ele deve ser um objeto JSON. Por exemplo, se você passar o conteúdo do arquivo a partir de uma ação FTP, o conteúdo do contexto ficará assim:

![Conteúdo do contexto][2]

>[AZURE.NOTE] Como esse objeto não foi convertido em uma cadeia de caracteres, o conteúdo será adicionado diretamente ao conteúdo JSON. No entanto, ele apresentará um erro se não for um token JSON (ou seja, uma cadeia de caracteres ou um objeto/matriz JSON). Para convertê-lo em uma cadeia de caracteres, basta adicionar aspas conforme mostrado na primeira ilustração neste artigo.

Então, o designer irá gerar um modelo de função que você poderá criar embutido. As variáveis são previamente criadas com base no contexto que você planeja passar para a função.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png

<!---HONumber=AcomDC_0803_2016-->