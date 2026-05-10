# Documentação da Geração do Dataset Sintético

## Classificação do Nível de Desperdício Ambiental em Racks de Datacenter

---

## 1. Objetivo do Dataset

O objetivo deste dataset é representar, de forma sintética e controlada, situações operacionais de racks de datacenter para permitir a aplicação de técnicas de aprendizado de máquina na tarefa de **classificação do nível de desperdício ambiental**.

Cada instância do dataset representa o comportamento de **um rack de datacenter em uma hora de operação**.

A classe-alvo do dataset é o nível de desperdício ambiental, dividido em três categorias:

- **baixo**
- **moderado**
- **alto**

O dataset será gerado com apoio de Modelos de Linguagem de Grande Escala (LLMs), seguindo um processo documentado, rastreável e baseado em fundamentação obtida por meio dos Mapeamentos Sistemáticos da Literatura.

---

## 2. Requisitos do Trabalho

Segundo as especificações do trabalho, o dataset deverá atender aos seguintes requisitos mínimos:

| Requisito | Como será atendido |
|---|---|
| Pelo menos 500 instâncias | O dataset será gerado com quantidade igual ou superior a 500 registros. |
| Pelo menos 5 atributos | O dataset conterá atributos energéticos, térmicos, computacionais, operacionais e ambientais. |
| Pelo menos 1 atributo irrelevante | Será incluído um atributo sem relação direta com a classe-alvo. |
| Presença de valores faltantes | Serão inseridos valores ausentes de forma intencional e documentada. |
| Presença de ruído | Serão inseridas pequenas variações ou inconsistências controladas. |
| Presença de outliers | Serão criadas instâncias discrepantes em relação à distribuição principal. |
| Uso de LLM | O dataset será gerado com apoio de Modelo de Linguagem de Grande Escala. |
| Prompts documentados | Todos os prompts utilizados serão registrados e justificados. |
| Rastreabilidade | O processo de geração será documentado do planejamento até o dataset final. |
| Figura do pipeline | Será criada uma imagem representando o fluxo de geração dos dados. |
| Formato compatível com Weka | O dataset final será salvo em `.arff`. |

---

## 5. Estrutura Geral do Dataset

### 5.1. Unidade de Análise

Cada linha do dataset representa:

> Um rack de datacenter em uma hora de operação.

Essa unidade permite relacionar variáveis de consumo, refrigeração, carga computacional e impacto ambiental em uma janela temporal simples e interpretável.

---

### 5.2. Classe-Alvo

A variável-alvo será:

```bash
nivel_desperdicio
```

Com os seguintes valores:

| Classe       | Descrição                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------- |
| **baixo**    | Operação proporcional entre uso computacional, consumo energético e refrigeração.           |
| **moderado** | Presença de sinais intermediários de ineficiência ou desperdício.                           |
| **alto**     | Consumo, refrigeração ou impacto ambiental desproporcional em relação à utilização do rack. |

---

## 8. Pipeline Metodológico de Geração do Dataset

O pipeline de geração será baseado nas práticas encontradas no MSL (Eixo 3 — Geração de Datasets Sintéticos com LLM), especialmente em estudos sobre geração de datasets sintéticos com LLMs, dados tabulares, validação, controle de qualidade e documentação.

O processo será organizado nas seguintes etapas.

---

### Etapa 1 — Definição do Esquema do Dataset

**Objetivo:** definir a estrutura do dataset antes da geração.

**O que será feito:**

* definir a unidade de análise;
* definir a classe-alvo;
* definir os atributos;
* definir o tipo de cada atributo;
* definir faixas plausíveis;
* definir categorias válidas;
* definir relações esperadas entre atributos.

**Artefato gerado:**

* esta documentação de geração do dataset.

**Justificativa:**

A literatura sobre geração sintética com LLM indica que a geração tende a ser mais consistente quando o modelo recebe um esquema claro, com atributos, tipos, limites e regras de domínio. Isso reduz o risco de saídas incoerentes, categorias inesperadas ou valores fora de escopo.

---

### Etapa 2 — Definição de Regras Semânticas

**Objetivo:** estabelecer regras de plausibilidade entre os atributos.

**O que será feito:**

* definir relações esperadas entre consumo, carga computacional, refrigeração, temperatura e impacto ambiental;
* definir condições aproximadas para cada classe;
* definir situações que caracterizam desperdício baixo, moderado e alto;
* definir limites para evitar registros fisicamente ou operacionalmente incoerentes.

**Artefato gerado:**

* seção de regras semânticas neste documento.

**Justificativa:**

A geração com LLM não deve ser livre ou aleatória. As regras semânticas funcionam como restrições de domínio que orientam o modelo a gerar dados plausíveis para o contexto de racks de datacenter.

---

### Etapa 3 — Construção dos Prompts

**Objetivo:** criar prompts estruturados para orientar a geração dos dados.

**O que será feito:**

* criar prompts com instruções explícitas;
* informar os atributos e seus tipos;
* informar as faixas permitidas;
* informar as regras semânticas;
* informar o formato esperado da saída;
* solicitar saída em formato tabular, preferencialmente CSV;
* versionar os prompts utilizados.

**Artefatos gerados:**

* `prompts/prompts_utilizados.txt`;
* versões dos prompts, se necessário.

**Justificativa:**

A literatura analisada no MSL 3 aponta que prompts estruturados, com esquema, regras e formato de saída explícito, aumentam a consistência da geração e reduzem erros de formatação.

---

### Etapa 4 — Geração de Amostra Inicial

**Objetivo:** testar se o LLM compreende a estrutura esperada antes da geração completa.

**O que será feito:**

* gerar uma pequena amostra de dados;
* verificar se os atributos aparecem corretamente;
* verificar se as classes estão corretas;
* verificar se os valores respeitam as faixas definidas;
* verificar se o formato tabular foi obedecido;
* ajustar o prompt, se necessário.

**Artefato gerado:**

* primeira amostra sintética;
* registro dos ajustes feitos nos prompts.

**Justificativa:**

A geração de uma amostra inicial reduz o risco de gerar um dataset completo com erros estruturais ou semânticos. Essa etapa permite corrigir problemas antes da geração final.

---

### Etapa 5 — Geração Completa do Dataset

**Objetivo:** gerar o dataset sintético completo.

**O que será feito:**

* gerar pelo menos 500 instâncias;
* preferencialmente gerar uma margem acima do mínimo;
* garantir presença das três classes;
* gerar registros por classe ou por cenário;
* consolidar os lotes em um único arquivo.

**Estratégia sugerida:**

| Classe   | Quantidade aproximada |
| -------- | --------------------: |
| baixo    |             preencher |
| moderado |             preencher |
| alto     |             preencher |

**Artefato gerado:**

* `dataset_original.arff`.

**Justificativa:**

A geração por classe ou cenário ajuda a controlar a distribuição da classe-alvo e evita que o LLM produza um dataset desbalanceado ou pouco representativo.

---

### Etapa 6 — Inserção Planejada de Valores Faltantes, Ruído e Outliers

**Objetivo:** atender aos requisitos do trabalho e simular imperfeições presentes em dados operacionais.

**O que será feito:**

* inserir valores faltantes de forma intencional;
* inserir ruído controlado;
* inserir outliers;
* manter registro da estratégia usada;
* garantir que esses elementos permaneçam no dataset original.

**Artefato gerado:**

* seção de registro de faltantes, ruído e outliers neste documento.

**Justificativa:**

O enunciado do trabalho exige que o dataset contenha valores faltantes, ruído e outliers. Esses elementos devem ser planejados e documentados, não inseridos aleatoriamente sem explicação.

---

### Etapa 7 — Validação Estrutural e Semântica da Geração

**Objetivo:** verificar se o dataset gerado atende aos requisitos mínimos e se está coerente com o esquema definido.

**O que será feito:**

* verificar número de instâncias;
* verificar número de atributos;
* verificar presença da classe-alvo;
* verificar presença do atributo irrelevante;
* verificar presença de valores faltantes;
* verificar presença de ruído;
* verificar presença de outliers;
* verificar se as classes são apenas `baixo`, `moderado` e `alto`;
* verificar se há linhas quebradas;
* verificar se há valores incompatíveis com o tipo do atributo;
* verificar se as principais regras semânticas foram respeitadas.

**Importante:**

Esta etapa **não corresponde ao pré-processamento formal**. Valores faltantes, ruído e outliers planejados não devem ser removidos nesta fase, pois fazem parte do dataset original.

**Artefatos gerados:**

* `dataset_original.arff`.

---

### Etapa 8 — Documentação e Rastreabilidade

**Objetivo:** registrar o processo de geração do dataset.

**O que será documentado:**

* prompts utilizados;
* versões dos prompts;
* mudanças feitas entre versões;
* modelo LLM utilizado;
* data da geração;
* critérios de geração;
* regras semânticas;
* quantidade de instâncias geradas;
* registros descartados, se houver;
* presença de valores faltantes, ruído e outliers;
* limitações do dataset.

**Artefatos gerados:**

* `prompts/prompts_utilizados.txt`;
* `dataset/documentacao_geracao_dataset.md`;
* `dataset/dataset_original.arff`;
* `imagens/pipeline_geracao.png`.

---

## 9. Regras Semânticas do Dataset


As regras abaixo serão usadas para orientar a geração sintética e validar a plausibilidade dos registros.

| Código | Regra     | Justificativa |
| ------ | --------- | ------------- |
| R1     | preencher | preencher     |
| R2     | preencher | preencher     |
| R3     | preencher | preencher     |
| R4     | preencher | preencher     |
| R5     | preencher | preencher     |

Exemplos de regras esperadas:

* consumo energético não deve ser negativo;
* utilização de CPU deve estar entre 0 e 100%;
* temperatura deve estar em faixa operacional plausível;
* alto consumo com baixa utilização pode indicar desperdício;
* alta refrigeração com baixa carga pode indicar desperdício;
* alto consumo com alta carga pode ser operação justificável;
* emissão de carbono deve acompanhar o consumo energético;
* uso de água deve estar associado à refrigeração, quando aplicável.

---

## 10. Estratégia para Valores Faltantes

Valores faltantes serão inseridos intencionalmente para simular falhas de sensores, ausência de telemetria ou indisponibilidade parcial de dados.

| Atributo  | Estratégia | Justificativa |
| --------- | ---------- | ------------- |
| preencher | preencher  | preencher     |
| preencher | preencher  | preencher     |

Os valores faltantes serão representados por:

```text
?
```

Essa representação é compatível com o formato ARFF utilizado pelo Weka.

---

## 11. Estratégia para Ruído

O ruído será inserido de forma controlada para simular variações de medição, imprecisão de sensores ou pequenas inconsistências operacionais.

| Tipo de ruído | Atributo afetado | Justificativa |
| ------------- | ---------------- | ------------- |
| preencher     | preencher        | preencher     |
| preencher     | preencher        | preencher     |

O ruído não deve destruir a coerência geral do registro. Ele deve representar variações plausíveis e não transformar todo o dataset em dados aleatórios.

---

## 12. Estratégia para Outliers

Os outliers serão inseridos intencionalmente para representar instâncias distantes da distribuição principal ou com comportamento discrepante em relação aos demais dados.

| Tipo de outlier | Descrição | Justificativa |
| --------------- | --------- | ------------- |
| preencher       | preencher | preencher     |
| preencher       | preencher | preencher     |

Exemplos possíveis:

* consumo muito alto com baixa utilização;
* temperatura muito alta em relação ao padrão;
* refrigeração muito alta com baixa carga computacional;
* emissão de carbono elevada em cenário de baixa demanda;
* combinação extrema de consumo, temperatura e refrigeração.

Os outliers devem permanecer no dataset original para análise posterior no pré-processamento.

---

## 13. Critérios de Validação da Geração

Antes de considerar o dataset original concluído, serão verificados os seguintes critérios:

| Critério                                   | Resultado |
| ------------------------------------------ | --------- |
| O dataset possui pelo menos 500 instâncias | preencher |
| O dataset possui pelo menos 5 atributos    | preencher |
| O dataset possui atributo irrelevante      | preencher |
| O dataset possui valores faltantes         | preencher |
| O dataset possui ruído                     | preencher |
| O dataset possui outliers                  | preencher |
| O dataset possui classe-alvo               | preencher |
| As classes são baixo, moderado e alto      | preencher |
| O dataset está em CSV                      | preencher |
| O dataset foi convertido para ARFF         | preencher |
| O arquivo ARFF abre no Weka                | preencher |

---

## 14. Artefatos Finais da Etapa de Geração

Ao final da geração, devem existir os seguintes artefatos:

| Artefato                                  | Finalidade                                      |
| ----------------------------------------- | ----------------------------------------------- |
| `dataset/documentacao_geracao_dataset.md` | Documento central do processo de geração        |
| `dataset/dataset_original.csv`            | Dataset original em CSV                         |
| `dataset/dataset_original.arff`           | Dataset original em formato compatível com Weka |
| `prompts/prompts_utilizados.txt`          | Registro dos prompts usados                     |
| `imagens/pipeline_geracao.png`            | Figura do fluxo de geração                      |

