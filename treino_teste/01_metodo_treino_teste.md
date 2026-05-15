# Método de Treino e Teste

#### Responsável: `Gregory Ozaki`

## Objetivo

Este documento descreve o método usado para treinar, testar e organizar os classificadores no Weka.

A etapa tem como objetivo comparar os algoritmos de classificação abordados em sala, usando duas versões do dataset:

```bash
dataset/dataset_preprocessado.arff
dataset/dataset_preprocessado_attrselect.arff
```

Classe-alvo:

```bash
environmental_waste_risk_level
```

Classes:

```bash
baixo
moderado
alto
```

---

## 1. Organização da pasta

Como foram realizadas várias execuções, os resultados foram organizados por algoritmo dentro da pasta `treino_teste`.

Estrutura adotada:

```bash
treino_teste/
├── 01_metodo.md
├── 02_analise_resultados.md
└── algoritmos/
    ├── 01_zeror.md
    ├── 02_oner.md
    ├── 03_ibk_knn.md
    ├── 04_naive_bayes.md
    ├── 05_smo_svm.md
    ├── 06_j48.md
    └── 07_random_forest.md
```

Cada arquivo dentro de `algoritmos/` contém:

* explicação resumida do algoritmo;
* configuração usada no Weka;
* resultado no `dataset_preprocessado.arff`;
* resultado no `dataset_preprocessado_attrselect.arff`;
* matriz de confusão;
* análise comparativa entre os dois datasets;
* síntese do comportamento do modelo.

Nos arquivos `random_forest.md` e `ibk_knn.md`, também foi incluída a análise do ajuste de hiperparâmetros.

O arquivo `02_analise_resultados.md` será usado para consolidar os resultados e comparar todos os algoritmos.

---

## 2. Datasets utilizados

| Dataset                                 | Descrição                                                                                                                                             | Papel                          |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| `dataset_preprocessado.arff`            | Dataset principal após tratamento de missing, remoção de atributos irrelevantes, conversão de `num_gpus`, aplicação de `RemoveUseless` e normalização | Base principal                 |
| `dataset_preprocessado_attrselect.arff` | Dataset complementar gerado com `AttributeSelection`                                                                                                  | Comparação com versão reduzida |

A versão principal foi considerada a referência do experimento. A versão com `AttributeSelection` foi usada para verificar se a redução automática de atributos melhora ou prejudica o desempenho dos classificadores.

---

## 3. Estratégia de avaliação

A avaliação foi realizada no Weka pela aba:

```bash
Explorer > Classify
```

Configuração de teste:

```bash
Cross-validation
Folds: 10
```

A validação cruzada com 10 folds foi escolhida porque o dataset possui 674 instâncias, três classes, ruído planejado, outliers interpretáveis e desbalanceamento moderado da classe `alto`.

Essa estratégia é mais estável do que um único `Percentage split`, pois reduz a dependência de uma separação aleatória específica entre treino e teste. Além disso, permite que todas as instâncias sejam usadas em treino e teste em diferentes partições.

A opção `Use training set` não foi usada como avaliação final, pois tende a superestimar o desempenho ao testar o modelo nos mesmos dados usados para treinamento.

---

## 4. Algoritmos avaliados

Foram avaliados os algoritmos de classificação abordados em sala.

| Algoritmo no trabalho | Nome no Weka   | Papel                                  |
| --------------------- | -------------- | -------------------------------------- |
| ZeroR                 | `ZeroR`        | Baseline mínimo                        |
| OneR                  | `OneR`         | Baseline simples baseado em uma regra  |
| Naive Bayes           | `NaiveBayes`   | Modelo probabilístico                  |
| Árvore de decisão     | `J48`          | Modelo interpretável baseado em regras |
| Random Forest         | `RandomForest` | Ensemble de árvores                    |
| KNN                   | `IBk`          | Classificador baseado em vizinhança    |
| SVM                   | `SMO`          | Classificador baseado em margem        |

### Justificativa resumida

* `ZeroR` e `OneR` foram usados como baselines para verificar se os modelos mais complexos realmente aprendem padrões úteis.
* `J48` foi avaliado porque as visualizações mostraram separações por faixas e fronteiras interpretáveis.
* `RandomForest` foi testado por sua presença frequente na literatura e por lidar bem com múltiplas relações, ruído e interações entre atributos.
* `IBk` foi testado porque os dados foram normalizados e algumas visualizações indicaram agrupamentos locais.
* `SMO` foi testado porque pode explorar separações geométricas em dados normalizados.
* `NaiveBayes` foi usado como baseline probabilístico, embora existam dependências entre alguns atributos.

---

## 5. Procedimento geral no Weka

Para cada dataset e algoritmo, foi seguido o procedimento:

```text
1. Abrir o arquivo .arff no Weka
2. Ir para a aba Classify
3. Confirmar a classe-alvo: environmental_waste_risk_level
4. Selecionar Cross-validation
5. Definir Folds = 10
6. Escolher o algoritmo
7. Clicar em Start
8. Registrar acurácia, métricas por classe e matriz de confusão
```

A classe-alvo foi mantida como:

```bash
(Nom) environmental_waste_risk_level
```

---

## 6. Execuções principais

Cada algoritmo foi executado nos dois datasets.

| Execução | Dataset                                 | Algoritmo      |
| -------: | --------------------------------------- | -------------- |
|        1 | `dataset_preprocessado.arff`            | `ZeroR`        |
|        2 | `dataset_preprocessado.arff`            | `OneR`         |
|        3 | `dataset_preprocessado.arff`            | `NaiveBayes`   |
|        4 | `dataset_preprocessado.arff`            | `J48`          |
|        5 | `dataset_preprocessado.arff`            | `RandomForest` |
|        6 | `dataset_preprocessado.arff`            | `IBk`          |
|        7 | `dataset_preprocessado.arff`            | `SMO`          |
|        8 | `dataset_preprocessado_attrselect.arff` | `ZeroR`        |
|        9 | `dataset_preprocessado_attrselect.arff` | `OneR`         |
|       10 | `dataset_preprocessado_attrselect.arff` | `NaiveBayes`   |
|       11 | `dataset_preprocessado_attrselect.arff` | `J48`          |
|       12 | `dataset_preprocessado_attrselect.arff` | `RandomForest` |
|       13 | `dataset_preprocessado_attrselect.arff` | `IBk`          |
|       14 | `dataset_preprocessado_attrselect.arff` | `SMO`          |

Total principal:

```bash
7 algoritmos × 2 datasets = 14 execuções
```

---

## 7. Ajuste de hiperparâmetros

O enunciado solicita ajuste de hiperparâmetros quando aplicável. Neste trabalho, os ajustes foram feitos em dois algoritmos:

```bash
RandomForest
IBk
```

A ideia não foi testar muitas combinações aleatórias, mas comparar configurações específicas e justificadas.

---

## 7.1. Ajuste do Random Forest

### Justificativa

O `RandomForest` foi escolhido para ajuste porque é um algoritmo robusto e adequado para problemas com múltiplas relações entre atributos. Como o dataset possui interações entre consumo, utilização, temperatura, densidade energética e status operacional, faz sentido avaliar se o aumento do número de árvores melhora a estabilidade do modelo.

### Configurações avaliadas

| Versão           | Parâmetro  | Valor |
| ---------------- | ---------- | ----: |
| Ajuste 1         | `numTrees` |    50 |
| Padrão observado | `numTrees` |   100 |
| Ajuste 2         | `numTrees` |   200 |

No Weka, o parâmetro aparece como `numTrees`. A configuração padrão observada foi de 100 árvores.

Os demais parâmetros foram mantidos no padrão para evitar misturar efeitos de múltiplas alterações ao mesmo tempo.

### Métricas comparadas

Foram comparados:

* acurácia;
* Kappa;
* F1 ponderado;
* recall da classe `alto`;
* matriz de confusão;
* erro entre as classes `baixo`, `moderado` e `alto`;
* erro `out of bag`.

---

## 7.2. Ajuste do IBk / KNN

### Justificativa

O `IBk` depende diretamente do número de vizinhos considerados. Como o dataset foi normalizado e apresenta regiões de sobreposição entre classes, a escolha do valor de `K` pode alterar bastante o comportamento do classificador.

### Configurações avaliadas

| Versão   | Parâmetro | Valor |
| -------- | --------- | ----: |
| Padrão   | `K`       |     1 |
| Ajuste 1 | `K`       |     3 |
| Ajuste 2 | `K`       |     5 |
| Ajuste 3 | `K`       |     7 |

### Métricas comparadas

Foram comparados:

* acurácia;
* Kappa;
* F1 ponderado;
* recall da classe `alto`;
* matriz de confusão;
* impacto do aumento de `K`;
* possível suavização excessiva das fronteiras de decisão.

---

## 8. Métricas registradas

Para cada execução, foram registrados os principais resultados exibidos pelo Weka.

| Métrica                          | Motivo                                           |
| -------------------------------- | ------------------------------------------------ |
| Correctly Classified Instances   | Acurácia geral                                   |
| Incorrectly Classified Instances | Erros gerais                                     |
| Kappa statistic                  | Concordância além do acaso                       |
| Mean absolute error              | Erro médio absoluto                              |
| Root mean squared error          | Erro quadrático médio                            |
| TP Rate por classe               | Sensibilidade/recall por classe                  |
| FP Rate por classe               | Falsos positivos por classe                      |
| Precision por classe             | Precisão por classe                              |
| Recall por classe                | Capacidade de recuperar cada classe              |
| F-Measure por classe             | Equilíbrio entre precisão e recall               |
| ROC Area                         | Capacidade de separação por classe               |
| Confusion Matrix                 | Tipos de erro entre `baixo`, `moderado` e `alto` |

A análise não foi limitada à acurácia. A classe `alto` recebeu atenção especial, pois representa maior risco de desperdício ambiental. Assim, recall e F-measure da classe `alto` foram considerados na interpretação dos modelos.

---

## 9. Organização dos registros por algoritmo

Cada arquivo da pasta `treino_teste/algoritmos/` segue esta estrutura geral:

```md
# Nome do algoritmo

## Funcionamento do algoritmo

## Configuração no Weka

## Resultados obtidos

## Matriz de confusão

## Análise dos resultados

## Comparação entre os datasets

## Síntese

## Resposta Weka
```

Para `RandomForest` e `IBk`, também foram adicionadas seções específicas sobre ajuste de hiperparâmetros.

---

## 10. Tabela-base para o arquivo de análise geral

A tabela abaixo será consolidada no arquivo `02_analise_resultados.md`.

| Dataset                                 | Algoritmo    | Configuração     | Acurácia | Kappa | F1 ponderado | Recall classe `alto` | Observação       |
| --------------------------------------- | ------------ | ---------------- | -------: | ----: | -----------: | -------------------: | ---------------- |
| `dataset_preprocessado.arff`            | ZeroR        | padrão           |          |       |              |                      | Baseline mínimo  |
| `dataset_preprocessado.arff`            | OneR         | padrão           |          |       |              |                      | Baseline simples |
| `dataset_preprocessado.arff`            | NaiveBayes   | padrão           |          |       |              |                      |                  |
| `dataset_preprocessado.arff`            | J48          | padrão           |          |       |              |                      |                  |
| `dataset_preprocessado.arff`            | RandomForest | `numTrees = 50`  |          |       |              |                      | Ajuste           |
| `dataset_preprocessado.arff`            | RandomForest | `numTrees = 100` |          |       |              |                      | Padrão observado |
| `dataset_preprocessado.arff`            | RandomForest | `numTrees = 200` |          |       |              |                      | Ajuste           |
| `dataset_preprocessado.arff`            | IBk          | `K = 1`          |          |       |              |                      | Padrão           |
| `dataset_preprocessado.arff`            | IBk          | `K = 3`          |          |       |              |                      | Ajuste           |
| `dataset_preprocessado.arff`            | IBk          | `K = 5`          |          |       |              |                      | Ajuste           |
| `dataset_preprocessado.arff`            | IBk          | `K = 7`          |          |       |              |                      | Ajuste           |
| `dataset_preprocessado.arff`            | SMO          | padrão           |          |       |              |                      |                  |
| `dataset_preprocessado_attrselect.arff` | ZeroR        | padrão           |          |       |              |                      | Baseline mínimo  |
| `dataset_preprocessado_attrselect.arff` | OneR         | padrão           |          |       |              |                      | Baseline simples |
| `dataset_preprocessado_attrselect.arff` | NaiveBayes   | padrão           |          |       |              |                      |                  |
| `dataset_preprocessado_attrselect.arff` | J48          | padrão           |          |       |              |                      |                  |
| `dataset_preprocessado_attrselect.arff` | RandomForest | `numTrees = 50`  |          |       |              |                      | Ajuste           |
| `dataset_preprocessado_attrselect.arff` | RandomForest | `numTrees = 100` |          |       |              |                      | Padrão observado |
| `dataset_preprocessado_attrselect.arff` | RandomForest | `numTrees = 200` |          |       |              |                      | Ajuste           |
| `dataset_preprocessado_attrselect.arff` | IBk          | `K = 1`          |          |       |              |                      | Padrão           |
| `dataset_preprocessado_attrselect.arff` | IBk          | `K = 3`          |          |       |              |                      | Ajuste           |
| `dataset_preprocessado_attrselect.arff` | IBk          | `K = 5`          |          |       |              |                      | Ajuste           |
| `dataset_preprocessado_attrselect.arff` | IBk          | `K = 7`          |          |       |              |                      | Ajuste           |
| `dataset_preprocessado_attrselect.arff` | SMO          | padrão           |          |       |              |                      |                  |

---

## 11. Critérios de comparação

A comparação dos resultados deverá considerar:

1. desempenho acima dos baselines `ZeroR` e `OneR`;
2. acurácia geral;
3. Kappa;
4. F1 ponderado;
5. recall da classe `alto`;
6. matriz de confusão;
7. erros entre `baixo` e `moderado`;
8. erros entre `moderado` e `alto`;
9. diferença entre dataset principal e dataset com `AttributeSelection`;
10. impacto dos ajustes em `RandomForest` e `IBk`;
11. coerência entre resultados numéricos e análise visual anterior.

A escolha do melhor algoritmo não deve depender apenas da acurácia. Em um problema de risco ambiental, classificar registros `alto` como `baixo` ou `moderado` é mais grave do que outros tipos de erro.

Por isso, a análise final deve considerar especialmente:

```bash
recall da classe alto
F1 ponderado
matriz de confusão
erros críticos envolvendo a classe alto
```

