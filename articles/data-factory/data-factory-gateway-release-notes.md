<properties 
	pageTitle="Notas de versão para o Gateway de Gerenciamento de Dados | Azure Data Factory" 
	description="Notas de versão do Gateway de Gerenciamento de Dados" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/17/2016" 
	ms.author="spelluru"/>

# Notas de versão para o Gateway de Gerenciamento de Dados

Um dos desafios da integração de dados moderna é mover dados continuamente para e do local para a nuvem. O data factory torna perfeita essa integração com o Gateway de Gerenciamento de Dados, que é um agente que você pode instalar localmente para habilitar a movimentação de dados híbridos.

Consulte os artigos [Mover dados entre o local e a nuvem usando o Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md) e [Gateway de Gerenciamento de Dados](data-factory-data-management-gateway.md) para obter mais informações.

## VERSÃO ATUAL (2.1.6040.1)

- O driver DB2 agora está incluído no pacote de instalação do gateway. Você não precisa instalá-lo separadamente.
- O driver DB2 agora oferece suporte ao z/OS e DB2 para i (AS/400) junto com as plataformas já suportadas (Linux, Unix e Windows).
- Oferece suporte ao uso do Banco de Dados de Documentos como uma origem ou destino para os armazenamentos de dados locais
- Dá suporte à cópia dos dados de/para o armazenamento de blobs quente/frio junto com a conta de armazenamento de uso geral já com suporte.
- Permite que você conecte o SQL Server local por meio do gateway com privilégios de logon remoto.

## Versões anteriores

## 2\.0.6013.1

- Você pode selecionar o idioma/cultura a ser usada por um gateway durante a instalação manual.
- Quando o gateway não funciona conforme o esperado, você pode optar por enviar logs de gateway dos últimos sete dias à Microsoft para facilitar a solução de problemas do problema. Se gateway não estiver conectado ao serviço de nuvem, você poderá optar por salvar e arquivar logs de gateway.
- Aprimoramentos na interface do usuário para o gerenciador de configuração de gateway:
	- Torne o status do gateway mais visível na guia Página Inicial.
	- Controles reorganizados e simplificados.
- Você pode copiar os dados de um armazenamento que não seja o Blob do Azure para o Azure SQL Data Warehouse via Polybase e blob de preparo usando a [ferramenta de visualização de cópia sem código](data-factory-copy-data-wizard-tutorial.md). Consulte [Cópia em Etapas](data-factory-copy-activity-performance.md#staged-copy) para obter detalhes gerais sobre esse recurso.
- Você pode aproveitar o Gateway de Gerenciamento de Dados para ingerir dados diretamente de um banco de dados do SQL Server local para o Aprendizado de Máquina do Azure.
- Aprimoramentos de desempenho
	- Melhore o desempenho de exibição de Esquema/Visualização no SQL Server na ferramenta de visualização de cópia sem código.



## 1\.12.5953.1
- Correções de bug

## 1\.11.5918.1

- O tamanho máximo do log de eventos do gateway foi aumentado de 1 MB para 40 MB.
- Uma caixa de diálogo de aviso é exibida caso uma reinicialização seja necessária durante a atualização automática do gateway. Você pode optar por reiniciar logo em seguida ou mais tarde.
- Em caso de falha da atualização automática, instalador do gateway tentará novamente executar a atualização automática 3 vezes no máximo.
- Aprimoramentos de desempenho
	- Melhora no desempenho do carregamento de grandes tabelas de servidor local no cenário de cópia sem código.
- Correções de bug

## 1\.10.5892.1

- Aprimoramentos de desempenho
- Correções de bug

## 1\.9.5865.2

- Capacidade de atualização automática zero touch
- Novo ícone de bandeja com indicadores de status do gateway
- Capacidade de "Atualizar agora" do cliente
- Capacidade de definir o horário da agenda de atualização
- Script do PowerShell para ativar/desativar a atualização automática
- Suporte para o formato JSON
- Aprimoramentos de desempenho
- Correções de bug

## 1\.8.5822.1

- Melhorar a experiência de solução de problemas
- Aprimoramentos de desempenho
- Correções de bug

### 1\.7.5795.1

- Aprimoramentos de desempenho
- Correções de bug

### 1\.7.5764.1

- Aprimoramentos de desempenho
- Correções de bug

### 1\.6.5735.1

- Suporte local a coletor/fonte HDFS
- Aprimoramentos de desempenho
- Correções de bug

### 1\.6.5696.1

- Aprimoramentos de desempenho
- Correções de bug

### 1\.6.5676.1

- Suporte a ferramentas de diagnóstico no Gerenciador de Configurações
- Suporte a colunas de tabela para fontes de dados tabulares do Azure Data Factory
- Suporte a SQL DW para Azure Data Factory
- Suporte Reclusivo em BlobSource e FileSource para o Azure Data Factory
- Suporte a CopyBehavior – MergeFiles, PreserveHierarchy e FlattenHierarchy em BlobSink e FileSink com Cópia Binária para o Azure Data Factory
- Suporte a relatórios de andamento de Atividade de Cópia do Azure Data Factory
- Suporte a Validação de Conectividade de Fonte de Dados para Azure Data Factory
- Correções de bug


### 1\.6.5672.1

- Suporte a nome de tabela para fonte de dados ODBC para o Azure Data Factory
- Aprimoramentos de desempenho
- Correções de bug

### 1\.6.5658.1

- Suporte a Coletor de Arquivo para o Azure Data Factory
- Suporte à preservação de hierarquia na cópia binária para o Azure Data Factory
- Suporte a Idempotência de Atividade de Cópia para o Azure Data Factory
- Correções de bug

### 1\.6.5640.1

- Suporte a três ou mais fontes de dados para o Azure Data Factory (ODBC, OData, HDFS)
- Suporte ao caractere de aspas no analisador de csv para o Azure Data Factory
- Suporte à compactação (BZip2)
- Correções de bug

### 1\.5.5612.1

- Suporte a cinco bancos de dados relacionais do Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata e Sybase)
- Suporte à compactação (Gzip e Deflate)
- Aprimoramentos de desempenho
- Correções de bug


### 1\.4.5549.1

- Adicionar suporte a fonte de dados Oracle para o Azure Data Factory
- Aprimoramentos de desempenho
- Correções de bug

### 1\.4.5492.1

- Binário unificado que dá suporte aos serviços Microsoft Azure Data Factory e Office 365 Power BI
- Refinar a interface do usuário de Configuração e o processo de registro
- Azure Data Factory – suporte a Ingresso e Egresso do Azure para fonte de dados do SQL Server

### 1\.2.5303.1

- 	Correção do problema de tempo limite para dar suporte a conexões de fontes de dados mais demoradas.
 	
### 1\.1.5526.8

- Requer o .NET Framework 4.5.1 como pré-requisito durante a instalação.

### 1\.0.5144.2

- Nenhuma alteração que afeta os cenários do Azure Data Factory.

## Perguntas/respostas

### Por que o Gerenciador de Fonte de Dados está tentando se conectar a um gateway?
Esse é um design de segurança em que você só pode configurar fontes de dados locais para acesso à nuvem na rede corporativa, e suas credenciais não fluirão para fora do firewall corporativo. Verifique se o seu computador pode acessar o computador em que o gateway está instalado.

<!---HONumber=AcomDC_0803_2016-->