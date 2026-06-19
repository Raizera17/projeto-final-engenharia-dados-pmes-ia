# Projeto Final — Engenharia de Dados

## Pipeline de Engenharia de Dados para IA aplicada a PMEs

Este repositório apresenta o projeto final da disciplina **Engenharia de Dados**, desenvolvido como parte da pós-graduação, com foco no desenho de um pipeline de dados para pequenas e médias empresas que desejam utilizar Inteligência Artificial em processos comerciais, operacionais e de relacionamento com clientes.

O projeto propõe uma arquitetura baseada no padrão **Medallion**, organizada em três camadas principais: **Bronze**, **Silver** e **Gold**. O objetivo é coletar dados de múltiplas fontes, armazená-los de forma rastreável, aplicar processos de limpeza e padronização, e disponibilizar conjuntos de dados prontos para uso em algoritmos de IA.

## Integrantes

| Nome                            | RA         |
| ------------------------------- | ---------- |
| Raí van der Meer Oliveira Neves | 20.01188-0 |
| Rafael de Almeida Valente       | 20.01293-4 |

## Objetivo do projeto

Descrever uma arquitetura de Engenharia de Dados capaz de preparar dados de PMEs para uso em modelos de Inteligência Artificial, como:

* previsão de demanda;
* análise de churn de clientes;
* segmentação de clientes;
* classificação de atendimentos;
* recomendação de ações comerciais.

O projeto não tem como objetivo implementar o pipeline em produção, mas sim apresentar suas funcionalidades, camadas, tipos de dados, tecnologias propostas e boas práticas de Engenharia de Dados.

## Problema de negócio

Pequenas e médias empresas costumam possuir dados distribuídos em diferentes sistemas, como ERPs, CRMs, planilhas, canais de atendimento e ferramentas comerciais. Esses dados geralmente estão despadronizados, duplicados, incompletos ou armazenados em formatos diferentes.

Essa realidade dificulta o uso de Inteligência Artificial, pois modelos de IA dependem de dados consistentes, organizados, históricos e confiáveis.

O pipeline proposto busca resolver esse problema por meio de uma arquitetura capaz de transformar dados brutos e dispersos em bases preparadas para análise, automação e treinamento de modelos de IA.

## Fontes de dados propostas

O pipeline considera dados provenientes de diferentes fontes:

| Fonte                       | Tipo de dado                       | Exemplos                                              |
| --------------------------- | ---------------------------------- | ----------------------------------------------------- |
| ERP / sistema de vendas     | Estruturado                        | pedidos, clientes, produtos, estoque, notas fiscais   |
| CRM / funil comercial       | Estruturado ou semiestruturado     | leads, oportunidades, status, origem do cliente       |
| WhatsApp / atendimento      | Semiestruturado ou não estruturado | mensagens, dúvidas, reclamações, histórico de contato |
| Planilhas comerciais        | Estruturado                        | metas, campanhas, tabelas de preço, vendedores        |
| Logs e eventos operacionais | Semiestruturado / streaming        | movimentações de estoque, cliques, interações         |
| APIs externas               | Estruturado ou semiestruturado     | feriados, clima, indicadores econômicos               |

A multiplicidade de fontes permite uma visão mais completa do negócio e contribui para a criação de variáveis relevantes para os modelos de IA.

## Arquitetura proposta

A arquitetura utiliza ferramentas open source e conceitos trabalhados na disciplina:

* **Docker e Docker Compose** para conteinerização dos serviços;
* **MinIO** como Data Lake local compatível com S3;
* **Airbyte** para ingestão visual de dados;
* **Python + dlt** para ingestões programáticas;
* **Apache Parquet** com compressão Snappy como formato de armazenamento analítico;
* **dbt, DuckDB ou Apache Spark** para transformação dos dados;
* **Apache Airflow** para orquestração dos pipelines;
* **Great Expectations ou testes dbt** para qualidade de dados;
* **GitHub** para versionamento e documentação do projeto.

## Visão geral do fluxo

```text
Fontes de Dados
ERP | CRM | WhatsApp | Planilhas | Logs | APIs Externas
        ↓
Ingestão de Dados
Airbyte | Python + dlt | Kafka
        ↓
Camada Bronze
Dados brutos preservados no MinIO/S3 em Parquet
        ↓
Camada Silver
Dados limpos, padronizados, validados e integrados
        ↓
Camada Gold
Datasets curados para Analytics e IA
        ↓
Consumo
Modelos de IA | Dashboards | Relatórios | Decisão de negócio
```

## Camadas do pipeline

### Camada Bronze

A camada Bronze armazena os dados brutos vindos das fontes originais, preservando sua estrutura inicial. O objetivo dessa camada é manter a rastreabilidade e garantir que os dados possam ser reprocessados futuramente, caso necessário.

Nesta camada, os dados recebem apenas metadados técnicos, como:

* fonte de origem;
* data e hora de ingestão;
* versão do schema;
* identificador do lote;
* caminho do arquivo original.

Exemplos de datasets na camada Bronze:

```text
bronze_erp_pedidos
bronze_erp_clientes
bronze_crm_leads
bronze_whatsapp_mensagens
bronze_planilhas_metas
bronze_api_feriados
```

### Camada Silver

A camada Silver é responsável pela limpeza, padronização, integração e validação dos dados. Nela, os dados deixam de ser apenas registros brutos e passam a ter consistência para uso analítico.

Principais tratamentos aplicados:

* padronização de datas;
* padronização de moedas e valores numéricos;
* tratamento de campos nulos;
* remoção de duplicidades;
* validação de CPF, CNPJ, e-mail e telefone;
* padronização de categorias de produtos;
* integração entre clientes, pedidos, produtos, estoque e atendimentos;
* anonimização ou mascaramento de dados sensíveis.

Exemplos de datasets na camada Silver:

```text
silver_clientes
silver_produtos
silver_pedidos
silver_itens_pedido
silver_estoque
silver_atendimentos
silver_leads
```

### Camada Gold

A camada Gold disponibiliza dados curados e modelados para uso em Analytics, BI e Machine Learning. Nessa camada, os dados são organizados conforme os objetivos de negócio e os requisitos dos algoritmos de IA.

Exemplos de datasets na camada Gold:

```text
gold_clientes_ia
gold_vendas_diarias
gold_estoque_produto
gold_interacoes_cliente
gold_features_churn
gold_features_previsao_demanda
gold_features_recomendacao_comercial
```

## Uso dos dados em algoritmos de IA

Os dados preparados na camada Gold serão utilizados em diferentes modelos de Inteligência Artificial.

| Objetivo                     | Algoritmos possíveis                        | Dados necessários                                                     |
| ---------------------------- | ------------------------------------------- | --------------------------------------------------------------------- |
| Previsão de demanda          | Prophet, XGBoost, LightGBM, Random Forest   | histórico de vendas, sazonalidade, estoque, campanhas, feriados       |
| Churn de clientes            | Regressão Logística, Random Forest, XGBoost | recência, frequência, ticket médio, reclamações, histórico de compras |
| Segmentação de clientes      | K-Means, DBSCAN                             | comportamento de compra, valor gasto, frequência, perfil              |
| Classificação de atendimento | NLP, modelos de classificação               | mensagens, intenção, sentimento, categoria do atendimento             |
| Recomendação comercial       | Regras de associação, modelos de ranking    | histórico de compra, produtos relacionados, perfil do cliente         |

## Qualidade de dados

O pipeline proposto contempla regras de qualidade para garantir que os dados estejam adequados para análise e IA.

Exemplos de validações:

* verificação de campos obrigatórios;
* controle de duplicidade;
* validação de tipos de dados;
* consistência entre tabelas;
* checagem de valores fora do padrão;
* validação de integridade referencial;
* monitoramento de volume de dados ingeridos;
* identificação de quebras de schema.

## Governança e segurança

Como o projeto envolve dados de clientes, vendas e atendimentos, a arquitetura considera boas práticas de governança e segurança.

Medidas propostas:

* controle de acesso por camada;
* separação entre dados brutos, tratados e curados;
* mascaramento de dados sensíveis;
* rastreabilidade da origem dos dados;
* documentação dos datasets;
* versionamento de transformações;
* logs de execução dos pipelines;
* atenção à Lei Geral de Proteção de Dados (LGPD).

## Orquestração e operação

A execução do pipeline será orquestrada pelo Apache Airflow, permitindo o agendamento e monitoramento das etapas.

Fluxo operacional proposto:

```text
1. Ingestão dos dados das fontes originais
2. Armazenamento na camada Bronze
3. Validação inicial dos arquivos ingeridos
4. Transformação e padronização para a camada Silver
5. Criação das bases analíticas na camada Gold
6. Execução de testes de qualidade
7. Disponibilização dos datasets para IA e Analytics
```

## Estrutura do repositório

```text
projeto-final-engenharia-dados-pmes-ia/
│
├── README.md
├── Projeto_Final_Engenharia_de_Dados_Pipeline_IA_PMEs.docx
├── Projeto_Final_Engenharia_de_Dados_Pipeline_IA_PMEs.pdf
├── arquitetura_pipeline.png
│
├── docs/
│   ├── resumo_projeto.md
│   └── referencias.md
│
└── assets/
    └── arquitetura_pipeline.png
```

## Arquivos principais

* [Documento Word do projeto](./Projeto_Final_Engenharia_de_Dados_Pipeline_IA_PMEs.docx)
* [Documento PDF do projeto](./Projeto_Final_Engenharia_de_Dados_Pipeline_IA_PMEs.pdf)
* [Diagrama da arquitetura](./arquitetura_pipeline.png)

## Entregáveis

Este repositório contém os seguintes entregáveis:

* documento completo do projeto em Word;
* versão em PDF do documento;
* diagrama da arquitetura proposta;
* README explicativo;
* referências utilizadas.

## Referências

As referências utilizadas no projeto incluem os materiais da disciplina de Engenharia de Dados e as documentações oficiais das ferramentas propostas na arquitetura, como Airbyte, MinIO, Apache Airflow, dbt, dlt, Apache Parquet, Great Expectations e bibliotecas de Machine Learning em Python.

## Observação

Este projeto apresenta a proposta conceitual e arquitetural de um pipeline de Engenharia de Dados. A implementação prática do pipeline não é obrigatória, conforme orientação do enunciado da atividade.
