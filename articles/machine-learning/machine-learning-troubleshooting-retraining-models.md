<properties
	pageTitle="Solução de Problemas de Readaptação de um serviço Web do Machine Learning | Microsoft Azure"
	description="Identifique e corrija os problemas comuns encontrados quando você está readaptando o modelo para um serviço Web do Aprendizado de Máquina do Azure."
	services="machine-learning"
	documentationCenter=""
	authors="VDonGlover"
	manager=""
	editor=""/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/05/2016"
	ms.author="v-donglo"/>

#Solução de problemas de readaptação de um serviço da Web clássico do Aprendizado de Máquina do Azure

## Visão geral da readaptação

Quando você implanta um experimento de previsão como um serviço Web de pontuação, ele é um modelo estático. Conforme novos dados ficam disponíveis ou quando o consumidor da API tem seus próprios dados, o modelo precisa ser readaptado.

Para obter uma explicação completa do processo de readaptação de um serviço Web clássico, consulte [Readaptar os Modelos de Aprendizado de Máquina de Forma Programática](machine-learning-retrain-models-programmatically.md).

## Processo de readaptação

Quando você precisa readaptar o serviço Web, deve adicionar algumas partes extras:

* Um serviço Web implantado a partir do teste de treinamento. O teste deve ter um módulo de **saída do serviço Web** anexado à saída do módulo do **Modelo de Treinamento**.

	![Anexe a saída do serviço Web ao modelo de treinamento.][image1]

* Um novo ponto de extremidade adicionado ao seu serviço Web de pontuação. Você pode adicionar o ponto de extremidade por meio da programação usando o código de exemplo referenciado no tópico Readaptar os modelos de Aprendizado de Máquina de forma programática ou por meio do portal clássico do Azure.

Em seguida, pode usar o código C# de exemplo na página de ajuda da API do Serviço Web de Treinamento para readaptar o modelo. Depois de ter avaliado os resultados e ficar satisfeito com eles, você atualizará o serviço Web de pontuação do modelo treinado usando o novo ponto de extremidade adicionado.

Com todas as peças no lugar, as principais etapas necessárias para readaptar o modelo são as seguintes:

1.	Chame o Serviço Web de Treinamento: a chamada é para o BES (Serviço de Execução em Lote), não o RRS (Serviço de Resposta a Solicitação). Você pode usar o Código c# de exemplo na página de ajuda da API para fazer a chamada.
2.	Encontre os valores para *BaseLocation*, *RelativeLocation* e *SasBlobToken*: esses valores são retornados na saída de sua chamada para o Serviço Web de Treinamento. ![exibindo a saída do exemplo de readaptação e os valores BaseLocation, RelativeLocation e SasBlobToken.][image6]
3.	Atualize o ponto de extremidade adicionado do serviço Web de pontuação com o novo modelo treinado: usando o código de exemplo fornecido em Readaptar os modelos de Aprendizado de Máquina de forma programática, atualize o novo ponto de extremidade adicionado ao modelo de pontuação com o modelo treinado recentemente do Serviço Web de Treinamento.

## Obstáculos comuns
### Verifique se você tem a URL correta do PATCH

A URL do PATCH que você está usando deve ser a associada ao novo ponto de extremidade de pontuação adicionado ao serviço Web de pontuação. Há duas maneiras de obter a URL do PATCH:

Opção 1: usando o C#

Para obter a URL correta do PATCH:

1.	Execute o código de exemplo [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).
2.	Na saída do AddEndpoint, encontre o valor *HelpLocation* e copie a URL.

	![HelpLocation na saída do exemplo addEndpoint.][image2]

3.	Cole a URL em um navegador para navegar até uma página que fornece links de ajuda para o serviço Web.
4.	Clique no link **Atualizar Recurso** para abrir a página de ajuda do patch.

Opção 2: usando o Portal do Azure

1.	Faça logon no [Portal do Azure](https://manage.windowsazure.com).
2.	Abra a guia Aprendizado de Máquina. ![Guia Aprendizado de máquina.][image4]
3.	Clique no nome do espaço de trabalho, em seguida, em **Serviços Web**.
4.	Clique no serviço Web de pontuação com o qual você está trabalhando. (Se você não modificou o nome padrão do serviço Web, ele normalmente terminará com [Scoring Exp.].)
5.	Clicar em Adicionar Ponto de Extremidade
6.	Depois do ponto de extremidade ser adicionado, clique no nome dele. Em seguida, clique em **Atualizar Recurso** para abrir a página de ajuda do patch.

![Painel Novo ponto de extremidade.][image3]

A página de ajuda do PATCH contém a URL do PATCH que você deve usar e fornece o código de exemplo que você pode usar para chamar.

![URL de patch.][image5]


### Verifique se você está atualizando o ponto de extremidade correto de pontuação
* Não corrija o serviço Web de treinamento: a operação do patch deve ser executada no serviço Web de pontuação.
* Não corrija o ponto de extremidade padrão no serviço Web: a operação do patch deve ser executada no novo ponto de extremidade do serviço Web de pontuação que você adicionou.

Verifique se você adicionou o ponto de extremidade ao serviço Web correto

O ponto de extremidade usado para readaptar o modelo deve estar no serviço Web de pontuação, não no serviço Web de treinamento. Você pode verificar em qual serviço Web o ponto de extremidade está visitando o portal clássico do Azure.

1.	Faça logon no [Portal do Azure](https://manage.windowsazure.com).
2.	Abra a guia Aprendizado de Máquina. ![IU do espaço de Trabalho do Machine Learning.][image4]
3.	Selecione o espaço de trabalho.
4.	Clique em **Serviços Web**.
5.	Selecione o serviço Web de previsão.
6.	Verifique se o novo ponto de extremidade foi adicionado ao serviço Web.

### Verifique o espaço de trabalho no qual o serviço Web está para garantir que esteja na região correta

1.	Faça logon no [Portal do Azure](https://manage.windowsazure.com).
2.	Selecione Aprendizado de Máquina no menu. ![IU da região do aprendizado de máquina.][image4]
3.	Verifique o local do seu espaço de trabalho.


<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png

<!---HONumber=AcomDC_0810_2016-->