<properties
	pageTitle="Otimizar seu ambiente com a solução de Avaliação de SQL da Log Analytics | Microsoft Azure"
	description="Você pode usar a solução de Avaliação de SQL para avaliar o risco e a integridade de seus ambientes de servidor em intervalos regulares."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/28/2016"
	ms.author="banders"/>

# Otimizar seu ambiente com a solução de Avaliação de SQL da Log Analytics


Você pode usar a solução de Avaliação de SQL para avaliar o risco e a integridade de seus ambientes de servidor em intervalos regulares. Este artigo ajudará você a instalar a solução para que você possa tomar ações corretivas para potenciais problemas.

Esta solução fornece uma lista priorizada de recomendações específicas para sua infraestrutura de servidor implantado. As recomendações são categorizadas em seis áreas de foco que ajudarão a entender o risco e tomar uma ação corretiva.

As recomendações baseiam-se no conhecimento e na experiências obtidas pelos engenheiros da Microsoft em milhares de visitas a clientes. Cada recomendação fornece diretrizes sobre por que um problema pode ser relevante para você e como implementar as alterações sugeridas.

Você pode escolher as áreas de foco que são mais importantes para sua organização e acompanhar seu progresso no sentido de executar um ambiente íntegro e sem riscos.

Após terminar de adicionar a solução e a avaliação ser concluída, as informações resumidas para áreas de foco são mostradas no painel **Avaliação de SQL** para a infraestrutura no seu ambiente. As seções a seguir descrevem como usar as informações no painel **Avaliação de SQL**, em que é possível exibir e executar as ações recomendadas para sua infraestrutura de SQL Server.

![imagem do bloco de Avaliação do SQL](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![imagem do painel de avaliação do SQL](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## Instalando e configurando a solução
A Avaliação de SQL trabalha com todas as versões do SQL Server com suporte atualmente para as edições Standard, Developer e Enterprise.

Use as informações a seguir para instalar e configurar a solução.

- A solução de Avaliação do SQL requer o .NET Framework 4 instalado em cada computador que tem um agente do OMS.
- Ao usar o agente do Operations Manager com a Avaliação do SQL, você precisará usar uma conta Executar como do Operations Manager. Consulte [Contas Executar como do Operations Manager para OMS](#operations-manager-run-as-accounts-for-oms) abaixo para obter mais informações.

    >[AZURE.NOTE] O agente MMA não dá suporte a contas Executar como do Operations Manager.

- Adicionar a solução da Avaliação do SQL ao seu espaço de trabalho do OMS usando o processo descrito em [Adicionar soluções do Log Analytics da Galeria de Soluções](log-analytics-add-solutions.md). Não é necessária nenhuma configuração.

>[AZURE.NOTE] Depois de adicionar a solução, o arquivo AdvisorAssessment.exe é adicionado aos servidores com agentes. Os dados de configuração são lidos e, em seguida, enviados para o serviço OMS na nuvem para processamento. A lógica é aplicada aos dados recebidos e o serviço de nuvem registra os dados.

## Detalhes de coleta de dados da Avaliação de SQL

A tabela a seguir mostra os métodos de coleta de dados dos agentes, se o SCOM (Operations Manager) é necessário e com que frequência os dados são coletados por um agente.

| plataforma | Agente direto | Agente SCOM | Armazenamento do Azure | SCOM necessário? | Os dados do agente SCOM enviados por meio do grupo de gerenciamento | frequência de coleta |
|---|---|---|---|---|---|---|
|Windows|![Sim](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Sim](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Não](./media/log-analytics-sql-assessment/oms-bullet-red.png)|	![Não](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![Sim](./media/log-analytics-sql-assessment/oms-bullet-green.png)|	7 dias|

## Contas Executar como do Operations Manager para OMS

O Log Analytics no OMS usa o grupo de gerenciamento e o agente do Operations Manager para coletar e enviar dados para o serviço do OMS. O OMS é criado com base nos pacotes de gerenciamento para cargas de trabalho para fornecer serviços com valor agregado. Cada carga de trabalho exige privilégios específicos da carga de trabalho para executar pacotes de gerenciamento em um contexto de segurança diferente, como uma conta de domínio. Você precisa fornecer informações de credenciais configurando uma conta Executar como do Operations Manager.

Use as informações a seguir para definir a conta Executar como do Operations Manager para Avaliação de SQL.

### Definir a conta Executar como para avaliação do SQL

 Se você já estiver usando o pacote de gerenciamento do SQL Server, deve usar essa conta Executar como.

#### Para configurar a conta Executar como do SQL no console Operações

1. No Operations Manager, abra o console Operações e clique em **Administração**.

2. Em **Configuração Executar como**, clique em **Perfis** e abra **Perfil Executar como da Avaliação SQL do OMS**.

3. Na página **Contas Executar como**, clique em **Adicionar**.

4. Selecione uma conta Executar como do Windows que contenha as credenciais necessárias para o SQL Server ou clique em **Nova** para criar uma.
	>[AZURE.NOTE] O tipo de conta Executar Como deve ser Windows. A conta Executar como também deve fazer parte do grupo de administradores locais em todos os servidores Windows que hospedam instâncias do SQL Server.

5. Clique em **Salvar**.

6. Modifique e, em seguida, execute o seguinte exemplo de T-SQL em cada instância do SQL Server para conceder as permissões mínimas necessárias para a conta Executar como para realizar uma avaliação de SQL. No entanto, você não precisa fazer isso se uma conta Executar como já fizer parte da função de servidor sysadmin em instâncias do SQL Server.

```
	---
	    -- Replace <UserName> with the actual user name being used as Run As Account.
	    USE master
	
	    -- Create login for the user, comment this line if login is already created.
	    CREATE LOGIN [<UserName>] FROM WINDOWS
	
	    -- Grant permissions to user.
	    GRANT VIEW SERVER STATE TO [<UserName>]
	    GRANT VIEW ANY DEFINITION TO [<UserName>]
	    GRANT VIEW ANY DATABASE TO [<UserName>]
	
	    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
	    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
	    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### Para configurar a conta Executar como do SQL usando o Windows PowerShell

Abra uma janela do PowerShell e execute o script a seguir depois de atualizá-lo com as suas informações:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## Compreendendo como as recomendações são priorizadas

Cada recomendação feita recebe um valor de ponderação que identifica a importância relativa da recomendação. Somente as dez recomendações mais importantes são mostradas.

### Como os pesos são calculados

Os pesos são valores agregados com base em três fatores principais:

- A *probabilidade* de que um problema identificado cause problemas. Uma probabilidade mais alta é igual a uma pontuação geral maior para a recomendação.

- O *impacto* da questão na sua organização se ela causar um problema. Um impacto maior é igual a uma pontuação geral maior para a recomendação.

- O *esforço* necessário para implementar a recomendação. Um esforço maior é igual a uma pontuação geral menor para a recomendação.

A importância de cada recomendação é expressa como um percentual da pontuação total disponível para todas as áreas de foco. Por exemplo, se uma recomendação na área de foco de segurança e conformidade tiver uma pontuação de 5%, implementar essa recomendação aumentará sua pontuação geral de segurança e conformidade em 5%.

### Áreas de foco

**Segurança e conformidade** - essa área de foco mostra recomendações para possíveis ameaças de segurança e violações, políticas corporativas, bem como os requisitos de conformidade técnica, legal e regulatória.

**Disponibilidade e continuidade dos negócios** - essa área de foco mostra as recomendações para a disponibilidade de serviço, resiliência de sua infraestrutura e proteção dos negócios.

**Desempenho e escalabilidade** - essa área de foco mostra recomendações para ajudar a expansão da infraestrutura de TI de sua organização, garantir que seu ambiente de TI atende aos requisitos de desempenho atuais e estar apta a responder às necessidades de infraestrutura em constante mudança.

**Atualização, implantação e migração** - essa área de foco mostra recomendações para ajudá-lo a atualizar, migrar e implantar o SQL Server em sua infraestrutura existente.

**Operações e monitoramento** - essa área de foco mostra recomendações que ajudam a simplificar as operações de TI, implementar manutenção preventiva e maximizar o desempenho.

**Gerenciamento de alterações e configuração** - essa área de foco mostra as recomendações para ajudar a proteger as operações diárias, garantir que as alterações não afetam sua infra-estrutura negativamente, estabelecer procedimentos de controle de alterações e para controlar e auditar as configurações do sistema.

### Você deve visar à pontuação de 100% em cada área de foco?

Não necessariamente. As recomendações baseiam-se no conhecimento e nas experiências adquiridas pelos engenheiros da Microsoft em milhares de visitas a clientes. No entanto, nenhuma infraestrutura de servidor é igual à outra, assim, recomendações específicas podem ser mais ou menos relevantes para você. Por exemplo, algumas recomendações de segurança poderão ser menos relevantes se as máquinas virtuais não estiverem expostas à Internet. Algumas recomendações de disponibilidade podem ser menos relevantes para os serviços que fornecem relatórios e coleta de dados ad hoc de baixa prioridade. Problemas importantes para empresas maduras podem ser menos importantes para uma empresa start-up. Você pode preferir identificar quais áreas de foco são suas prioridades e depois examinar como suas pontuações mudam ao longo do tempo.

Cada recomendação inclui diretrizes sobre sua importância. Você deve usar essas diretrizes para avaliar se é adequado implementar a recomendação considerando a natureza de seus serviços de TI e as necessidades comerciais da sua organização.

## Use as recomendações de área de foco de avaliação

Antes de usar uma solução de avaliação no OMS, é necessário ter a solução instalada. Para ler mais sobre como instalar as soluções, confira [Adicionar soluções do Log Analytics por meio da Galeria de Soluções](log-analytics-add-solutions.md). Após a instalação, é possível exibir o resumo das recomendações usando o bloco Avaliação do SQL na página Visão geral do OMS.

Veja as avaliações de conformidade resumidas para sua infraestrutura e faça uma busca detalhada das recomendações.

### Para exibir as recomendações para uma área de foco e tomar uma ação corretiva

1. Na página **Visão Geral**, clique no bloco **Avaliação do SQL**.
2. Na página **Avaliação do SQL**, analise as informações resumidas em uma das folhas da área de foco e clique em uma para exibir as recomendações para a área de foco.
3. Em qualquer uma das páginas da área de foco, você pode exibir as recomendações priorizadas para seu ambiente. Clique em uma recomendação sob **Objetos Afetados** para exibir detalhes sobre o motivo pelo qual a recomendação foi feita. 
![imagem de recomendações de avaliação do SQL](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. É possível executar as ações corretivas sugeridas em **Ações Sugeridas**. Quando o item tiver sido resolvido, avaliações posteriores gravarão que essas ações recomendadas foram executadas e sua pontuação de conformidade aumentará. Os itens corrigido aparecem como **Objetos Passados**.

## Ignorar as recomendações

Se houver recomendações que deseja ignorar, você poderá criar um arquivo de texto que será usado pelo OMS para impedir que as recomendações sejam exibidas nos resultados da avaliação.

### Para identificar as recomendações que serão ignoradas

1.	Entre em seu espaço de trabalho e abra a Pesquisa de Log. Use a consulta a seguir para listar as recomendações que falharam para os computadores em seu ambiente.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Esta é uma captura de tela mostrando a consulta da Pesquisa de Log:
     ![recomendações com falha](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.	Escolha as recomendações que você deseja ignorar. Você usará os valores para RecommendationId no próximo procedimento.


### Criar e usar um arquivo de texto IgnoreRecommendations.txt

1.	Crie um arquivo chamado IgnoreRecommendations.txt.
2.	Cole ou digite cada RecommendationId para cada recomendação que você deseja que o OMS ignore em uma linha separada, salve e feche o arquivo.
3.	Coloque o arquivo na pasta a seguir em cada computador no qual você deseja que o OMS ignore as recomendações.
    - Em computadores com o Microsoft Monitoring Agent (conectado diretamente ou por meio do Operations Manager) - *SystemDrive*: \\Arquivos de Programas\\Microsoft Monitoring Agent\\Agent
    - No servidor de gerenciamento do Operations Manager - *SystemDrive*: \\Arquivos de Programa\\Microsoft System Center 2012 R2\\Operations Manager\\Server

### Para verificar se as recomendações são ignoradas

1.	Após a execução da próxima avaliação agendada, por padrão a cada 7 dias, as recomendações especificadas são marcadas como Ignoradas e não aparecerão no painel de avaliação.
2.	É possível usar as consultas da Pesquisa de Log a seguir para listar todas as recomendações ignoradas.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.	Se você decidir posteriormente que deseja ver as recomendações ignoradas, remova todos os arquivos IgnoreRecommendations.txt ou remova as RecommendationIDs deles.

## Perguntas frequentes sobre soluções de Avaliação de SQL

*Com que frequência uma avaliação é executada?*
- A avaliação é executada a cada sete dias.

*Há uma maneira de configurar a frequência com que a avaliação é executada?*
- Não no momento.

*Se outro servidor for descoberto após ter adicionado uma solução de avaliação de SQL, ele será avaliado?*
- Sim, assim que for descoberto, ele é avaliado a partir de então a cada sete dias.

*Se um servidor for encerrado, quando ele será removido da avaliação?*
- Se um servidor não enviar dados por três semanas, ele será removido.

*Qual é o nome do processo que faz a coleta de dados?*
- AdvisorAssessment.exe

*Quanto tempo leva para os dados serem coletados?*
- A coleta de dados real no servidor leva aproximadamente 1 hora. Pode levar mais tempo em servidores que têm um grande número de instâncias ou bancos de dados SQL.

*Que tipo de dados é coletado?*
- Os seguintes tipos de dados são coletados:
    - WMI
    - Registro
    - Contadores de desempenho
    - Exibições de gerenciamento dinâmico SQL (DMV).

*Há uma maneira de configurar quando os dados são coletados?*
- Não no momento.

*Por que é necessário configurar uma conta Executar como?*
- Para o SQL Server, um pequeno número de consultas SQL é executado. Para elas serem executadas, uma conta Executar como com permissões VIEW SERVER STATE devem ser usadas para SQL. Além disso, para consultar WMI, são necessárias credenciais de administrador local.

*Por que apenas as 10 principais recomendações são exibidas?*
- Em vez de apresentar uma lista exaustiva de tarefas, é recomendável que você se concentre em tratar as recomendações priorizadas primeiro. Depois de tratar dessas recomendações, recomendações adicionais serão disponibilizadas. Se preferir ver a lista detalhada, exiba todas as recomendações usando a pesquisa de log do OMS.

*É possível ignorar uma recomendação?*
- Sim, confira a seção [Ignorar recomendações](#ignore-recommendations) acima.



## Próximas etapas

- [Pesquisar logs](log-analytics-log-searches.md) para exibir dados detalhados de Avaliação do SQL e recomendações.

<!---HONumber=AcomDC_0504_2016-->