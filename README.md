# Projeto Final — Engenharia de Dados

## Pipeline de Engenharia de Dados para IA aplicada a PMEs

Este repositório apresenta o projeto final da disciplina de Engenharia de Dados da pós-graduação, com foco no desenho de um pipeline de dados para pequenas e médias empresas que desejam utilizar Inteligência Artificial em processos comerciais, operacionais e de relacionamento com clientes.

O projeto propõe uma arquitetura baseada no padrão Medallion, organizada em três camadas principais: Bronze, Silver e Gold. O objetivo é coletar dados de múltiplas fontes, armazená-los de forma rastreável, aplicar processos de limpeza e padronização, e disponibilizar conjuntos de dados prontos para algoritmos de IA.

## Objetivo do projeto

Descrever uma arquitetura de Engenharia de Dados capaz de preparar dados de PMEs para uso em modelos de Inteligência Artificial, como previsão de demanda, análise de churn, segmentação de clientes e recomendação de ações comerciais.

## Fontes de dados propostas

O pipeline considera dados provenientes de diferentes fontes:

- Sistemas ERP e bancos relacionais;
- APIs de CRM e atendimento;
- Conversas de WhatsApp e canais digitais;
- Planilhas comerciais em CSV ou Excel;
- Logs e eventos operacionais;
- APIs externas, como feriados, clima e indicadores econômicos.

## Arquitetura proposta

A arquitetura utiliza ferramentas open source e conceitos abordados na disciplina:

- Docker e Docker Compose para conteinerização;
- MinIO como Data Lake local compatível com S3;
- Airbyte para ingestão visual de dados;
- Python e dlt para ingestões programáticas;
- Parquet como formato de armazenamento analítico;
- dbt, DuckDB ou Spark para transformação dos dados;
- Airflow para orquestração dos pipelines;
- Great Expectations ou testes dbt para qualidade de dados;
- GitHub para versionamento e documentação.

## Camadas do pipeline

### Bronze

Armazena os dados brutos vindos das fontes originais, preservando sua estrutura inicial e adicionando apenas metadados de rastreabilidade, como fonte, data de ingestão e versão do schema.

### Silver

Realiza limpeza, padronização, deduplicação, integração e validação dos dados, tornando-os consistentes e confiáveis para uso analítico.

### Gold

Disponibiliza datasets curados e modelados para Analytics e Machine Learning, incluindo bases para previsão de demanda, churn de clientes, segmentação e recomendação comercial.

## Uso dos dados em IA

Os dados preparados na camada Gold serão utilizados em algoritmos de IA, como:

- Prophet, XGBoost, LightGBM e Random Forest para previsão de demanda;
- Regressão Logística, Random Forest e XGBoost para previsão de churn;
- K-Means para segmentação de clientes;
- Modelos de NLP para classificação de mensagens e análise de atendimento;
- Modelos de recomendação para sugestão de ações comerciais.

## Entregáveis

- Documento PDF com a descrição completa do projeto;
- Diagrama da arquitetura proposta;
- README explicativo no GitHub;
- Referências utilizadas.
