
# Pipeline ETL com Airflow e Meltano

Este repositório contém uma pipeline ETL (Extract, Transform, Load) utilizando **Airflow** e **Meltano**. O objetivo dessa pipeline é consumir dados de duas fontes distintas (um banco de dados PostgreSQL e um arquivo CSV), gerar um novo arquivo CSV e depois carregar esse arquivo CSV em um banco de dados PostgreSQL.

## Estrutura da Pipeline

A pipeline é dividida em duas **DAGs** no **Airflow**:

1. **DAG 1 (`dag_1_extract_and_generate_csv`)**:
   - Extrai dados do banco de dados **PostgreSQL** e do arquivo **CSV**.
   - Gera um novo arquivo CSV contendo os dados extraídos e transformados.
   
2. **DAG 2 (`dag_2_load_to_postgres`)**:
   - Carrega o novo arquivo CSV gerado na **DAG 1** para o banco de dados **PostgreSQL**.

## Tecnologias Usadas

- **Airflow**: Orquestrador de workflows para gerenciar a execução das DAGs.
- **Meltano**: Framework de ELT (Extract, Load, Transform) para orquestrar a extração e carga de dados.
- **PostgreSQL**: Banco de dados utilizado para armazenar os dados.
- **CSV**: Formato de arquivo utilizado para transportar dados entre as etapas do processo.
- **Python**: Linguagem de programação usada para definir as DAGs e as tarefas.


## Pré-requisitos

Antes de rodar o código, você precisa configurar algumas variáveis e dependências:

1. **Airflow**: Instalar e configurar o **Apache Airflow**. Caso não tenha, siga as instruções da [documentação do Airflow](https://airflow.apache.org/docs/apache-airflow/stable/installation/index.html).

2. **Meltano**: Instalar o **Meltano** para integração dos conectores de extração e carregamento de dados. Siga as instruções do [Meltano](https://www.meltano.com/docs/installation/).

3. **Conectores Meltano**:
   - **tap-postgres**: Conector de extração para o banco de dados PostgreSQL.
   - **target-csv**: Conector de destino para salvar dados extraídos em formato CSV.
   - **tap-csv**: Conector de extração para o arquivo CSV gerado.
   - **target-postgres**: Conector de destino para carregar dados no PostgreSQL.

4. **PostgreSQL**:
   - Você precisa de uma instância de banco de dados **PostgreSQL** configurada e com as credenciais de acesso.

5. **Arquivo CSV**:
   - O arquivo CSV a ser usado na **DAG 1** pode ser encontrado em: [Arquivo CSV](https://github.com/anajuliq/code-challenge/blob/main/data%2Forder_details.csv).

## Configurações Necessárias

1. **Configuração do Airflow**:
   - Certifique-se de que o Airflow esteja configurado corretamente no seu ambiente.
   - Adicione suas credenciais de conexão do PostgreSQL no **Airflow Connection UI** ou diretamente no arquivo de configurações de conexões do Airflow (`airflow.cfg`).

2. **Configuração do Meltano**:
   - O Meltano deve estar configurado no seu ambiente, com os conectores adequados instalados.
   - Execute os seguintes comandos para configurar os conectores:
     ```bash
     meltano add extractor tap-postgres
     meltano add target target-csv
     meltano add extractor tap-csv
     meltano add target target-postgres
     ```

3. **Credenciais do PostgreSQL**:
   - Substitua o link de conexão do PostgreSQL no código, alterando `<username>`, `<password>`, `<host>`, `<port>` e `<database_name>` com as credenciais reais do seu banco de dados.
     ```python
     "postgresql://<username>:<password>@<host>:<port>/<database_name>"
     ```

4. **Caminho do CSV gerado**:
   - Defina o caminho correto para o arquivo CSV gerado pela **DAG 1**. Ele será utilizado na **DAG 2** para carregar os dados no PostgreSQL.
     ```python
     "path/to/generated_file.csv"
     ```

## Como Rodar

1. **Inicie o Airflow**:
   - Inicie o servidor web e o scheduler do Airflow:
     ```bash
     airflow webserver -p 8080
     airflow scheduler
     ```

2. **Execute a Pipeline**:
   - Uma vez que o Airflow estiver em execução, você pode iniciar a execução das DAGs manualmente ou aguardar a execução automática diária.
   - Acesse a interface web do Airflow em `http://localhost:8080` para monitorar o andamento das DAGs.

3. **Verifique os Logs**:
   - Caso haja algum erro, os logs podem ser acessados através da interface do Airflow para depuração.
