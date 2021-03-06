<properties 
    pageTitle="Introdução à autenticação baseada em certificado no iOS | Microsoft Azure" 
    description="Saiba como configurar a autenticação baseada em certificado em soluções com dispositivos iOS" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/02/2016" 
    ms.author="markvi" />



# Introdução à autenticação baseada em certificado no iOS — Visualização Pública

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


Este tópico mostra como configurar e utilizar a CBA (Autenticação Baseada em Certificado) em um dispositivo iOS para usuários de locatários nos planos Office 365 Enterprise, Business e Education.

A CBA permite que você seja autenticado pelo Azure Active Directory com um certificado de cliente em um dispositivo Android ou iOS ao conectar sua conta do Exchange Online a:

- Aplicativos móveis do Office, como Microsoft Outlook e Microsoft Word
- Clientes do EAS (Exchange ActiveSync)

Configurar esse recurso elimina a necessidade de digitar uma combinação de nome de usuário e senha em determinados emails e aplicativos do Microsoft Office no seu dispositivo móvel.
 

## Requisitos e cenários com suporte  



### Requisitos gerais 


Em todos os cenários deste tópico, as seguintes tarefas são necessárias:

- Acesso a autoridades de certificação para emitir certificados de cliente.

- As autoridades de certificação devem ser configuradas no Azure Active Directory. Você pode encontrar etapas detalhadas sobre como concluir a configuração na seção [Introdução](#getting-started).

- A autoridade de certificação raiz e qualquer autoridade de certificação intermediária devem ser configuradas no Azure Active Directory.

- Cada autoridade de certificação deve ter uma CRL (Lista de Certificados Revogados) que pode ser referenciada por meio de uma URL para a Internet.

- O certificado de cliente deve ser emitido para autenticação de cliente.


- Somente para clientes do Exchange ActiveSync, o certificado do cliente deve ter endereços de email roteáveis do usuário no Exchange Online, seja no valor Nome da Entidade ou Nome RFC822 do campo Nome Alternativo da Entidade. O Azure Active Directory mapeia o valor de RFC822 para o atributo Endereço de Proxy no diretório.



### Suporte a aplicativos móveis do Office 

| Aplicativos | Suporte |
| ---                       | ---          |
| Word/Excel/PowerPoint | ![Verificação][1] |
| OneNote | ![Verificação][1] |
| OneDrive | ![Verificação][1] |
| Outlook | Em breve |
| Yammer | ![Verificação][1] |
| Skype for Business | Em breve |


### Requisitos  

A versão do sistema operacional do dispositivo deve ser iOS 9 e superior

Um servidor de federação deve ser configurado.

É necessário um Azure Authenticator para aplicativos do Office em iOS.

Para que o Azure Active Directory revogue um certificado do cliente, o token ADFS deve ter as seguintes declarações:

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (O número de série do certificado do cliente)

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (A cadeia de caracteres para o emissor do certificado do cliente)

O Azure Active Directory adiciona essas declarações ao token de atualização se elas estiverem disponíveis no token ADFS (ou qualquer outro token SAML). Quando o token de atualização precisa ser validado, essas informações são usadas para verificar a revogação.

Como prática recomendada, você deve atualizar as páginas de erro do ADFS com o seguinte:

- O requisito para instalar o Azure Authenticator no iOS

- Instruções sobre como obter um certificado de usuário.

Para obter mais detalhes, confira [Personalizando as páginas de entrada do AD FS](https://technet.microsoft.com/library/dn280950.aspx).



### Suporte aos clientes do Exchange ActiveSync 


No iOS 9 ou posterior, há suporte para o cliente de email do iOS nativo. Para todos os outros aplicativos do Exchange ActiveSync, para determinar se há suporte para esse recurso, contate o desenvolvedor do aplicativo.



## Introdução 


Para começar, você precisa configurar autoridades de certificação no Azure Active Directory. Para cada autoridade de certificação, carregue o seguinte:

- A parte pública do certificado, no formato *.cer*

- As URLs para a Internet, em que as CRLs (Listas de Certificados Revogados) residem
 

Veja abaixo o esquema de uma autoridade de certificação:

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Para carregar as informações, você pode usar o módulo do Azure AD por meio do Windows PowerShell. Veja abaixo os exemplos para adicionar, remover ou modificar uma autoridade de certificação.



### Configurando o locatário do Azure AD para autenticação baseada em certificado 

1. Inicie o Windows PowerShell com os privilégios de administrador.

2. Instale o módulo do Azure AD. Você precisa instalar a versão [1\.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) ou superior.

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Conecte-se ao locatário de destino:

        Connect-AzureAD 

### Adicionando uma nova autoridade de certificação

1. Defina várias propriedades da autoridade de certificação e adicione-a ao Azure Active Directory:

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Obtenha as Autoridades de Certificação:

        Get-AzureADTrustedCertificateAuthority 


### Recuperando as autoridades de certificação de lista

Recupere as autoridades de certificação atualmente armazenadas no Azure Active Directory do seu locatário:

        Get-AzureADTrustedCertificateAuthority 


### Removendo uma autoridade de certificação

1.	Recupere as autoridades de certificação:

		$c=Get-AzureADTrustedCertificateAuthority 


2. Remova o certificado da autoridade de certificação:

		Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### Modificando uma autoridade de certificação 

1.	Recupere as autoridades de certificação:

		$c=Get-AzureADTrustedCertificateAuthority 


2. Modifique as propriedades na autoridade de certificação:

		$c[0].AuthorityType=1 

3. Defina a **Autoridade de Certificação**:

		Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## Testando aplicativos móveis do Office  

Para testar a autenticação do certificado em seu aplicativo móvel do Office:

1.	Em seu dispositivo de teste, instale um aplicativo móvel do Office (por exemplo, OneDrive) da App Store.

2.	Verifique se o certificado de usuário foi provisionado para seu dispositivo de teste.

3.	Inicie o aplicativo.

4.	Insira seu nome de usuário e escolha o certificado de usuário que deseja usar.

Você deve se conectar com êxito.





## Testando aplicativos cliente do Exchange ActiveSync

Para acessar o Exchange ActiveSync por meio da autenticação baseada em certificado, um perfil do EAS que contém o certificado do cliente deve estar disponível para o aplicativo. O perfil do EAS deve conter as seguintes informações:

- O certificado do usuário a ser usado para autenticação

- O ponto de extremidade do EAS deve ser outlook.office365.com (pois esse recurso atualmente dá suporte apenas ao ambiente multilocatário do Exchange Online)

Um perfil do EAS pode ser configurado e colocado no dispositivo por meio da utilização de um MDM, como o Intune, ou colocando o certificado manualmente no perfil do EAS no dispositivo.

### Testando os aplicativos cliente do EAS no iOS 

Para testar a autenticação do certificado com o aplicativo de email nativo do iOS 9 ou superior:

1.	Configure um perfil do EAS que atenda aos requisitos acima.

2.	Instale o perfil no dispositivo iOS (usando um MDM, como o Intune ou o aplicativo Apple Configurator)

3.	Depois que o perfil estiver corretamente instalado, abra o aplicativo Mail nativo e verifique se o email está sincronizando



## Revogação

Para revogar um certificado do cliente, o Azure Active Directory busca a CRL (Lista de Certificados Revogados) nas URLs carregadas como parte das informações da autoridade de certificado e a armazena em cache. O carimbo de data/hora da última publicação (propriedade **Effective Date**) na CRL é usado para garantir que a CRL continua sendo válida. A CRL é referenciada periodicamente para revogar o acesso a certificados que fazem parte da lista.

Se uma revogação mais imediata for necessária (por exemplo, se um usuário perder um dispositivo), o token de autorização do usuário poderá ser invalidado. Para invalidar o token de autorização, defina o campo **StsRefreshTokenValidFrom** para esse usuário específico usando o Windows PowerShell. Você deve atualizar o campo **StsRefreshTokenValidFrom** para cada usuário do qual deseja revogar o acesso.
 
Para garantir que a revogação persista, é preciso definir **Effective Date** da CRL para uma data posterior ao valor definido por **StsRefreshTokenValidFrom** e garantir que o certificado em questão esteja na CRL.
 
As etapas a seguir descrevem o processo para atualizar e invalidar o token de autorização pela definição do campo **StsRefreshTokenValidFrom**.

1. Conecte-se com credenciais de administrador ao serviço MSOL:

		$msolcred = get-credential 
		connect-msolservice -credential $msolcred 

1.	Recupere o valor StsRefreshTokensValidFrom atual para um usuário:

		$user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
		$user.StsRefreshTokensValidFrom 


1.	Configure um novo valor StsRefreshTokensValidFrom para o usuário igual ao carimbo de data/hora atual:

		Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


A data que você define deve estar no futuro. Se a data não estiver no futuro, a propriedade **StsRefreshTokensValidFrom** não será definida. Se a data estiver no futuro, **StsRefreshTokensValidFrom** será definida para a hora atual (não a data indicada pelo comando Set-MsolUser).



<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png

<!---HONumber=AcomDC_0803_2016-->