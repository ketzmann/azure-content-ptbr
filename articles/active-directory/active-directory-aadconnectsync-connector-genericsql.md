<properties
   pageTitle="Sincronização do Azure AD Connect: conector SQL genérico | Microsoft Azure"
   description="Este artigo descreve como configurar o conector SQL genérico da Microsoft."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/24/2016"
   ms.author="andkjell"/>

# Referência técnica do conector SQL genérico

Este artigo descreve o conector SQL genérico. O artigo se aplica aos seguintes produtos:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   É necessário usar o hotfix 4.1.3671.0 ou posterior [KB3092178](https://support.microsoft.com/kb/3092178).

Para MIM2016 e FIM2010R2 o conector está disponível para download do [Centro de Download da Microsoft](http://go.microsoft.com/fwlink/?LinkId=717495).

Para ver esse conector em ação, consulte o artigo [Generic SQL Connector step-by-step](active-directory-aadconnectsync-connector-genericsql-step-by-step.md).

## Visão geral do conector SQL genérico

O conector SQL genérico permite que você integre o serviço de sincronização com um sistema de banco de dados que oferece conectividade ODBC.

De uma perspectiva de alto nível, os seguintes recursos tem suporte pela versão atual do conector:

Recurso | Suporte
--- | ---
Fonte de dados conectada | O conector é compatível com todos os drivers ODBC de 64 bits. Ele foi testado com o seguinte: <li>Microsoft SQL Server & SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 & 11g</li><li>MySQL 5.x</li>
Cenários | <li>Gerenciamento de ciclo de vida do objeto</li><li>Gerenciamento de senhas</li>
Operações | <li>Importação completa e Importação Delta, Exportação</li><li>Para exportação: Adicionar, Excluir, Atualizar e Substituir</li><li>Definir senha, Alterar senha</li>
Esquema | <li>Detecção dinâmica de objetos e atributos</li>

### Pré-requisitos

Para usar o conector, verifique se você tem os seguintes itens no servidor de sincronização, além de qualquer hotfix mencionado acima:

- Microsoft .NET 4.5.2 Framework ou posterior
- Drivers de cliente ODBC de 64 bits

### Permissões na fonte de dados conectada

Para criar ou executar qualquer uma das tarefas com suporte no conector SQL genérico, você deve ter:

- db\_datareader
- db\_datawriter

### Portas e protocolos

Para as portas necessárias ao funcionamento do driver ODBC, consulte a documentação do fornecedor do banco de dados.

## Criar um novo conector

Para criar um conector SQL genérico, em **Serviço de Sincronização** selecione **Agente de Gerenciamento** e **Criar**. Selecione o **Conector SQL genérico (Microsoft)**.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### Conectividade

O conector usa um arquivo DSN ODBC para conectividade. Crie o arquivo DSN usando **Fontes de Dados ODBC** encontrado no menu Iniciar em **Ferramentas Administrativas**. A ferramenta administrativa, cria um **DSN de Arquivo** para que ele possa ser fornecido ao conector.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

A tela Conectividade é a primeira quando você cria um novo conector SQL genérico. Primeiro, forneça as seguintes informações:

- Caminho do arquivo DSN
- Autenticação
    - Nome de usuário
    - Senha

O banco de dados deve oferecer suporte a um dos métodos de autenticação abaixo.

- **Autenticação do Windows**: o banco de dados de autenticação usará as credenciais do Windows para verificar o usuário. O nome de usuário/senha especificados serão usados para autenticar com o banco de dados. Essa conta precisará de permissões para o banco de dados.
- **Autenticação do SQL**: o banco de dados de autenticação usará o nome de usuário/senha definido na tela Conectividade para se conectar ao banco de dados. Se você armazenar a nome de usuário/senha no arquivo DSN, as credenciais fornecidas na tela Conectividade terão precedência.
- **Autenticação do banco de dados SQL do Azure**: para mais informações, veja [Conectar ao banco de dados SQL usando a autenticação do Active Directory do Azure](..\sql-database\sql-database-aad-authentication.md)

**DN é Âncora**: se você selecionar esta opção, o DN também será usado como atributo de âncora. Ele pode ser usado para uma implementação simples, mas também tem as seguintes limitações:

-	O conector só dá suporte a 1 tipo de objeto. Portanto, qualquer atributo de referência só pode fazer referência ao mesmo tipo de objeto.

**Tipo de exportação: substituição de objeto**: durante a exportação, quando apenas alguns atributos foram alterados, o objeto inteiro com todos os atributos será exportado e substituirá o objeto existente.

### Esquema 1 (Detectar tipos de objeto)

Nesta página você configurará como o conector localizará tipos de objeto diferentes no banco de dados.

Cada tipo de objeto será apresentado como uma partição e configurado ainda mais em **Configurar Partições e Hierarquias**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Método de detecção do Tipo de objeto**: o conector dá suporte a esses métodos de detecção do tipo de objeto.

- **Valor Fixo**: forneça a lista de tipos de objeto como uma lista separada por vírgulas. Por exemplo Usuário, Grupo, Departamento. ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Tabela/exibição/procedimento armazenado**: forneça o nome da tabela/exibição/procedimento armazenado e, em seguida, o nome da coluna que fornece a lista de tipos de objeto. Se você usar um procedimento armazenado, forneça também parâmetros para ele no formato **[Nome]:[Direção]:[Valor]**. Forneça cada parâmetro em uma linha separada (use Ctrl + Enter para obter uma nova linha). ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **Consulta SQL**: essa opção permite que você forneça uma consulta SQL que retorna uma única coluna com tipos de objeto, por exemplo, `SELECT [Column Name] FROM TABLENAME`. A coluna retornada deve ser do tipo cadeia de caracteres (varchar).

### Esquema 2 (Detectar tipos de atributo)

Nesta página, você configurará como os nomes e tipos de atributos serão detectados. As opções de configuração são listadas para cada tipo de objeto detectado na página anterior.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Método de detecção do tipo de atributo**: o conector dá suporte a esses métodos de detecção do tipo de atributo com cada tipo de objeto detectado na tela Esquema 1.

- **Tabela/exibição/procedimento armazenado**: forneça o nome da tabela/exibição/procedimento armazenado que deve ser usado para localizar nomes de atributo. Se você usar um procedimento armazenado, forneça também parâmetros para ele no formato **[Nome]:[Direção]:[Valor]**. Forneça cada parâmetro em uma linha separada (use Ctrl+Enter para obter uma nova linha). Para detectar nomes de atributo em um atributo de valores múltiplos, forneça uma lista separada por vírgulas de tabelas ou exibições. Não haverá suporte para cenários de valores múltiplos se a tabela pai e filho tiverem os mesmos nomes de coluna.
- **Consulta SQL** Essa opção permite que você forneça uma consulta SQL que retorna uma única coluna com nomes de atributo, por exemplo, `SELECT [Column Name] FROM TABLENAME`. A coluna retornada deve ser do tipo cadeia de caracteres (varchar).

### Esquema 3 (Definir âncora e DN)

Esta página permite que você configure a âncora e o atributo DN para cada tipo de objeto detectado. Você pode selecionar vários atributos para tornar a âncora exclusiva.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Atributos de valores múltiplos e boolianos não são listados.
- O mesmo atributo não pode usar DN e âncora, a menos que **DN é Âncora** seja selecionado na página Conectividade.
- Se **DN é Âncora** estiver selecionado na página Conectividade, esta página só solicitará o atributo DN. Esse atributo será usado também como o atributo de âncora. ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### Esquema 4 (Definir tipo de atributo, referência e direção)

Esta página permite que você configure o tipo de atributo como inteiro, referência, cadeia de caracteres, binário, ou booliano e a direção para cada atributo. Todos os atributos da página **Esquema 2** estão listados como atributos de valores múltiplos.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **DataType**: usado para mapear o tipo de atributo para aqueles conhecidos do mecanismo de sincronização. O padrão é usar o mesmo tipo, conforme detectado no esquema SQL, mas DateTime e referência não são facilmente detectáveis. Para esses você precisa especificar **DateTime** ou **Referência**.
- **Direção**: você pode definir a direção do atributo para Importar, Exportar ou ImportExport. ImportExport é o padrão. ![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Observações:

- Se um tipo de atributo não for detectável pelo conector, ele usará o tipo de dados String.
- **Tabelas aninhadas** podem ser consideradas tabelas de banco de dados de uma coluna. O Oracle armazena as linhas de uma tabela aninhada sem nenhuma ordem específica. No entanto, quando você recupera a tabela aninhada em uma variável de PL/SQL, as linhas recebem subscritos consecutivos, começando em 1. Que lhe dá acesso do tipo matriz a linhas individuais.
- **VARRYS** não têm suporte no conector.

### Esquema 5 (Definir partição para atributos de referência)

Nesta página você configurará todos os atributos de referência a cuja partição, ou seja, tipo de objeto, um atributo está se referindo.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Se você usar **DN é âncora**, deverá usar o mesmo tipo de objeto do qual está fazendo referência. Não é possível referenciar outro tipo de objeto.

### Parâmetros Globais

A página Parâmetros Globais é usada para configurar importação Delta, formato de data/hora e método de senha.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

O conector SQL genérico dá suporte aos seguintes métodos de importação Delta:

- **Gatilho**: consulte [Gerar exibições de Delta usando gatilhos](https://technet.microsoft.com/library/cc708665.aspx).
- **Marca-d'água**: este é um método genérico e pode ser usado com qualquer banco de dados. A consulta de marca-d'água será preenchida com base no fornecedor do banco de dados. Uma coluna de marca-d'água deve estar presente em cada tabela/exibição usada. Isso deve controlar inserções e atualizações para as tabelas, bem como suas tabelas dependentes (de valores múltiplos ou filho). Os relógios entre o serviço de sincronização e o servidor de banco de dados devem ser sincronizados. Caso contrário, algumas entradas na importação delta podem ser omitidas. Limitação:
    - A estratégia de marca-d'água não dá suporte a exclusão de objetos.
- **Instantâneo** (funciona somente com o Microsoft SQL Server) [Gerando exibições de Delta usando instantâneos](https://technet.microsoft.com/library/cc720640.aspx)
- **Acompanhamento de alterações** (só funciona com o Microsoft SQL Server) [Sobre acompanhamento de alterações](https://msdn.microsoft.com/library/bb933875.aspx) Limitações:
    - Atributo de âncora e DN devem ser parte da chave primária para o objeto selecionado na tabela.
    - Consulta SQL não tem suporte durante importação e exportação com acompanhamento de alterações.

**Parâmetros adicionais**: especifique o fuso horário do servidor de banco de dados que indica o local em que o servidor de banco de dados está localizado. Esse valor é usado para oferecer suporte ao vários formatos de atributos de data e hora.

O conector sempre armazenará data e data e hora no formato UTC. Para converter corretamente datas e horas, o fuso horário do servidor de banco de dados e o formato usado devem ser especificados. O formato deve ser expresso no formato .Net.

Durante a exportação, todos os atributos de data e hora devem ser fornecidos ao conector no formato de hora UTC.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Configuração de senha**: o conector fornece recursos de sincronização de senha e dá suporte a definição e alteração de senha.

O conector fornece dois métodos para dar suporte à sincronização de senha:

- **Procedimento armazenado**: esse método requer dois procedimentos armazenados para dar suporte a definição e alteração de senha. Digite todos os parâmetros para adicionar e alterar a operação de senha em Definir procedimento armazenado de senha e Alterar parâmetros de procedimento armazenado de senha como nos exemplos abaixo. ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Extensão de senha**: este método requer DLL de extensão de senha (você precisa fornecer o nome DLL de extensão que está implementando a interface [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx)). O assembly de extensão de senha deve ser colocado na pasta de extensão para que o conector possa carregar a DLL no tempo de execução. ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Você também deve habilitar o Gerenciamento de senhas na página **Configurar Extensão**. ![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### Configurar partições e hierarquias

Na página de partições e hierarquias, selecione todos os tipos de objeto. Cada tipo de objeto está em sua própria partição.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Você também pode substituir os valores definidos na página **Conectividade** ou **Parâmetros Globais**.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### Configurar Âncoras

Esta página é somente leitura, pois a âncora já foi definida. O atributo de âncora selecionado sempre é acrescentado com o tipo de objeto para garantir que ele permanecerá exclusivo em todos os tipos de objeto.

![ancoras](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## Configurar parâmetros da etapa de execução

Essas etapas são configuradas em perfis de execução do conector. Essas configurações farão o trabalho real de importar e exportar dados.

### Importação completa e Delta

O conector SQL genérico oferece suporte a importação completa e Delta usando estes métodos genérico:

- Tabela
- Visualizar
- Procedimento armazenado
- Consulta SQL

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabela/Exibição**

Para importar atributos de valores múltiplos de um objeto, você precisa fornecer o nome de tabela/exibição separados por vírgulas em **Nome de tabela/exibições de valores múltiplos** e as respectivas condições de junção em **Condição de junção** com a tabela principal.

Exemplo: você deseja importar o objeto de funcionário e todos os seus atributos de valores múltiplos. Há duas tabelas chamadas Funcionário (tabela principal) e Departamento (valores múltiplos). Faça o seguinte:

- Digite **Funcionário** em **Tabela/Exibição/SP**.
- Digite Departamento em **Nome da tabela/exibições de valores múltiplos**.
- Digite a condição de junção entre Funcionário e Departamento em **Condição de Junção**, por exemplo, `Employee.DEPTID=Department.DepartmentID`. ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Procedimentos armazenados**

![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Se você tiver muitos dados, é recomendável implementar a paginação com os procedimentos armazenados.
- Para o procedimento armazenado oferecer suporte à paginação, forneça o Índice inicial e o final. Consulte: [Paginação eficiente em grandes quantidades de dados](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndex e @EndIndex serão substituídos em tempo de execução pelo respectivo valor do tamanho de página configurado na página **Configurar Etapa**. Exemplo: se o conector recuperar a primeira página e o tamanho da página for definido como 500, em tal situação @StartIndex seria 1 e @EndIndex considerado como 500, e esse valor aumenta conforme o conector recupera páginas subsequentes e altera o valor de @StartIndex e @EndIndex.
- Para executar o procedimento armazenado com parâmetros, forneça os parâmetros no formato `[Name]:[Direction]:[Value]`. Forneça cada parâmetro em uma linha separada (use Ctrl + Enter para obter uma nova linha).
- O conector SQL genérico também dá suporte a operação de importação de um ambiente distribuído, por exemplo, servidores vinculados no Microsoft SQL Server. Caso seja necessário recuperar informações de uma tabela no servidor vinculado, a tabela deve ser fornecida no formato: `[ServerName].[Database].[Schema].[TableName]`
    - Em ambientes distribuídos, o conector só dá suporte ao servidor vinculado da Microsoft.
- O conector SQL genérico só dá suporte a objetos com estrutura semelhante (nome de alias e tipo de dados) entre informações de etapas de execução e detecção de esquema. Se o objeto selecionado do esquema e as informações fornecidas na etapa de execução forem diferentes, o conector do SQL será não poderá dar suporte a esse tipo de cenário.

**Consulta SQL**

![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Consultas com vários conjunto de resultados não têm suporte.
- A consulta SQL dá suporte a paginação e fornece o índice inicial e final como uma variável para dar suporte à paginação.

### Importação de delta

![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Configuração de importação Delta requer outras configurações, juntamente com outro método com suporte, como na Importação completa.

- Se os usuários escolherem a abordagem de instantâneo ou gatilho para acompanhar as alterações delta, ele poderá fornecer banco de dados de uma tabela de histórico ou instantâneo na caixa **Nome de banco de dados de Instantâneo ou tabela de Histórico**.
- O usuário também precisa fornecer a condição de junção entre a tabela de histórico e tabela principal. Exemplo: `Employee.ID=History.EmployeeID`
- Para controlar a transação na tabela principal da tabela de histórico, o usuário deve fornecer o nome da coluna que contém as informações da operação como (Adicionar/Atualizar/Excluir).
- Se o usuário escolher marca-d'água para acompanhar as alterações delta, deverá fornecer o nome da coluna que contém as informações de operação em **Nome da Coluna de Marca D'água**. Portanto, conector pode considerar essa coluna ao executar a importação Delta.
- A coluna **alterar atributo de tipo** é necessária para o tipo de alteração. Essa coluna mapeia uma alteração que ocorre na tabela primária ou tabela de valores múltiplos para um tipo de alteração na exibição de delta. Esta coluna pode conter o tipo de alteração Modify\_Attribute para a alteração de nível de atributo ou um tipo de alteração Adicionar, Modificar ou Excluir, para um tipo de alteração de nível de objeto. Se for algo diferente do valor padrão de Adicionar, Modificar ou Excluir, o usuário poderá definir esses valores usando essa opção.

### Exportação

![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

O conector SQL genérico oferece suporte à exportação usando quatro métodos:

- Tabela
- Visualizar
- Procedimento armazenado
- Consulta SQL

**Tabela/Exibição**

Se os usuários escolherem a opção de Tabela/Exibição, o conector gerará as respectivas consultas e executar a exportação.

**Procedimentos armazenados**

![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Se os usuários escolherem a opção Procedimento Armazenado, a exportação exigirá 3 procedimentos armazenados diferentes para executar várias operações Insert/Update/Delete.

- **Adicionar nome do SP**. Esse SP será executado se houver objetos para inserção no conector, na respectiva tabela.
- **Atualizar nome do SP**. Esse SP será executado se houver objetos para atualização no conector, na respectiva tabela.
- **Excluir nome do SP**. Esse SP será executado se houver objetos para exclusão no conector, na respectiva tabela.
- Atributo selecionado do esquema usado como um valor de parâmetro para o procedimento armazenado. Exemplo: EmployeeName: INPUT: @EmployeeName (EmployeeName é selecionado no esquema do conector e o conector substitui o valor respectivo ao executar a exportação)
- Para executar o procedimento armazenado com parâmetros, digite todos os parâmetros no formato `[Name]:[Direction]:[Value]`. Forneça cada parâmetro em uma linha separada (use Ctrl + Enter para obter uma nova linha).

**Consulta SQL**

![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Se os usuários escolherem a opção de consulta SQL, a exportação exigirá 3 procedimentos armazenados diferentes para executar várias operações Insert/Update/Delete.

- **Consulta Insert**. Essa consulta será executada se houver objetos para inserção no conector, na respectiva tabela.
- **Consulta Update**: esta consulta será executada se houver objetos para atualização no conector, na respectiva tabela.
- **Consulta Delete**: esta consulta será executada se houver objetos para atualização no conector, na respectiva tabela.
- Atributo selecionado do esquema, usado como um valor de parâmetro na consulta. Exemplo: `Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## Solucionar problemas

-	Para saber mais sobre como habilitar o registro em log para solucionar problemas do conector, confira [How to Enable ETW Tracing for Connectors](http://go.microsoft.com/fwlink/?LinkId=335731).

<!---HONumber=AcomDC_0525_2016-->