
# Protocolo do Mapeamento Sistemático da Literatura

## Classificação do Nível de Desperdício Ambiental em Racks de Datacenter

---

## 1. Objetivo Geral

Conduzir um mapeamento sistemático integrado da literatura para fundamentar o desenvolvimento de um dataset sintético voltado à **classificação do nível de desperdício ambiental em racks de datacenter**.

O protocolo é organizado em três eixos complementares:

1. **Estudo do problema**;
2. **Atributos e métricas para representação do desperdício ambiental**;
3. **Metodologias para geração de datasets sintéticos com LLMs**.

---

## 1.1. Objetivos Específicos

- Mapear evidências sobre impacto ambiental, desperdício de recursos, eficiência energética, refrigeração, uso de água e emissão de carbono em datacenters.
- Identificar estudos que aplicam técnicas computacionais, como Machine Learning, classificação, predição ou detecção de anomalias, em problemas relacionados a datacenters.
- Levantar atributos, métricas e indicadores utilizados para representar consumo energético, temperatura, refrigeração, utilização computacional e impacto ambiental.
- Identificar quais atributos podem fundamentar a construção de um dataset sintético para classificação do nível de desperdício ambiental em racks.
- Mapear práticas metodológicas para geração, revisão, validação, documentação e rastreabilidade de datasets sintéticos com apoio de LLMs.
- Identificar riscos e limitações associados ao uso de LLMs na geração de datasets sintéticos.

---

## 2. Organização do Protocolo

| Eixo | Foco | Função no trabalho |
|---|---|---|
| **Eixo 1 — Estudo do problema** | Desperdício ambiental em datacenters, servidores e racks | Fundamentar o problema, o domínio e a lacuna da pesquisa |
| **Eixo 2 — Atributos e métricas** | Variáveis, métricas e indicadores usados para representar desperdício ambiental | Fundamentar a escolha dos atributos do dataset |
| **Eixo 3 — Geração sintética com LLM** | Métodos para geração, validação e documentação de datasets sintéticos | Fundamentar o pipeline metodológico de geração do dataset |

---

## 3. Questões de Pesquisa

### 3.1. Eixo 1 — Estudo do Problema

| Código | Questão |
|---|---|
| **QP1.1** | Como a literatura caracteriza o desperdício ambiental e os impactos ambientais associados à operação de datacenters? |
| **QP1.2** | Quais fatores operacionais são associados à ineficiência ou desperdício ambiental em datacenters, servidores ou racks? |
| **QP1.3** | Quais métricas são utilizadas para avaliar consumo energético, refrigeração, emissão de carbono, uso de água ou eficiência operacional? |
| **QP1.4** | Quais técnicas computacionais são aplicadas para analisar, prever, classificar ou detectar ineficiências em datacenters? |
| **QP1.5** | Quais lacunas existem na literatura em relação à classificação do nível de desperdício ambiental em racks de datacenter? |

---

### 3.2. Eixo 2 — Atributos e Métricas

| Código | Questão |
|---|---|
| **QP2.1** | Quais atributos e métricas são utilizados para representar consumo energético e potência em datacenters, servidores ou racks? |
| **QP2.2** | Quais atributos são utilizados para representar refrigeração, temperatura e eficiência térmica? |
| **QP2.3** | Quais variáveis operacionais são associadas à carga de trabalho, utilização computacional, ociosidade ou subutilização? |
| **QP2.4** | Quais indicadores ambientais são utilizados para representar emissão de carbono, uso de água, sustentabilidade ou desperdício ambiental? |
| **QP2.5** | Quais atributos podem fundamentar a construção de um dataset para classificação do nível de desperdício ambiental em racks de datacenter? |

---

### 3.3. Eixo 3 — Geração de Datasets Sintéticos com LLM

| Código | Questão |
|---|---|
| **QP3.1** | Como LLMs são utilizados na geração de datasets sintéticos em estudos científicos? |
| **QP3.2** | Quais etapas metodológicas são adotadas para planejar, gerar, revisar e validar datasets sintéticos com LLM? |
| **QP3.3** | Quais estratégias são utilizadas para avaliar qualidade, consistência, plausibilidade e utilidade dos dados sintéticos gerados? |
| **QP3.4** | Como os estudos documentam prompts, versões, critérios de geração, rastreabilidade e limitações do processo? |
| **QP3.5** | Quais riscos, limitações e cuidados metodológicos são relatados no uso de LLMs para geração de datasets sintéticos? |

---

## 4. Expressões de Busca com PICOC

---

### 4.1. Eixo 1 — Estudo do Problema

| Elemento | Definição |
|---|---|
| **P — Population** | Datacenters, servidores, racks e infraestrutura computacional |
| **I — Intervention** | Classificação, predição, detecção, análise, otimização ou Machine Learning |
| **C — Comparison** | Não considerar |
| **O — Outcome** | Desperdício ambiental, impacto ambiental, eficiência energética, consumo energético, refrigeração, carbono e água |
| **C — Context** | Operação de datacenters e infraestrutura física/computacional |

#### String de Busca

```bash
("data center" OR datacenter OR "server rack" OR "rack-level" OR server OR "computing infrastructure")
AND
(classification OR prediction OR detection OR "anomaly detection" OR analysis OR optimization OR "machine learning" OR "artificial intelligence")
AND
("environmental impact" OR "energy waste" OR "energy efficiency" OR "power consumption" OR "energy consumption" OR cooling OR temperature OR "carbon footprint" OR "carbon emissions" OR "water consumption" OR sustainability)
AND
("data center operation" OR "data center infrastructure" OR "cloud infrastructure" OR "server room" OR "IT infrastructure" OR "facility operation")
```

---

### 4.2. Eixo 2 — Atributos e Métricas

| Elemento             | Definição                                                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **P — Population**   | Datacenters, servidores, racks e infraestrutura computacional                                                                 |
| **I — Intervention** | Atributos, métricas, indicadores, variáveis, features ou parâmetros operacionais                                              |
| **C — Comparison**   | Não considerar                                                                                                                |
| **O — Outcome**      | Representação de consumo energético, refrigeração, temperatura, utilização, carbono, água, eficiência e desperdício ambiental |
| **C — Context**      | Monitoramento, medição, modelagem, análise ou avaliação operacional de datacenters                                            |

#### String de Busca

```bash
("data center" OR datacenter OR "server rack" OR "rack-level" OR server OR "computing infrastructure")
AND
(attribute OR attributes OR metric OR metrics OR indicator OR indicators OR variable OR variables OR feature OR features OR parameter OR parameters)
AND
("energy consumption" OR "power consumption" OR "energy efficiency" OR cooling OR temperature OR "thermal efficiency" OR utilization OR workload OR "carbon footprint" OR "carbon emissions" OR "water consumption" OR sustainability OR "environmental impact" OR "energy waste")
AND
(monitoring OR measurement OR modeling OR assessment OR evaluation OR analysis OR dataset OR telemetry OR operation)
```

---

### 4.3. Eixo 3 — Geração de Datasets Sintéticos com LLM

| Elemento             | Definição                                                                                                     |
| -------------------- | ------------------------------------------------------------------------------------------------------------- |
| **P — Population**   | Datasets sintéticos, dados sintéticos e bases de dados geradas artificialmente                                |
| **I — Intervention** | Uso de LLMs, modelos generativos, ChatGPT, GPT, Claude, Gemini ou modelos de linguagem                        |
| **C — Comparison**   | Não considerar                                                                                                |
| **O — Outcome**      | Geração, validação, avaliação, controle de qualidade, documentação, rastreabilidade e reprodutibilidade       |
| **C — Context**      | Pesquisa científica, Machine Learning, ciência de dados, construção de datasets e experimentos computacionais |

#### String de Busca

```bash
("synthetic dataset" OR "synthetic datasets" OR "synthetic data" OR "artificial dataset" OR "generated dataset")
AND
("large language model" OR "large language models" OR LLM OR LLMs OR ChatGPT OR GPT OR "generative AI" OR "language model")
AND
("data generation" OR "dataset generation" OR generation OR validation OR evaluation OR "quality assessment" OR consistency OR plausibility OR reliability OR reproducibility OR documentation OR traceability)
AND
("machine learning" OR "data science" OR "scientific research" OR experiment OR methodology OR benchmark OR classification OR prediction)
```

---

## 5. Fontes de Busca

A busca será conduzida principalmente por meio do **SciSpace**, utilizado como interface de busca integrada, organização bibliográfica e análise assistida por IA.

A partir do SciSpace, serão considerados estudos recuperados de bases científicas como:

* IEEE Xplore;
* ACM Digital Library;
* arXiv;
* outras fontes acadêmicas indexadas.

---

## 6. Critérios de Inclusão e Exclusão

Como os três eixos possuem objetivos diferentes, os critérios foram organizados por eixo.

---

### 6.1. Critérios de Inclusão — Eixo 1

| Código    | Critério                                                                                                                          |
| --------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **CI1.1** | Estudos sobre datacenters, servidores, racks ou infraestrutura computacional.                                                     |
| **CI1.2** | Estudos que discutem energia, refrigeração, temperatura, carbono, água, sustentabilidade ou impacto ambiental.                    |
| **CI1.3** | Estudos que analisam desperdício, ineficiência, subutilização, consumo excessivo ou eficiência operacional.                       |
| **CI1.4** | Estudos que aplicam classificação, predição, detecção, otimização, Machine Learning ou análise computacional relacionada ao tema. |
| **CI1.5** | Estudos que apresentam dados reais, medições, simulações, datasets ou experimentos relacionados a datacenters.                    |

---

### 6.2. Critérios de Inclusão — Eixo 2

| Código    | Critério                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CI2.1** | Estudos sobre datacenters, servidores, racks ou infraestrutura computacional.                                                                       |
| **CI2.2** | Estudos que apresentem atributos, métricas, indicadores, variáveis ou features relacionadas ao funcionamento de datacenters.                        |
| **CI2.3** | Estudos que abordem consumo energético, potência, refrigeração, temperatura, utilização computacional, carga de trabalho ou eficiência operacional. |
| **CI2.4** | Estudos que apresentem indicadores ambientais, como emissão de carbono, uso de água, sustentabilidade ou impacto ambiental.                         |
| **CI2.5** | Estudos cujas informações possam fundamentar a escolha de atributos para um dataset sobre desperdício ambiental em racks de datacenter.             |

---

### 6.3. Critérios de Inclusão — Eixo 3

| Código    | Critério                                                                                                                   |
| --------- | -------------------------------------------------------------------------------------------------------------------------- |
| **CI3.1** | Estudos que utilizam LLMs ou modelos generativos textuais para gerar datasets sintéticos.                                  |
| **CI3.2** | Estudos que descrevem etapas metodológicas de geração, revisão, validação ou documentação dos dados sintéticos.            |
| **CI3.3** | Estudos que discutem qualidade, consistência, plausibilidade, utilidade, viés ou limitações dos dados gerados.             |
| **CI3.4** | Estudos que apresentam prompts, estratégias de prompting, critérios de geração ou mecanismos de rastreabilidade.           |
| **CI3.5** | Estudos aplicados a Machine Learning, ciência de dados, classificação, predição, benchmark ou experimentos computacionais. |

---

### 6.4. Critérios Gerais de Exclusão

| Código   | Critério                                                                         |
| -------- | -------------------------------------------------------------------------------- |
| **CEG1** | Estudos sem relação com o eixo de investigação correspondente.                   |
| **CEG2** | Estudos que mencionam o tema apenas de forma periférica.                         |
| **CEG3** | Trabalhos sem resumo ou sem acesso ao texto completo quando necessário.          |
| **CEG4** | Estudos duplicados.                                                              |
| **CEG5** | Blogs, notícias, materiais comerciais, tutoriais ou documentos sem método claro. |

---

### 6.5. Critérios Específicos de Exclusão — Eixo 1

| Código    | Critério                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------- |
| **CE1.1** | Estudos sobre sustentabilidade em tecnologia sem relação direta com datacenters, servidores ou racks.         |
| **CE1.2** | Estudos puramente sobre desempenho computacional, sem relação com energia, refrigeração ou impacto ambiental. |

---

### 6.6. Critérios Específicos de Exclusão — Eixo 2

| Código    | Critério                                                                                                       |
| --------- | -------------------------------------------------------------------------------------------------------------- |
| **CE2.1** | Estudos sobre datacenters que não apresentem atributos, métricas ou indicadores extraíveis.                    |
| **CE2.2** | Estudos puramente conceituais sobre sustentabilidade, sem dados, variáveis ou métricas aplicáveis.             |
| **CE2.3** | Estudos sobre desempenho computacional sem relação com energia, refrigeração, utilização ou impacto ambiental. |

---

### 6.7. Critérios Específicos de Exclusão — Eixo 3

| Código    | Critério                                                                                    |
| --------- | ------------------------------------------------------------------------------------------- |
| **CE3.1** | Estudos sobre dados sintéticos que não utilizam LLMs ou modelos generativos textuais.       |
| **CE3.2** | Estudos que apenas mencionam LLMs sem explicar o processo de geração dos dados.             |
| **CE3.3** | Estudos focados apenas em geração de texto, sem construção de dataset ou base estruturada.  |
| **CE3.4** | Estudos sem discussão de validação, qualidade, consistência ou utilidade dos dados gerados. |

---

## 7. Processo de Seleção

A seleção dos estudos será realizada em duas etapas: **CS1** e **CS2**.

---

### 7.1. CS1 — Título, Resumo, Palavras-chave e Ano

No CS1, serão avaliados:

* título;
* resumo;
* palavras-chave;
* ano;
* aderência inicial ao eixo de investigação;
* atendimento preliminar aos critérios de inclusão e exclusão.

---

### 7.2. CS2 — Leitura Completa

No CS2, serão avaliados:

* aderência real ao eixo correspondente;
* clareza metodológica;
* contribuição para responder às questões de pesquisa;
* existência de dados extraíveis;
* relação com o objetivo do trabalho.

| Eixo       | Foco no CS2                                                                                                                                                                         |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Eixo 1** | Aderência ao tema, método utilizado, dados ou métricas disponíveis, relação com desperdício ambiental, eficiência ou técnicas computacionais.                                       |
| **Eixo 2** | Presença real de atributos, métricas ou indicadores; relação com energia, refrigeração, temperatura, utilização ou impacto ambiental; utilidade para justificar colunas do dataset. |
| **Eixo 3** | Uso real de LLM; descrição do pipeline de geração; validação ou controle de qualidade; documentação de prompts, versões ou critérios; discussão de riscos, viés e limitações.       |

---

## 8. Uso de LLM

### 8.1. Uso do SciSpace

O SciSpace será usado como ferramenta auxiliar para:

* executar buscas;
* sugerir ajustes nas strings;
* localizar estudos relacionados;
* apoiar leitura inicial;
* organizar artigos candidatos;
* identificar atributos, métricas e práticas metodológicas relevantes.

Os prompts utilizados no SciSpace serão documentados em seção própria ou apêndice do relatório.

---

### 8.2. Uso do NotebookLM

O NotebookLM será utilizado principalmente após o CS1, durante o CS2 e a extração de dados.

Será usado para:

* leitura dirigida dos PDFs;
* localização de evidências;
* comparação entre artigos;
* extração de dados conforme os campos definidos;
* apoio à síntese dos resultados.

As decisões finais de inclusão, exclusão, interpretação e síntese serão realizadas pelos autores.

---

## 9. Tabela de Extração de Dados

A extração será organizada em três grupos:

1. **A) Dados da publicação**
2. **B) Dados relacionados ao objetivo do eixo**
3. **C) Dados para análise crítica**

---

### 9.1. Campos Comuns

| Grupo                  | Campo                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------- |
| **A) Publicação**      | título, autores, ano, fonte, tipo de estudo, idioma                                               |
| **C) Análise crítica** | relevância, contribuição ao trabalho, limitações, localidade, força da evidência quando aplicável |

---

### 9.2. Campos Específicos — Eixo 1

| Grupo                       | Campo                                                                                                                               |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **B) Objetivo da pesquisa** | problema tratado, impacto ambiental, unidade de análise, métricas, técnica computacional, dataset/dados usados, resultados, lacunas |

---

### 9.3. Campos Específicos — Eixo 2

| Grupo                       | Campo                                                                                                                                                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **B) Objetivo da pesquisa** | atributos energéticos, atributos térmicos, atributos computacionais, indicadores ambientais, métricas, unidade de análise, relação com desperdício/eficiência, atributos aproveitáveis |

---

### 9.4. Campos Específicos — Eixo 3

| Grupo                             | Campo                                                                                                                                                                            |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **B) Objetivo da pesquisa**       | LLM usado, tipo de dataset, domínio, estratégia de prompting, pipeline, revisão humana, validação, critérios de qualidade, documentação, rastreabilidade, resultados, limitações |
| **C) Análise crítica específica** | contribuição ao pipeline, riscos, viés, aplicabilidade a dataset tabular, práticas aproveitáveis, práticas inadequadas                                                           |

---

## 10. Síntese dos Resultados

A síntese será feita por eixo e, ao final, integrada ao desenvolvimento do dataset.

---

### 10.1. Síntese do Eixo 1

A síntese do Eixo 1 deverá apresentar:

* impactos ambientais associados a datacenters;
* fatores de desperdício;
* métricas utilizadas;
* técnicas computacionais aplicadas;
* lacunas existentes;
* relação com a classificação do nível de desperdício ambiental em racks.

---

### 10.2. Síntese do Eixo 2

A síntese do Eixo 2 deverá apresentar:

* atributos e métricas encontrados;
* agrupamento dos atributos por tipo;
* atributos aproveitáveis para o dataset;
* justificativa para escolha das colunas;
* atributos descartados ou pouco aplicáveis.

---

### 10.3. Síntese do Eixo 3

A síntese do Eixo 3 deverá apresentar:

* formas de uso de LLMs para gerar datasets sintéticos;
* etapas metodológicas recorrentes;
* práticas de validação e controle de qualidade;
* documentação e rastreabilidade;
* riscos e limitações;
* práticas aproveitáveis para o pipeline de geração do dataset deste trabalho.

---

## 11. Integração dos Três Eixos

Os resultados dos três eixos serão integrados da seguinte forma:

| Resultado                                  | Origem          |
| ------------------------------------------ | --------------- |
| Justificativa do problema                  | Eixo 1          |
| Definição dos atributos do dataset         | Eixo 2          |
| Pipeline de geração sintética com LLM      | Eixo 3          |
| Regras semânticas do dataset               | Eixo 1 + Eixo 2 |
| Documentação dos prompts e rastreabilidade | Eixo 3          |
| Justificativa da tarefa de classificação   | Eixo 1          |
| Fundamentação da classe-alvo               | Eixo 1 + Eixo 2 |

A integração dos três eixos servirá como base para a etapa seguinte do trabalho: **especificação e geração do dataset sintético**.

---

## 12. Resultados Esperados do Protocolo

Ao final do mapeamento integrado, espera-se obter:

* fundamentação teórica sobre desperdício ambiental em datacenters;
* identificação de métricas e indicadores relevantes;
* lista justificada de atributos para o dataset sintético;
* compreensão dos riscos e limitações do uso de LLMs;
* pipeline metodológico para geração do dataset;
* base para documentação dos prompts, regras e decisões de geração.

---

## 13. Referências

KITCHENHAM, B.; CHARTERS, S. M. **Guidelines for performing systematic literature reviews in software engineering**. Keele, 2007.

PETERSEN, K. et al. **Systematic mapping studies in software engineering**. In: *Evaluation and Assessment in Software Engineering (EASE)*. Bari: EASE, 2008. p. 68–77.
