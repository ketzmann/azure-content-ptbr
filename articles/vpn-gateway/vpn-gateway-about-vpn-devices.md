<properties 
   pageTitle="Sobre dispositivos VPN para conexões de Gateway de VPN Site a Site para Redes Virtuais do Azure | Microsoft Azure"
   description="Saiba mais sobre dispositivos VPN e os parâmetros de IPsec para conexões do Gateway de VPN S2S. Conexões Site a Site podem ser usadas para configurações híbridas. Este artigo contém links para exemplos e instruções de configuração de dispositivos de gateway de VPN."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# Sobre dispositivos VPN para conexões de Gateway de VPN Site a Site

Um dispositivo VPN é necessário para configurar uma conexão VPN S2S (Site a Site). Conexões Site a Site podem ser usadas para criar uma solução híbrida ou sempre que desejar uma conexão segura entre sua rede local e sua rede virtual. Este artigo aborda os dispositivos VPN compatíveis e parâmetros de configuração. Observe que, ao configurar uma conexão Site a Site, um endereço IP IPv4 público é necessário para seu dispositivo VPN.

Se o dispositivo não aparecer na tabela [Dispositivos VPN validados](#devicetable), consulte a seção [Dispositivos VPN não validados](#additionaldevices) deste artigo. É possível que seu dispositivo ainda possa funcionar com o Azure. Para suporte ao dispositivo VPN, entre em contato com o fabricante do dispositivo.

**Itens a ser observados ao exibir as tabelas:**

- Houve uma mudança de terminologia do roteamento estático e dinâmico. Provavelmente, você executará em ambos os termos. Não há alterações de funcionalidade, somente os nomes estão sendo alterados.
	- Roteamento estático = baseado em política
	- Roteamento dinâmico = baseado em rota
- As especificações de gateway de VPN de Alto Desempenho e de gateway de VPN baseado em rota são as mesmas, salvo indicação em contrário. Por exemplo, os dispositivos VPN validados compatíveis com os gateways de VPN baseado em rota também são compatíveis com o gateway de VPN de Alto Desempenho do Azure.


## <a name="devicetable"></a>Dispositivos de VPN validados 

Validamos um conjunto de dispositivos VPN padrão em parceria com fornecedores de dispositivos. Todos os dispositivos nas famílias de dispositivos contidos na lista abaixo devem funcionar com os gateways de VPN do Azure. Consulte [Sobre o Gateway de VPN](vpn-gateway-about-vpngateways.md) para verificar o tipo de gateway que você precisa criar para a solução que deseja configurar.

Para ajudar a configurar seu dispositivo VPN, veja os links que correspondem à família de dispositivos apropriada.



| **Fornecedor** | **Família do dispositivo** | **Versão mínima do sistema operacional** | **Baseado em política** | **Baseado em rota** |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis | Série AR de roteadores VPN | 2\.9.2 | Em breve | Não compatível |
| Barracuda Networks, Inc. | Barracuda NextGen Firewall F-series | Baseado em política: 5.4.3, Baseado em rota: 6.2.0 | [Instruções de configuração](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Instruções de configuração](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. | Barracuda NextGen Firewall X-series | Barracuda Firewall 6.5 | [Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Não compatível |
| Brocade | Vyatta 5400 vRouter | Roteador virtual 6.6R3 GA | [Instruções de configuração](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) | Não compatível |
| Ponto de Verificação | Gateway de segurança | R75.40, R75.40VS | [Instruções de configuração](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) | [Instruções de configuração](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco | ASA | 8\.3 | [Exemplos da Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) | Não compatível |
| Cisco | ASR | IOS 15.1 (baseado em política), IOS 15.2 (baseado em rota) | [Exemplos da Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) | [Exemplos da Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco | ISR | IOS 15.0 (baseado em política), IOS 15.1 (baseado em rota*) | [Exemplos da Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) | [Exemplos da Cisco*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix | NetScaler MPX, SDX, VPX |10\.1 e acima | [Instruções de integração](https://docs.citrix.com/pt-BR/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) | Não compatível |
| Dell SonicWALL | Séries TZ, NSA, SuperMassive e NSA classe E | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646) | [Instruções - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instruções - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) | [Instruções - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instruções - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| F5 | Série BIG-IP | N/D | [Instruções de configuração](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) | Não compatível |
| Fortinet | FortiGate | FortiOS 5.2.7 | [Instruções de configuração](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure) | [Instruções de configuração](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure) |
| Internet Initiative Japan (IIJ) | Série SEIL | SEIL/X 4.60, SEIL/B1 4.60, SEIL/x86 3.20 | [Instruções de configuração](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) | Não compatível |
| Juniper | SRX | JunOS 10.2 (baseado em política) e JunOS 11.4 (baseado em rota) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper | Série J | JunOS 10.4r9 (baseado em política) e JunOS 11.4 (baseado em rota) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper | ISG | ScreenOS 6.3 (baseado em política e baseado em rota) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper | SSG | ScreenOS 6.2 (baseado em política e baseado em rota) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) | [Exemplos do Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft | Serviço de Roteamento e Acesso Remoto | Windows Server 2012 | Não compatível | [Exemplos da Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| AG de sistemas abertos | Gateway de Segurança de Controle de Missão | N/D | [Guia de instalação](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Guia de instalação](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan | Openswan | 2\.6.32 | (Em breve) | Não compatível |
| Redes Palo Alto | Todos os dispositivos executando PAN-OS | PAN-OS 6.1.5 ou posterior (baseado em políticas), PAN-OS 7.0.5 ou posterior (baseado em rota) | [Instruções de configuração](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) | [Instruções de configuração](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| Watchguard | Todos | Fireware XTM v11.x | [Instruções de configuração](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/) | Compativel na versão 11.11.2 ou superior |

(*) Os roteadores da Série ISR 7200 só oferecem suporte a VPNs baseadas em política.

## <a name="additionaldevices"></a>Dispositivos VPN não validados

Se você não vir seu dispositivo listado na tabela de dispositivos VPN Validados (acima), ele ainda pode funcionar com uma conexão Site a Site. Verifique se seu dispositivo VPN atende aos requisitos mínimos descritos na seção Requisitos do Gateway do artigo [Sobre Gateways de VPN](vpn-gateway-about-vpngateways.md#gateway-requirements). Os dispositivos que atendem aos requisitos mínimos devem funcionar bem com os gateways de VPN. Entre em contato com o fabricante do dispositivo para obter instruções adicionais de configuração e suporte.


## Edição de exemplos de configuração de dispositivo

Depois de baixar o exemplo de configuração de dispositivo VPN fornecido, você precisará substituir alguns dos valores para refletir as configurações do seu ambiente.

**Para editar um exemplo:**

1. Abra o exemplo usando o Bloco de Notas.
1. Pesquise e substitua todas as cadeias de caracteres de <*texto*> por valores condizentes com seu ambiente. Inclua < e >. Quando um nome é especificado, o nome que você selecionar deve ser exclusivo. Se um comando não funcionar, consulte a documentação do fabricante do dispositivo.

| **Texto de exemplo** | **Alterar para** |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP\_OnPremisesNetwork&gt; | O nome que você escolheu para este objeto. Exemplo: myOnPremisesNetwork |
| &lt;RP\_AzureNetwork&gt; | O nome que você escolheu para este objeto. Exemplo: myAzureNetwork |
| &lt;RP\_AccessList&gt; | O nome que você escolheu para este objeto. Exemplo: myAzureAccessList |
| &lt;RP\_IPSecTransformSet&gt; | O nome que você escolheu para este objeto. Exemplo: myIPSecTransformSet |
| &lt;RP\_IPSecCryptoMap&gt; | O nome que você escolheu para este objeto. Exemplo: myIPSecCryptoMap |
| &lt;SP\_AzureNetworkIpRange&gt; | Especifique o intervalo. Exemplo: 192.168.0.0 |
| &lt;SP\_AzureNetworkSubnetMask&gt; | Especificar máscara de sub-rede. Exemplo: 255.255.0.0 |
| &lt;SP\_OnPremisesNetworkIpRange&gt; | Especifique o intervalo local. Exemplo: 10.2.1.0 |
| &lt;SP\_OnPremisesNetworkSubnetMask&gt; | Especifique a máscara de sub-rede local. Exemplo: 255.255.255.0 |
| &lt;SP\_AzureGatewayIpAddress&gt; | Estas informações são específicas à sua rede virtual e estão localizadas no Portal de Gerenciamento como o **endereço IP do Gateway**. |
| &lt;SP\_PresharedKey&gt; | Essas informações são específicas da sua rede virtual e estão localizadas no Portal de Gerenciamento como Gerenciar Chave. |



## Parâmetros IPsec

>[AZURE.NOTE] Embora os valores listados na tabela a seguir sejam suportados pelo Gateway de VPN do Azure, atualmente, não é possível especificar nem selecionar uma combinação específica no Gateway de VPN do Azure. Você deve especificar quaisquer restrições no dispositivo VPN local. Além disso, você deve fixar MSS em 1350.

### Fase 1 da configuração IKE

| **Propriedade** | **Baseado em política** | **Gateway de VPN baseado em rota e Padrão ou de Alto Desempenho** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| Versão IKE | IKEv1 | IKEv2 |
| Grupo Diffie-Hellman | Grupo 2 (1024 bits) | Grupo 2 (1024 bits) |
| Método de autenticação | Chave Pré-Compartilhada | Chave Pré-Compartilhada |
| Algoritmos de criptografia | AES256 AES128 3DES | AES256 3DES |
| Algoritmo de hash | SHA1(SHA128) | SHA1(SHA128), SHA2(SHA256) |
| Tempo de vida (tempo) da SA (associação de segurança) da fase 1 | 28\.800 segundos | 10\.800 segundos |


### Fase 2 da configuração IKE

| **Propriedade** | **Baseado em política** | **Gateway de VPN baseado em rota e Padrão ou de Alto Desempenho** |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| Versão IKE | IKEv1 | IKEv2 |
| Algoritmo de hash | SHA1(SHA128) | SHA1(SHA128) |
| Tempo de vida (tempo) da SA (associação de segurança) da fase 2 | 3\.600 segundos | 3\.600 segundos |
| Tempo de vida (Tempo) da SA (Associação de Segurança) da fase 2 | 102.400.000 KB | - |
| Ofertas de Criptografia e Autenticação IPsec SA (em ordem de preferência) | 1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. N/D | Consulte *Ofertas de SA (Associação de Segurança) de IPsec do Gateway baseado em Rota* (abaixo) |
| PFS (Perfect Forward Secrecy) | Não | Sim (DH Group 1, 2, 5, 14, 24) |
| Detecção de Par Inativo | Sem suporte | Com suporte |

### Ofertas de SA (Associação de Segurança) de IPsec do gateway baseado em rota

A tabela a seguir lista as Ofertas de Criptografia e Autenticação IPsec SA. Ofertas estão listadas na ordem de preferência que a oferta é apresentada ou aceita.

| **Ofertas de criptografia e autenticação IPsec SA** | **Gateway do Azure como iniciador** | **Gateway do Azure como respondente** |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1 | ESP AES\_256 SHA | ESP AES\_128 SHA |
| 2 | ESP AES\_128 SHA | ESP 3\_DES MD5 |
| 3 | ESP 3\_DES MD5 | ESP 3\_DES SHA |
| 4 | ESP 3\_DES SHA | AH SHA1 com ESP AES\_128 com HMAC nulo |
| 5 | AH SHA1 com ESP AES\_256 com HMAC nulo | AH SHA1 com ESP 3\_DES com HMAC nulo |
| 6 | AH SHA1 com ESP AES\_128 com HMAC nulo | AH MD5 com ESP 3\_DES com HMAC nulo, sem tempos de vida propostos |
| 7 | AH SHA1 com ESP 3\_DES com HMAC nulo | AH SHA1 com ESP 3\_DES SHA1, sem tempos de vida |
| 8 | AH MD5 com ESP 3\_DES com HMAC nulo, sem tempos de vida propostos | AH MD5 com ESP 3\_DES MD5, sem tempos de vida |
| 9 | AH SHA1 com ESP 3\_DES SHA1, sem tempos de vida | ESP DES MD5 |
| 10 | AH MD5 com ESP 3\_DES MD5, sem tempos de vida | ESP DES SHA1, sem tempos de vida |
| 11 | ESP DES MD5 | AH SHA1 com ESP DES HMAC nulo, sem tempos de vida propostos |
| 12 | ESP DES SHA1, sem tempos de vida | AH MD5 com ESP DES HMAC nulo, sem tempos de vida propostos |
| 13 | AH SHA1 com ESP DES HMAC nulo, sem tempos de vida propostos | AH SHA1 com ESP DES SHA1, sem tempos de vida |
| 14 | AH MD5 com ESP DES HMAC nulo, sem tempos de vida propostos | AH MD5 com ESP DES MD5, sem tempos de vida |
| 15 | AH SHA1 com ESP DES SHA1, sem tempos de vida | ESP SHA, sem tempos de vida |
| 16 | AH MD5 com ESP DES MD5, sem tempos de vida | ESP MD5, sem tempos de vida |
| 17 | - | AH SHA, sem tempos de vida |
| 18 | - | AH MD5, sem tempos de vida |


- Você pode especificar a criptografia NULL de IPsec ESP com gateways de VPN baseados em rota e de Alto Desempenho. Criptografia com base em nulo não oferece proteção para dados em trânsito, devendo ser usada apenas quando for exigido o máximo de taxa de transferência com o mínimo de latência mínima. Os clientes podem optar por usar isso nos cenários de comunicação entre vnets ou quando a criptografia está sendo aplicada em outro lugar na solução.

- Para a conectividade entre locais através da Internet, use as configurações padrão de gateway de VPN do Azure com criptografia e algoritmos de hash listados nas tabelas acima para garantir a segurança da comunicação crítica.

<!----HONumber=AcomDC_0810_2016-->
