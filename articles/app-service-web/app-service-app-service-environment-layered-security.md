<properties 
	pageTitle="Arquitetura de segurança em camadas com os Ambientes do Serviço de Aplicativo" 
	description="Implementando uma arquitetura de segurança em camadas com Ambientes do Serviço de Aplicativo." 
	services="app-service" 
	documentationCenter="" 
	authors="stefsch" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/14/2016" 
	ms.author="stefsch"/>

# Implementando uma arquitetura de segurança em camadas com Ambientes do Serviço de Aplicativo

## Visão geral ##
 
Como os Ambientes do Serviço de Aplicativo fornecem um ambiente de tempo de execução isolado implantado em uma rede virtual, os desenvolvedores podem criar uma arquitetura de segurança em camadas fornecendo diferentes níveis de acesso à rede para cada camada de aplicativo físico.

Um desejo comum é ocultar os back-ends de API do acesso à Internet geral e só permitir que as APIs sejam chamadas por aplicativos Web upstream. Os [NSGs (grupos de segurança de rede)][NetworkSecurityGroups] podem ser usados em sub-redes contendo Ambientes do Serviço de Aplicativo para restringir o acesso público aos aplicativos da API.

O diagrama abaixo mostra uma arquitetura de exemplo com um aplicativo baseado em API Web implantado em um Ambiente do Serviço de Aplicativo. Três instâncias de aplicativos Web separados, implantadas em três Ambientes do Serviço de Aplicativo separados, fazem chamadas de back-end para o mesmo aplicativo da API Web.

![Arquitetura conceitual][ConceptualArchitecture]

O sinal de adição verde indica que o grupo de segurança de rede na sub-rede contendo “apiase” permite chamadas recebidas de aplicativos Web upstream, bem como chamadas de si mesmo. No entanto, o mesmo grupo de segurança de rede nega explicitamente o acesso ao tráfego de entrada geral da Internet.

O restante deste tópico explica as etapas necessárias para configurar o grupo de segurança de rede na sub-rede contendo “apiase”.

## Determinando o comportamento de rede ##
Para saber quais regras de segurança de rede são necessárias, você precisa determinar quais clientes de rede terão permissão para acessar o Ambiente do Serviço de Aplicativo que contém o aplicativo de API e quais clientes serão bloqueados.

Como os [NSGs (grupos de segurança de rede)][NetworkSecurityGroups] são aplicados às sub-redes e os Ambientes do Serviço de Aplicativo são implantados em sub-redes, as regras contidas em um NSG se aplicam a **todos** os aplicativos em execução em um Ambiente do Serviço de Aplicativo. Usando a arquitetura de exemplo deste artigo, depois que um grupo de segurança de rede for aplicado à sub-rede contendo “apiase”, todos os aplicativos em execução no Ambiente do Serviço de Aplicativo “apiase” estarão protegidos pelo mesmo conjunto de regras de segurança.

- **Determine o endereço IP de saída dos autores da chamada upstream:** qual é ou quais são os endereços IP dos autores da chamada upstream? Esses endereços precisarão receber explicitamente a permissão de acesso no NSG. Como as chamadas entre os Ambientes do Serviço de Aplicativo são consideradas chamadas da “Internet”, isso significa que o endereço IP de saída atribuído a cada um dos três Ambientes do Serviço de Aplicativo upstream precisa receber permissão de acesso no NSG para a sub-rede “apiase”. Para obter mais detalhes sobre como determinar o endereço IP de saída para aplicativos executados em um Ambiente do Serviço de Aplicativo, confira o artigo de Visão geral da [Arquitetura de rede][NetworkArchitecture].
- **O aplicativo de API de back-end precisará chamar a si mesmo?** Um ponto sutil e às vezes negligenciado é o cenário em que o aplicativo de back-end precisa chamar a si mesmo. Se um aplicativo de API de back-end em um Ambiente do Serviço de Aplicativo precisar chamar a si mesmo, isso também será tratado como uma chamada da “Internet”. Na arquitetura de exemplo, isso também requer a permissão de acesso do endereço IP de saída do Ambiente do Serviço de Aplicativo “apiase”.

## Configurando o Grupo de Segurança de Rede ##
Depois que o conjunto de endereços IP de saída forem conhecidos, a próxima etapa é criar um grupo de segurança de rede. Como atualmente há suporte apenas para os Ambientes do Serviço de Aplicativo nas redes virtuais “v1”, a [configuração do NSG][NetworkSecurityGroupsClassic] é feita com o suporte clássico do NSG no Powershell.

Para a arquitetura de exemplo, como os ambientes estão localizados no Centro Sul dos EUA, um NSG vazio é criado nessa região:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Primeiro uma regra de permissão explícita é adicionada para a infraestrutura de gerenciamento do Azure, como observado no artigo sobre o [tráfego de entrada][InboundTraffic] dos Ambientes do Serviço de Aplicativo.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Em seguida, duas regras foram adicionadas para permitir chamadas HTTP e HTTPS do primeiro Ambiente do Serviço de Aplicativo upstream (“fe1ase”).

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Aplique e repita para o segundo e terceiro Ambientes do Serviço de Aplicativo upstream (“fe2ase” e “fe3ase”).

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Por fim, conceda acesso ao endereço IP de saída do Ambiente do Serviço de Aplicativo da API de back-end para que ele possa chamar de volta a si mesmo.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Nenhuma outra regra de segurança de rede precisa ser configurada, pois cada NSG tem um conjunto de regras padrão que bloqueia o acesso de entrada da Internet por padrão.

A lista completa das regras no grupo de segurança de rede é mostrada abaixo. Observe como a última regra, que é realçada, bloqueia o acesso de entrada de todos os autores da chamada diferentes daqueles que receberam o acesso explicitamente.

![Configuração do NSG][NSGConfiguration]

A etapa final é aplicar o NSG à sub-rede que contém o Ambiente do Serviço de Aplicativo “apiase”.

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Com o NSG aplicado à sub-rede, somente três Ambientes do Serviço de Aplicativo upstream e o Ambiente do Serviço de Aplicativo contendo o back-end da API têm permissão para realizar chamadas no ambiente de “apiase”.


## Informações e links adicionais ##
Todos os artigos e instruções para os Ambientes do Serviço de Aplicativo estão disponíveis no [LEIAME para Ambientes do Serviço de Aplicativo](../app-service/app-service-app-service-environments-readme.md).

Configurando [grupos de segurança de rede][NetworkSecurityGroupsClassic] em redes virtuais clássicas.

Noções básicas sobre [endereços IP de saída][NetworkArchitecture] e Ambientes do Serviço de Aplicativo.

[Portas de rede][InboundTraffic] usadas pelos Ambientes do Serviço de Aplicativo.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]: https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkSecurityGroupsClassic]: https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-classic-ps/
[InboundTraffic]: https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]: ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png

<!---HONumber=AcomDC_0615_2016-->