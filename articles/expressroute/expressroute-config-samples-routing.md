<properties
   pageTitle="Exemplos de configuração de roteador de cliente Rota Expressa | Microsoft Azure"
   description="Esta página fornece exemplos de configuração do roteador para os roteadores da série Cisco ASA e Juniper."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/19/2016"
   ms.author="cherylmc"/>

# Exemplos de configuração do roteador para configurar e gerenciar o roteamento

Esta página fornece interface e modelos de configuração de roteamento para roteadores da série Cisco IOS-XE e Juniper MX. Devem ser exemplos para obter orientação apenas e não devem ser usados como estão. Você pode trabalhar com o fornecedor para exibir as configurações apropriadas para sua rede.

>[AZURE.IMPORTANT] Exemplos nesta página devem ser exclusivamente para obter orientação. Trabalhe com a equipe de vendas / equipe técnica e sua equipe de rede para exibir as configurações adequadas para atendar às suas necessidades. A Microsoft não oferecerá suporte a problemas relacionados a configurações listadas nesta página. Você deve entrar em contato com o fornecedor do dispositivo para problemas de suporte.

Os modelos de configuração abaixo se aplicam a todos os emparelhamentos. Examine [emparelhamentos Rota Expressa](expressroute-circuit-peerings.md) e [requisitos de roteamento Rota Expressa](expressroute-routing.md) para obter mais detalhes.

## Roteadores com base em Cisco IOS-XE

Os modelos nesta seção se aplicam a qualquer roteador que esteja executando a família de sistemas operacionais IOS XE.

### 1\. Configurando interfaces e sub-interfaces

Você precisará de uma sub interface por emparelhamento em cada roteador, que o conecte à Microsoft. Uma interface de sub-rotina pode ser identificada com uma ID de VLAN ou um par empilhado de IDs VLAN e um endereço IP.

#### Definição da interface Dot1Q

Este exemplo fornece a definição de sub-interface para a sub-interface com uma ID de VLAN única. A ID de VLAN é exclusiva por emparelhamento. O último octeto de seu endereço IPv4 sempre será um número ímpar.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### Definição da interface QinQ

Este exemplo fornece a definição de sub-interface para a sub-interface com duas IDs de VLAN única. ID da VLAN externa (s-tag), se usada permanece a mesma em todos os emparelhamentos. A ID de VLAN interna (marca c) é exclusiva por emparelhamento. O último octeto de seu endereço IPv4 sempre será um número ímpar.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### 2\. Configuração das sessões eBGP

Você deve configurar uma sessão BGP com a Microsoft para cada emparelhamento. O exemplo a seguir lhe permite configurar uma sessão BGP com a Microsoft. Se o endereço IPv4 usado para a sua sub interface era a.b.c. d, o endereço IP do vizinho BGP (Microsoft) será a.b.c.d+1. O último octeto do endereço de IPv4 do vizinho BGP sempre será um número par.

	router bgp <Customer_ASN>
	 bgp log-neighbor-changes
	 neighbor <IP#2_used_by_Azure> remote-as 12076
	 !        
	 address-family ipv4
	 neighbor <IP#2_used_by_Azure> activate
	 exit-address-family
	!

### 3\. Configurando os prefixos anunciados sobre a sessão BGP

Você pode configurar seu roteador para anunciar prefixos selecionados para a Microsoft. Você pode fazer isso usando o exemplo a seguir.

	router bgp <Customer_ASN>
	 bgp log-neighbor-changes
	 neighbor <IP#2_used_by_Azure> remote-as 12076
	 !        
	 address-family ipv4
	  network <Prefix_to_be_advertised> mask <Subnet_mask>
	  neighbor <IP#2_used_by_Azure> activate
	 exit-address-family
	!

### 4\. Mapas de rotas

Você pode usar mapas de rotas e listas de prefixo para prefixos de filtro propagados em sua rede. Você pode usar o exemplo a seguir para realizar a tarefa. Certifique-se de que as listas de prefixo foram configuradas apropriadamente.

	router bgp <Customer_ASN>
	 bgp log-neighbor-changes
	 neighbor <IP#2_used_by_Azure> remote-as 12076
	 !        
	 address-family ipv4
	  network <Prefix_to_be_advertised> mask <Subnet_mask>
	  neighbor <IP#2_used_by_Azure> activate
	  neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
	 exit-address-family
	!
	route-map <MS_Prefixes_Inbound> permit 10
	 match ip address prefix-list <MS_Prefixes>
	!


## Roteadores da série Juniper MX 

Os exemplos nesta seção se aplicam aos os roteadores da série Juniper MX.

### 1\. Configurando interfaces e sub-interfaces

#### Definição da interface Dot1Q

Este exemplo fornece a definição de sub-interface para a sub-interface com uma ID de VLAN única. A ID de VLAN é exclusiva por emparelhamento. O último octeto de seu endereço IPv4 sempre será um número ímpar.

    interfaces {
    	vlan-tagging;
    	<Interface_Number> {
    		unit <Number> {
    			vlan-id <VLAN_ID>;
    			family inet {
    				address <IPv4_Address/Subnet_Mask>;
    			}
    		}
    	}
    }


#### Definição da interface QinQ

Este exemplo fornece a definição de sub-interface para a sub-interface com duas IDs de VLAN única. ID da VLAN externa (s-tag), se usada permanece a mesma em todos os emparelhamentos. A ID de VLAN interna (marca c) é exclusiva por emparelhamento. O último octeto de seu endereço IPv4 sempre será um número ímpar.

	interfaces {
	    <Interface_Number> {
	        flexible-vlan-tagging;
	        unit <Number> {
	            vlan-tags outer <S-tag> inner <C-tag>;
	            family inet {
	                address <IPv4_Address/Subnet_Mask>;
	            }                           
	        }                               
	    }                                   
	}                           

### 2\. Configuração das sessões eBGP

Você deve configurar uma sessão BGP com a Microsoft para cada emparelhamento. O exemplo a seguir lhe permite configurar uma sessão BGP com a Microsoft. Se o endereço IPv4 usado para a sua sub interface era a.b.c. d, o endereço IP do vizinho BGP (Microsoft) será a.b.c.d+1. O último octeto do endereço de IPv4 do vizinho BGP sempre será um número par.

	routing-options {
	    autonomous-system <Customer_ASN>;
	}
	}
	protocols {
	    bgp { 
	        group <Group_Name> { 
	            peer-as 12076;              
	            neighbor <IP#2_used_by_Azure>;
	        }                               
	    }                                   
	}

### 3\. Configurando os prefixos anunciados sobre a sessão BGP

Você pode configurar seu roteador para anunciar prefixos selecionados para a Microsoft. Você pode fazer isso usando o exemplo a seguir.

	policy-options {
	    policy-statement <Policy_Name> {
	        term 1 {
	            from protocol OSPF;
		route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
	            then {
	                accept;
	            }
	        }
	    }
	}
	protocols {
	    bgp { 
	        group <Group_Name> { 
	            export <Policy_Name>
	            peer-as 12076;              
	            neighbor <IP#2_used_by_Azure>;
	        }                               
	    }                                   
	}


### 4\. Mapas de rotas

Você pode usar mapas de rotas e listas de prefixo para prefixos de filtro propagados em sua rede. Você pode usar o exemplo a seguir para realizar a tarefa. Certifique-se de que as listas de prefixo foram configuradas apropriadamente.

	policy-options {
	    prefix-list MS_Prefixes {
	        <IP_Prefix_1/Subnet_Mask>;
	        <IP_Prefix_2/Subnet_Mask>;
	    }
	    policy-statement <MS_Prefixes_Inbound> {
	        term 1 {
	            from {
		prefix-list MS_Prefixes;
	            }
	            then {
	                accept;
	            }
	        }
	    }
	}
	protocols {
	    bgp { 
	        group <Group_Name> { 
	            export <Policy_Name>
	            import <MS_Prefixes_Inbound>
	            peer-as 12076;              
	            neighbor <IP#2_used_by_Azure>;
	        }                               
	    }                                   
	}

## Próximas etapas

Consulte as [Perguntas Frequentes sobre Rota Expressa](expressroute-faqs.md) para obter mais detalhes.

<!---HONumber=AcomDC_0720_2016-->