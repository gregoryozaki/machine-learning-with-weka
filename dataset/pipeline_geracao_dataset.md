# Pipeline Metodológico de Geração do Dataset

O pipeline de geração foi baseado nas práticas encontradas no MSL (Eixo 3 — Geração de Datasets Sintéticos com LLM), especialmente em estudos sobre geração de datasets sintéticos com LLMs, dados tabulares, validação, controle de qualidade e documentação.

O processo está organizado nas seguintes etapas:

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

* [README](/README.md),[engenharia de atributos](/engenharia_atributos.md).

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

* [regras semanticas](/regras_semanticas.md).

**Justificativa:**

A geração com LLM não deve ser livre ou aleatória. As regras semânticas funcionam como restrições de domínio que orientam o modelo a gerar dados plausíveis para o contexto de racks de datacenter.

---

### Etapa 3 — Planejamento de Anomalias e Inconsistências

**Objetivo:** definir previamente como valores faltantes, ruído e outliers serão tratados no processo de geração do dataset.

**O que será feito:**

* definir quais atributos poderão receber valores faltantes;
* definir quais atributos poderão receber ruído;
* definir quais tipos de outliers serão inseridos;
* definir proporções aproximadas para cada tipo de inconsistência;
* garantir que esses elementos sejam inseridos de forma controlada e documentada.

**Artefato gerado:**

* [planejamento da inserção controlada de valores faltantes, ruidos e outliers](/planejamento_anomalias_inconsistencias.md)

**Justificativa:**

Valores faltantes, ruído e outliers são requisitos do trabalho e também simulam problemas comuns em dados operacionais reais. Planejar esses elementos antes da geração evita inserções aleatórias e permite controlar melhor a qualidade e a rastreabilidade do dataset.

---

### Etapa 4 — Construção dos Prompts

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

### Etapa 5 — Geração de Amostra Inicial

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

### Etapa 6 — Geração Completa do Dataset

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

### Etapa 7 — Inserção Controlada de Valores Faltantes, Ruído e Outliers

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

### Etapa 8 — Validação Estrutural e Semântica da Geração

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

### Etapa 9 — Documentação e Rastreabilidade

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