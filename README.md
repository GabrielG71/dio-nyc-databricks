# Azure Data Factory com Azure DevOps --- Pipeline End-to-End

Este projeto documenta a implementação de uma solução completa de
ingestão e transformação de dados utilizando **Azure Data Factory
(ADF)** com integração ao **Azure DevOps Repos**, aplicando
versionamento, organização por camadas e fluxo de publicação baseado em
ARM Templates.

O objetivo foi construir um projeto realista de Engenharia de Dados,
simulando um cenário corporativo com separação de ambientes, governança
básica e estrutura preparada para CI/CD.

---

## Contexto do Projeto

Durante o aprofundamento em Engenharia de Dados na Azure, explorei:

- Integração do Azure Data Factory com Azure DevOps
- Desenvolvimento em modo Git
- Branch de colaboração vs branch de publicação
- Organização de pipelines por camadas (RAW, CURATED, SERVING)
- Parametrização e boas práticas de nomenclatura
- Conceitos iniciais de promoção entre ambientes (DEV → PROD)

O projeto foi desenvolvido integralmente via **Azure Portal (ADF
Studio)** com controle de versão no **Azure DevOps Repos**.

---

## Dataset Utilizado

**NYC TLC Yellow Taxi Trip Data**

Dataset público contendo registros de corridas de táxi em Nova York, com
informações como:

- Data e hora de início e fim da corrida
- Distância percorrida
- Quantidade de passageiros
- Tipo de pagamento
- Valor da tarifa e gorjeta
- Localização de origem e destino

Motivo da escolha:

- Dataset realista e com volume relevante
- Permite aplicar validações e regras de qualidade
- Ideal para gerar agregações analíticas
- Simula um cenário comum de ingestão externa via HTTP

---

## Arquitetura da Solução

A arquitetura segue uma organização em três camadas lógicas:

\[HTTP Source\] → [RAW](#raw) → [CURATED](#curated) →
[SERVING](#serving)

### RAW

Camada responsável por armazenar os dados exatamente como recebidos da
origem.

Responsabilidades:

- Ingestão via Copy Activity
- Armazenamento em formato Parquet
- Preservação para reprocessamento futuro
- Sem regras de negócio

### CURATED

Camada de tratamento e padronização dos dados.

Transformações aplicadas:

- Conversão de tipos de dados
- Remoção de registros inconsistentes
- Tratamento de valores nulos
- Criação de colunas derivadas (ex: duração da corrida)
- Padronização de nomenclaturas

### SERVING

Camada analítica voltada para consumo por BI.

Exemplos de agregações:

- Receita total por dia
- Distância média por corrida
- Receita por tipo de pagamento
- Total de corridas por zona

---

## Componentes Utilizados no Azure Data Factory

### Linked Services

- HTTP (origem do dataset)
- Azure Data Lake Storage Gen2 (destino)

### Datasets

- Dataset RAW (Parquet)
- Dataset CURATED (Parquet)
- Dataset SERVING (Parquet)

### Pipelines

- Pipeline de ingestão (RAW)
- Pipeline de transformação (Data Flow para CURATED)
- Pipeline de agregação (SERVING)

### Triggers

- Execução manual
- Estrutura preparada para agendamento futuro

---

## Integração com Azure DevOps

O projeto foi configurado em modo Git dentro do ADF.

Estratégia adotada:

- Branch principal: main
- Branch de colaboração: collaboration
- Publicação gera ARM Template automaticamente

Fluxo de trabalho:

1.  Desenvolvimento na branch de colaboração
2.  Validação dos pipelines
3.  Publicação
4.  Geração automática de ARM Template
5.  Versionamento completo no repositório

---

## Conceitos Aplicados

- Data Warehouse vs Data Lake
- Orquestração de pipelines
- Versionamento de infraestrutura
- Governança básica de dados
- Separação de ambientes
- Parametrização de datasets
- Organização por camadas

---

## Possíveis Evoluções

- Implementação de CI/CD automatizado
- Deploy multi-ambiente (DEV / QA / PROD)
- Integração com Azure Key Vault
- Monitoramento com Azure Monitor
- Integração com Power BI
- Controle de qualidade automatizado

---

## Tecnologias

- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure DevOps Repos
- Parquet
- ARM Templates

---

## Autor

Projeto desenvolvido como parte do aprofundamento em Engenharia de Dados
na Azure por Gabriel Gonçalves.
