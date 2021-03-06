<properties
	pageTitle="Estatísticas em Tempo Real na CDN do Azure | Microsoft Azure"
	description="As Estatísticas em Tempo Real fornecem dados em tempo real sobre o desempenho da CDN do Azure ao fornecer conteúdo para os clientes."
	services="cdn"
	documentationCenter=""
	authors="camsoper"
	manager="erikre"
	editor=""/>

<tags
	ms.service="cdn"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/28/2016"
	ms.author="casoper"/>

# Estatísticas em tempo real na CDN do Microsoft Azure

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## Visão geral

Este documento explica as estatísticas em tempo real na CDN do Microsoft Azure. Essa funcionalidade fornece dados em tempo real, como a largura de banda, status do cache e conexões simultâneas, para seu perfil da CDN ao fornecer conteúdo para os clientes. Isso permite o monitoramento contínuo da integridade do serviço a qualquer momento, incluindo eventos de ativação.

Os gráficos a seguir estão disponíveis:

* [Largura de banda](#bandwidth)
* [Códigos de status](#status-codes)
* [Status do Cache](#cache-statuses)
* [Conexões](#connections)


## Acessando as estatísticas em tempo real

1. No [Portal do Azure](https://portal.azure.com), navegue para seu perfil CDN.

	![Folha Perfil CDN](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Na folha do perfil CDN, clique no botão **Gerenciar**.

	![botão gerenciar da folha Perfil CDN](./media/cdn-real-time-stats/cdn-manage-btn.png)

	O portal de gerenciamento da CDN é aberto.

3. Focalize a guia **Análise** e, em seguida, focalize o submenu **Estatísticas em Tempo Real**. Clique em **Objeto grande de HTTP**.

	![Portal de gerenciamento da CDN](./media/cdn-real-time-stats/cdn-premium-portal.png)

	Os gráficos de estatísticas em tempo real são exibidos.
	
Cada um dos gráficos exibe estatísticas em tempo real para o período de tempo selecionado, iniciando quando a página carrega. Os gráficos são atualizados automaticamente em alguns segundos. O botão **Atualizar Gráfico**, se presente, limpará o gráfico, depois exibirá apenas os dados selecionados.

## Largura de banda

![Gráfico da largura de banda](./media/cdn-real-time-stats/cdn-bandwidth.png)

O gráfico **Largura de Banda** exibe a quantidade de largura de banda usada para a plataforma atual durante um período de tempo selecionado. A parte sombreada do gráfico indica o uso de largura de banda. A quantidade exata de largura de banda usada atualmente é exibida diretamente abaixo do gráfico de linha.

## Códigos de status

![Gráfico do código de status](./media/cdn-real-time-stats/cdn-status-codes.png)

O gráfico **Códigos de Status** indica com que frequência determinados códigos de resposta HTTP estão ocorrendo no período de tempo selecionado.

> [AZURE.TIP]  Para obter uma descrição de cada opção do código de status HTTP, confira [Códigos de Status HTTP da CDN do Azure](https://msdn.microsoft.com/library/mt759238.aspx).

É exibida uma lista dos códigos de status HTTP diretamente acima do gráfico. Essa lista indica cada código de status que pode ser incluído no gráfico de linha e o número de ocorrências por segundo para esse código de status. Por padrão, uma linha é exibida para cada um destes códigos de status no gráfico. No entanto, você pode optar por monitorar apenas os códigos de status que têm um significado especial para a sua configuração de CDN. Para fazer isso, verifique os códigos de status desejados, desmarque todas as outras opções e clique em **Atualizar Gráfico**.

Você pode ocultar temporariamente os dados registrados para um código de status específico. Na legenda diretamente abaixo do gráfico, clique no código de status que você deseja ocultar. O código de status será imediatamente ocultado no gráfico. Clicar novamente no código de status fará com que essa opção seja exibida mais uma vez.

## Status do Cache

![Gráficos dos Status do Cache](./media/cdn-real-time-stats/cdn-cache-status.png)

O gráfico **Status do Cache** indica com que frequência determinados tipos de status do cache estão ocorrendo no período de tempo selecionado.

> [AZURE.TIP]  Para obter uma descrição de cada opção do código de status do cache, consulte [Códigos de Status do Cache da CDN do Azure](https://msdn.microsoft.com/library/mt759237.aspx).

É exibida uma lista dos códigos de status do cache diretamente acima do gráfico. Essa lista indica cada código de status que pode ser incluído no gráfico de linha e o número de ocorrências por segundo para esse código de status. Por padrão, uma linha é exibida para cada um destes códigos de status no gráfico. No entanto, você pode optar por monitorar apenas os códigos de status que têm um significado especial para a sua configuração de CDN. Para fazer isso, verifique os códigos de status desejados, desmarque todas as outras opções e clique em **Atualizar Gráfico**.

Você pode ocultar temporariamente os dados registrados para um código de status específico. Na legenda diretamente abaixo do gráfico, clique no código de status que você deseja ocultar. O código de status será imediatamente ocultado no gráfico. Clicar novamente no código de status fará com que essa opção seja exibida mais uma vez.

## Conexões

![Gráfico das Conexões](./media/cdn-real-time-stats/cdn-connections.png)

Esse gráfico indica quantas conexões foram estabelecidas para seus servidores de borda. Cada solicitação de um ativo que passa por nossos resultados da CDN em uma conexão.

## Próximas etapas

- Saiba mais com os [relatórios HTTP avançados](cdn-advanced-http-reports.md)
- Analisar os [padrões de uso](cdn-analyze-usage-patterns.md)

<!---HONumber=AcomDC_0803_2016-->