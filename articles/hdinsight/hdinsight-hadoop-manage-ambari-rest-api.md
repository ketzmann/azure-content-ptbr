<properties
   pageTitle="Monitorar e gerenciar clusters HDInsight usando a API REST do Apache Ambari | Microsoft Azure"
   description="Aprenda a usar o Ambari para monitorar e gerenciar clusters HDInsight baseados em Linux. Neste documento, você aprenderá a usar a API REST do Ambari incluída com clusters HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
	tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/05/2016"
   ms.author="larryfr"/>

#Gerenciar clusters HDInsight usando a API REST do Ambari

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

O Apache Ambari simplifica o gerenciamento e monitoramento de um cluster Hadoop, fornecendo uma forma fácil de usar a interface do usuário da Web e a API REST. O Ambari está incluído em clusters HDInsight baseados em Linux e é usado para monitorar o cluster e fazer alterações de configuração. Neste documento, você aprenderá os fundamentos de como trabalhar com a API REST do Ambari realizando tarefas comuns, como encontrar o nome de domínio totalmente qualificado dos nós do cluster ou localizar a conta de armazenamento padrão usada pelo cluster.

> [AZURE.NOTE] As informações deste artigo só se aplicam a clusters HDInsight baseados em Linux. Para os clusters HDInsight baseados no Windows, apenas um subconjunto da funcionalidade de monitoramento está disponível pela API REST do Ambari. Consulte [Monitorar Hadoop baseado em Windows no HDInsight usando a API do Ambari](hdinsight-monitor-use-ambari-api.md).

##Pré-requisitos

* [cURL](http://curl.haxx.se/): o cURL é um utilitário de plataforma cruzada que pode ser usado para trabalhar com APIs REST na linha de comando. Neste documento, ele é usado para se comunicar com a API REST do Ambari.
* [jq](https://stedolan.github.io/jq/): o jq é um utilitário de linha de comando de plataforma cruzada para trabalhar com documentos JSON. Neste documento, ele é usado para analisar os documentos JSON retornados da API REST do Ambari.
* [CLI do Azure](../xplat-cli-install.md): um utilitário de linha de comando de plataforma cruzada usado para trabalhar com serviços do Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a id="whatis"></a>O que é o Ambari?

O [Apache Ambari](http://ambari.apache.org) simplifica o gerenciamento do Hadoop, fornecendo uma interface do usuário da Web fácil de usar que pode ser utilizada para provisionar, gerenciar e monitorar clusters Hadoop. Os desenvolvedores podem integrar essas funcionalidades em seus aplicativos usando as [APIs REST do Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari é fornecido por padrão com os clusters HDInsight baseados em Linux.

##API REST

O URI de base para a API REST do Ambari no HDInsight é https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, no qual __NOMEDOCLUSTER__ é o nome do cluster.

> [AZURE.IMPORTANT] Embora o nome do cluster na parte do nome de domínio totalmente qualificado (FQDN) do URI (CLUSTERNAME.azurehdinsight.net) diferencie maiúsculas de minúsculas, outras ocorrências no URI diferenciam maiúsculas de minúsculas. Por exemplo, se seu cluster se chamar MyCluster, os seguintes URIs serão válidos:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster` `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Os URIs a seguir retornarão um erro porque a segunda ocorrência do nome não apresenta o uso correto de maiúsculas e minúsculas.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster` `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

A conexão com o Ambari no HDInsight exige HTTPS. Você também deve se autenticar no Ambari usando o nome da conta de administrador (o padrão é __admin__) e a senha fornecidos durante a criação do cluster.

Veja a seguir um exemplo do uso de cURL para fazer uma solicitação GET na API REST:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Se você executar esse exemplo substituindo __SENHA__ pela senha de administrador do cluster e __NOMEDOCLUSTER__ pelo nome do cluster, você receberá um documento JSON que começa com informações semelhantes às seguintes:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Como se trata de JSON, normalmente é mais fácil usar um analisador JSON para recuperar dados. Por exemplo, se quiser recuperar informações de status de integridade do cluster, você pode usar o seguinte.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'
    
Isso recupera o documento JSON e redireciona a saída para jq. `.Clusters.health_report` indica o elemento dentro do documento JSON que você deseja recuperar.

##Exemplo: obter o FQDN de nós do cluster

Ao trabalhar com o HDInsight, você precisará saber o nome de domínio totalmente qualificado (FQDN) de um nó do cluster. Você pode recuperar facilmente o FQDN para vários nós no cluster usando o seguinte:

* __Nós de cabeçalho__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Nós de trabalho__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Nós do Zookeeper__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Observe que cada um deles segue o mesmo padrão de consulta de um componente o qual sabemos que é executado nesses nós, recuperando assim os elementos `host_name` que contêm o FQDN desses nós.

O elemento `host_components` do documento de retorno contém vários itens. O uso do `.host_components[]` e, em seguida, a especificação de um caminho dentro do elemento causará um loop por cada item e extrairá o valor do caminho específico. Caso você queira apenas um valor, como a primeira entrada de FQDN, você poderá retornar os itens como uma coleção e selecionar uma entrada específica:

    jq '[.host_components[].HostRoles.host_name][0]'

Isso retornará o primeiro FQDN da coleção.

##Exemplo: obter a conta de armazenamento padrão e um contêiner

Quando você criar um cluster HDInsight, será necessário usar uma conta do Armazenamento do Azure e um contêiner de blob como o armazenamento padrão para o cluster. Você pode usar o Ambari para recuperar essas informações após a criação do cluster. Por exemplo, se você quiser gravar programaticamente os dados direto no contêiner.

O exemplo a seguir recuperará o URI WASB do armazenamento padrão de clusters:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Isso retornará a primeira configuração aplicada ao servidor (`service_config_version=1`) que conterá essas informações. Se você estiver recuperando um valor que foi modificado após a criação do cluster, talvez seja necessário listar as versões de configuração e recuperar a mais recente.

Isso retornará um valor semelhante ao seguinte, no qual __CONTÊINER__ é o contêiner padrão e __NOMEDACONTA__ é o nome da conta de Armazenamento do Azure:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Você pode usar essas informações com a [CLI do Azure](../xplat-cli-install.md) para carregar ou baixar dados do contêiner.

1. Obter o grupo de recursos para a conta de armazenamento. Substitua __NOMEDACONTA__ pelo nome da conta de armazenamento recuperada do Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Isso retornará o nome do grupo de recursos para a conta.
    
    > [AZURE.NOTE] Se nada for retornado por este comando, talvez você precise alterar a CLI do Azure para o modo do Gerenciador de Recursos do Azure e executar o comando novamente. Para alternar para o modo do Gerenciador de Recursos do Azure, use o seguinte comando.
    >
    > `azure config mode arm`
    
2. Obtenha a chave da conta de armazenamento. Substitua __NOMEDOGRUPO__ pelo Grupo de recursos criado na seção anterior: Substitua __NOMEDACONTA__ pelo nome da conta de armazenamento:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    Isso retornará a chave primária da conta.
    
3. Use o comando Carregar para armazenar um arquivo no contêiner:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Substitua __NOMEDACONTA__ pelo nome da conta de armazenamento. Substitua __CHAVEDACONTA__ pela chave recuperada anteriormente. __CAMINHODOARQUIVO__ é o caminho para o arquivo que você deseja carregar, enquanto __CAMINHODOBLOB__ é o caminho no contêiner.

    Por exemplo, se você quiser exibir o arquivo no HDInsight em wasbs://example/data/filename.txt, __CAMINHODOBLOB__ deve ser `example/data/filename.txt`.

##Exemplo: atualizar a configuração do Ambari

1. Obtenha a configuração atual, que o Ambari armazena como a "configuração desejada":

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    Isso retornará um documento JSON contendo a configuração atual (identificada pelo valor _tag_) para os componentes instalados no cluster. Por exemplo, veja a seguir um trecho dos dados retornados de um tipo de cluster Spark.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Nesta lista, você precisa copiar o nome do componente (por exemplo, __spark\_thrift\_sparkconf__ e o valor __tag__.
    
2. Recupere a configuração do componente e da marca usando o comando a seguir. Substitua __spark-thrift-sparkconf__ e __INITIAL__ pelo componente e pela marca cuja configuração você deseja recuperar.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Curl recupera o documento JSON e, depois, jq é usado para fazer algumas modificações para criar um modelo que podemos usar para adicionar/modificar valores de configuração. Especificamente, ele faz o seguinte:
    
    * Cria um valor exclusivo que contém a cadeia de caracteres “version” e a data, que é armazenada em __newtag__
    * Cria um documento raiz para a nova configuração desejada
    * Obtém o conteúdo da matriz .items e o adiciona no elemento __desired\_config__.
    * Exclui os elementos __href__, __version__ e __Config__, pois eles não são necessários para enviar uma nova configuração
    * Adiciona um novo elemento __tag__ e define seu valor como __version#################__, no qual a parte numérica é baseada na data atual. Cada configuração deve ter uma marca exclusiva.
    
    Por fim, os dados são salvos no documento __newconfig.json__. A estrutura do documento será semelhante a esta:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Abra o documento __newconfig.json__ e modifique/adicione valores no objeto __properties__. Por exemplo, altere o valor de __"spark.yarn.am.memory"__ de __"1g"__ para __"3g"__ e adicione um novo elemento para __"spark.kryoserializer.buffer.max"__ com um valor de __"256m"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Quando terminar de fazer modificações, salve o arquivo.

4. Use o seguinte para enviar a configuração atualizada para o Ambari:

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Esse comando redireciona o conteúdo do arquivo __newconfig.json__ para a solicitação curl, que o envia para o cluster como a nova configuração desejada. Isso retornará um documento JSON. O elemento __versionTag__ nesse documento deverá corresponder à versão enviada, e o objeto __configs__ conterá as alterações de configuração solicitadas.

###Exemplo: reiniciar um componente de serviço

Neste ponto, se você examinar a interface do usuário da Web do Ambari, o serviço do Spark indicará que ele precisa ser reiniciado para que a nova configuração entre em vigor. Use as etapas a seguir para reiniciar o serviço.

1. Use o seguinte para habilitar o modo de manutenção para o serviço do Spark:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Isso envia um documento JSON para o servidor (contido na instrução `echo`) que ativa o modo de manutenção. Você pode verificar se o serviço está em modo de manutenção usando a seguinte solicitação:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Isso retornará um valor de `"ON"`.

3. Em seguida, use o seguinte para desativar o serviço:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Você receberá uma resposta semelhante a esta:
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    O valor `href` retornado por esse URI usa o endereço IP interno do nó de cluster. Para usá-lo de fora do cluster, substitua a parte '10.0.0.18:8080' pelo FQDN do cluster. Por exemplo, isto recuperará o status da solicitação:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Se esse valor retornar `"COMPLETED"`, isso significará que a solicitação foi concluída.

4. Quando a solicitação anterior for concluída, use o seguinte para iniciar o serviço:

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Depois que o serviço for reiniciado, ele usará as novas configurações.

5. Por fim, use o seguinte para desativar o modo de manutenção:

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##Próximas etapas

Para obter uma referência completa da API REST, consulte [Referência de API do Ambari, V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Algumas funcionalidades do Ambari estão desabilitadas, já que ele é gerenciado pelo serviço de nuvem HDInsight; por exemplo, adicionar ou remover hosts do cluster ou adicionar novos serviços.

<!---HONumber=AcomDC_0727_2016-->