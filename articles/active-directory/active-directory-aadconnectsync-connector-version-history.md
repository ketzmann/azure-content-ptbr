<properties
   pageTitle="Sincronização do Azure AD Connect: Histórico de liberação da versão do Conector | Microsoft Azure"
   description="Este tópico lista todas as versões dos Conectores do FIM (Forefront Identity Manager) e MIM (Microsoft Identity Manager)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="05/24/2016"
   ms.author="andkjell"/>

# Sincronização do Azure AD Connect: Histórico de liberação da versão do Conector
Os Conectores do FIM (Forefront Identity Manager) e MIM (Microsoft Identity Manager) são atualizados com frequência.

Este artigo foi projetado para ajudá-lo a controlar as versões que foram lançadas e compreender se você precisa atualizar para a versão mais recente ou não.

Links relacionados:

- [Baixar os Conectores mais recentes](http://go.microsoft.com/fwlink/?LinkId=717495)
- Documentação de referência do [Conector LDAP Genérico](active-directory-aadconnectsync-connector-genericldap.md)
- Documentação de referência do [Conector do SQL Genérico](active-directory-aadconnectsync-connector-genericsql.md)
- Documentação de referência do [Conector dos Serviços Web](http://go.microsoft.com/fwlink/?LinkID=226245)
- Documentação de referência do [Conector do PowerShell](active-directory-aadconnectsync-connector-powershell.md)
- Documentação de referência do [Conector do Lotus Domino](active-directory-aadconnectsync-connector-domino.md)

## 1\.1.117.0
Lançamento: março de 2016

**Novo Conector** Versão inicial do [Conector do SQL Genérico](active-directory-aadconnectsync-connector-genericsql.md).

**Novos recursos:**

- Conector LDAP Genérico:
    - Adicionado suporte para importação delta com o Isode.
- Conector dos Serviços Web:
    - Atualizadas as atividades csEntryChangeResult e setImportErrorCode para permitir que erros no nível de objeto sejam retornados para o mecanismo de sincronização.
    - Atualizados os modelos SAP6 e SAP6User para usar a nova funcionalidade de erro no nível de objeto.
- Conector do Lotus Domino:
    - Ao exportar, é necessário ter um certificador por catálogo de endereços. Agora é possível usar a mesma senha para todos os certificadores, a fim de facilitar o gerenciamento.

**Problemas corrigidos:**

- Conector LDAP Genérico:
    - Para o IBM Tivoli DS, alguns atributos de referência não foram detectados corretamente.
    - Para o Open LDAP durante a importação delta, os espaços em branco no início e no final das cadeias de caracteres foram truncados.
    - Para o Novell e NetIQ, houve falha em uma exportação que movia um objeto entre UOs/contêineres e ao mesmo tempo renomeava o objeto.
- Conector dos Serviços Web:
    - Se o serviço Web tinha vários pontos de extremidade para a mesma associação, o conector não os descobriu corretamente.
- Conector do Lotus Domino:
    - Uma exportação do atributo fullName para um banco de dados de entrada de email não funcionou.
    - Uma exportação que adicionava e removia um membro de um grupo exportou apenas os membros adicionados.
    - Se um Documento do Notes for inválido (o atributo isValid for definido como false), haverá uma falha do Conector.

## Versões mais antigas
Antes de março de 2016, os Conectores foram liberados como tópicos de suporte.

**LDAP Genérico**

- [KB3078617](https://support.microsoft.com/kb/3078617) – 1.0.0597, setembro de 2015
- [KB3044896](https://support.microsoft.com/kb/3044896) – 1.0.0549, março de 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) – 1.0.0534, janeiro de 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) – 1.0.0419, setembro de 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) – 4.3.1082, março de 2014

**Serviços Web**

- [KB3008178](https://support.microsoft.com/kb/3008178) – 1.0.0419, setembro de 2014

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) – 1.0.0419, setembro de 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) – 1.0.0597, setembro de 2015
- [KB3044895](https://support.microsoft.com/kb/3044895) – 1.0.0549, março de 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) – 5.3.0712, agosto de 2014
- [KB2932635](https://support.microsoft.com/kb/2932635) – 5.3.1003, fevereiro de 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) – 5.3.0721, outubro de 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) – 5.3.0534, agosto de 2013

## Próximas etapas
Saiba mais sobre a configuração de [sincronização do Azure AD Connect](active-directory-aadconnectsync-whatis.md).

Saiba mais sobre [Como integrar suas identidades locais ao Active Directory do Azure](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0525_2016-->