# Documentação da Geração do Dataset Sintético

## Classificação do Nível de Risco de Desperdício Ambiental em Racks de Datacenters Voltados a Cargas de IA

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

## 3. Estrutura Geral do Dataset

### 3.1. Unidade de Análise

Cada linha do dataset representa:

> Um rack de datacenter em uma hora de operação.

Essa unidade permite relacionar variáveis de consumo, refrigeração, carga computacional e impacto ambiental em uma janela temporal simples e interpretável.

---

### 3.2. Classe-Alvo

A variável-alvo será:

```bash
environmental_waste_risk_level
```

Com os seguintes valores:

| Classe       | Descrição                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------- |
| **baixo**    | Operação proporcional entre uso computacional, consumo energético e refrigeração.           |
| **moderado** | Presença de sinais intermediários de ineficiência ou desperdício.                           |
| **alto**     | Consumo, refrigeração ou impacto ambiental desproporcional em relação à utilização do rack. |

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

