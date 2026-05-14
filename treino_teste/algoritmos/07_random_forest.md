# Random Forest

## Funcionamento do algoritmo

O `RandomForest` é um algoritmo baseado em conjunto de árvores de decisão. Em vez de construir uma única árvore, ele cria várias árvores e combina suas previsões para produzir a classificação final.

Cada árvore é construída usando variações dos dados e subconjuntos de atributos. Isso tende a tornar o modelo mais robusto do que uma árvore única, reduzindo a dependência de uma regra específica.

Neste experimento, o hiperparâmetro ajustado foi:

```bash
numTrees
```

Foram testados três valores:

```bash
numTrees = 50
numTrees = 100
numTrees = 200
```

O objetivo foi verificar se o aumento do número de árvores melhora a estabilidade e o desempenho do classificador.

---

## Configuração no Weka

```bash
Scheme: weka.classifiers.trees.RandomForest
Test mode: 10-fold cross-validation
Class: environmental_waste_risk_level
```

O algoritmo foi executado com validação cruzada estratificada de 10 folds, mantendo a mesma estratégia de avaliação usada para os demais modelos.

No dataset completo, cada árvore considerou 5 atributos aleatórios por divisão. No dataset com `AttributeSelection`, cada árvore considerou 3 atributos aleatórios, devido à redução no número total de atributos.

---

## Resultados obtidos

### `dataset_preprocessado.arff`

| numTrees | Acurácia |    Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` | Out of bag error |
| -------: | -------: | ------: | -----: | -----------: | -------------: | ----------------: | ------------: | ---------------: |
|       50 | 91,5430% | 8,4570% | 0,8700 |        0,915 |          0,937 |             0,855 |         0,975 |           0,1053 |
|      100 | 91,6914% | 8,3086% | 0,8723 |        0,917 |          0,933 |             0,863 |         0,975 |           0,0920 |
|      200 | 91,9881% | 8,0119% | 0,8769 |        0,920 |          0,929 |             0,875 |         0,975 |           0,0831 |

### `dataset_preprocessado_attrselect.arff`

| numTrees | Acurácia |     Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` | Out of bag error |
| -------: | -------: | -------: | -----: | -----------: | -------------: | ----------------: | ------------: | ---------------: |
|       50 | 85,9050% | 14,0950% | 0,7838 |        0,859 |          0,843 |             0,802 |         0,975 |           0,1320 |
|      100 | 86,6469% | 13,3531% | 0,7950 |        0,866 |          0,854 |             0,815 |         0,968 |           0,1231 |
|      200 | 87,0920% | 12,9080% | 0,8019 |        0,871 |          0,854 |             0,823 |         0,975 |           0,1217 |

---

## Melhor configuração por dataset

| Dataset                                 | Melhor configuração | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| --------------------------------------- | ------------------: | -------: | -----: | -----------: | ------------: |
| `dataset_preprocessado.arff`            |    `numTrees = 200` | 91,9881% | 0,8769 |        0,920 |         0,975 |
| `dataset_preprocessado_attrselect.arff` |    `numTrees = 200` | 87,0920% | 0,8019 |        0,871 |         0,975 |

---

## Matrizes de confusão

### `dataset_preprocessado.arff` — melhor configuração: `numTrees = 200`

```bash
   a   b   c   <-- classified as
 249  19   0 |   a = baixo
  28 217   3 |   b = moderado
   0   4 154 |   c = alto
```

### `dataset_preprocessado_attrselect.arff` — melhor configuração: `numTrees = 200`

```bash
   a   b   c   <-- classified as
 229  39   0 |   a = baixo
  39 204   5 |   b = moderado
   0   4 154 |   c = alto
```

---

## Análise dos resultados no dataset preprocessado

No `dataset_preprocessado.arff`, o `RandomForest` apresentou o melhor desempenho geral entre as configurações testadas.

A melhor configuração foi:

```bash
numTrees = 200
```

com:

```bash
Acurácia: 91,9881%
Kappa: 0,8769
F1 ponderado: 0,920
Recall alto: 0,975
```

O aumento no número de árvores gerou melhora gradual no desempenho:

```bash
50 árvores  -> 91,5430%
100 árvores -> 91,6914%
200 árvores -> 91,9881%
```

A melhora não foi grande, mas foi consistente. Além disso, o erro `out of bag` também diminuiu conforme o número de árvores aumentou:

```bash
0,1053 -> 0,0920 -> 0,0831
```

Isso sugere que o aumento do número de árvores deixou o modelo mais estável.

A matriz de confusão mostra que a classe `alto` foi muito bem identificada:

```bash
alto: 154 acertos de 158
```

Apenas 4 registros da classe `alto` foram classificados como `moderado`, e nenhum foi classificado como `baixo`. Esse é um resultado importante, pois evita o erro mais grave para o problema: tratar um caso de alto risco como baixo risco.

Os principais erros ocorreram entre as classes `baixo` e `moderado`:

```bash
baixo -> moderado: 19 casos
moderado -> baixo: 28 casos
```

Esse comportamento é coerente com as visualizações anteriores, nas quais `baixo` e `moderado` apresentaram maior sobreposição.

---

## Análise do dataset com AttributeSelection

No `dataset_preprocessado_attrselect.arff`, o `RandomForest` também teve melhor resultado com:

```bash
numTrees = 200
```

A configuração alcançou:

```bash
Acurácia: 87,0920%
Kappa: 0,8019
F1 ponderado: 0,871
Recall alto: 0,975
```

O aumento do número de árvores também melhorou o desempenho na versão reduzida:

```bash
50 árvores  -> 85,9050%
100 árvores -> 86,6469%
200 árvores -> 87,0920%
```

A classe `alto` novamente foi muito bem classificada, com 154 acertos em 158 registros.

Porém, o desempenho geral foi inferior ao dataset completo. Isso indica que o `AttributeSelection` removeu atributos que ajudavam o `RandomForest` a compor decisões mais robustas.

A matriz de confusão mostra maior quantidade de erros entre `baixo` e `moderado` na versão reduzida:

```bash
baixo -> moderado: 39 casos
moderado -> baixo: 39 casos
```

No dataset completo, esses erros foram menores. Portanto, a versão com menos atributos manteve boa identificação da classe `alto`, mas perdeu capacidade de separar melhor as classes de menor risco.

---

## Impacto do ajuste de hiperparâmetro

O ajuste do parâmetro `numTrees` teve impacto positivo nos dois datasets.

No dataset completo:

```bash
numTrees = 50  -> 91,5430%
numTrees = 100 -> 91,6914%
numTrees = 200 -> 91,9881%
```

No dataset com `AttributeSelection`:

```bash
numTrees = 50  -> 85,9050%
numTrees = 100 -> 86,6469%
numTrees = 200 -> 87,0920%
```

Em ambos os casos, a melhor configuração foi `numTrees = 200`.

O ganho foi moderado, mas consistente. Isso justifica a escolha de `numTrees = 200` como melhor configuração entre as testadas.

---

## Comparação entre os datasets

| Critério                   | Dataset completo | Dataset com AttributeSelection |
| -------------------------- | ---------------: | -----------------------------: |
| Melhor configuração        |      200 árvores |                    200 árvores |
| Acurácia                   |         91,9881% |                       87,0920% |
| Kappa                      |           0,8769 |                         0,8019 |
| F1 ponderado               |            0,920 |                          0,871 |
| Recall `baixo`             |            0,929 |                          0,854 |
| Recall `moderado`          |            0,875 |                          0,823 |
| Recall `alto`              |            0,975 |                          0,975 |
| Erros `baixo` ↔ `moderado` |               47 |                             78 |

O dataset completo foi superior em desempenho geral. A versão com `AttributeSelection` manteve o mesmo recall da classe `alto`, mas teve pior separação entre `baixo` e `moderado`.

Isso mostra que, para o `RandomForest`, a redução automática de atributos não foi vantajosa. O algoritmo já possui mecanismos internos de seleção aleatória de atributos em cada árvore, então a remoção prévia de variáveis pode ter reduzido informações úteis.

---

## Síntese

O `RandomForest` foi o melhor algoritmo avaliado até esta etapa.

A melhor configuração geral foi:

```bash
dataset_preprocessado.arff
numTrees = 200
```

com:

```bash
Acurácia: 91,9881%
F1 ponderado: 0,920
Recall alto: 0,975
```

O modelo apresentou ótimo equilíbrio entre desempenho geral e identificação da classe mais crítica. Além disso, classificou muito bem a classe `alto`, sem confundir nenhum registro dessa classe com `baixo`.

O `AttributeSelection` não melhorou o resultado do `RandomForest`. Pelo contrário, reduziu o desempenho geral, principalmente na separação entre `baixo` e `moderado`.

Portanto, para este algoritmo, o dataset completo é a melhor opção.

---

## Resposta Weka

### `dataset_preprocessado.arff`

Como as três execuções possuem a mesma estrutura de atributos, a saída abaixo foi resumida para evitar repetição. A diferença principal entre elas é o valor de `numTrees`.

#### `numTrees = 50`

```bash
Scheme:weka.classifiers.trees.RandomForest -I 50 -K 0 -S 1
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

Random forest of 50 trees, each constructed while considering 5 random features.
Out of bag error: 0.1053

=== Summary ===

Correctly Classified Instances         617               91.543  %
Incorrectly Classified Instances        57                8.457  %
Kappa statistic                          0.87  
Mean absolute error                      0.129 
Root mean squared error                  0.2171
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.937     0.081      0.884     0.937     0.909      0.979    baixo
                 0.855     0.049      0.91      0.855     0.881      0.973    moderado
                 0.975     0.006      0.981     0.975     0.978      1        alto
Weighted Avg.    0.915     0.052      0.916     0.915     0.915      0.982

=== Confusion Matrix ===

   a   b   c   <-- classified as
 251  17   0 |   a = baixo
  33 212   3 |   b = moderado
   0   4 154 |   c = alto
```

#### `numTrees = 100`

```bash
Scheme:weka.classifiers.trees.RandomForest -I 100 -K 0 -S 1
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

Random forest of 100 trees, each constructed while considering 5 random features.
Out of bag error: 0.092

=== Summary ===

Correctly Classified Instances         618               91.6914 %
Incorrectly Classified Instances        56                8.3086 %
Kappa statistic                          0.8723
Mean absolute error                      0.1303
Root mean squared error                  0.2174
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.933     0.076      0.89      0.933     0.911      0.979    baixo
                 0.863     0.052      0.907     0.863     0.884      0.973    moderado
                 0.975     0.006      0.981     0.975     0.978      1        alto
Weighted Avg.    0.917     0.051      0.917     0.917     0.917      0.982

=== Confusion Matrix ===

   a   b   c   <-- classified as
 250  18   0 |   a = baixo
  31 214   3 |   b = moderado
   0   4 154 |   c = alto
```

#### `numTrees = 200`

```bash
Scheme:weka.classifiers.trees.RandomForest -I 200 -K 0 -S 1
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

Random forest of 200 trees, each constructed while considering 5 random features.
Out of bag error: 0.0831

=== Summary ===

Correctly Classified Instances         620               91.9881 %
Incorrectly Classified Instances        54                8.0119 %
Kappa statistic                          0.8769
Mean absolute error                      0.1309
Root mean squared error                  0.2168
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.929     0.069      0.899     0.929     0.914      0.98     baixo
                 0.875     0.054      0.904     0.875     0.889      0.974    moderado
                 0.975     0.006      0.981     0.975     0.978      1        alto
Weighted Avg.    0.92      0.049      0.92      0.92      0.92       0.982

=== Confusion Matrix ===

   a   b   c   <-- classified as
 249  19   0 |   a = baixo
  28 217   3 |   b = moderado
   0   4 154 |   c = alto
```

---

### `dataset_preprocessado_attrselect.arff`

A versão com `AttributeSelection` possui 6 atributos. O `RandomForest` considerou 3 atributos aleatórios por árvore nessa versão.

#### `numTrees = 50`

```bash
Scheme:weka.classifiers.trees.RandomForest -I 50 -K 0 -S 1
Instances:    674
Attributes:   6
Test mode:10-fold cross-validation

Random forest of 50 trees, each constructed while considering 3 random features.
Out of bag error: 0.132

=== Summary ===

Correctly Classified Instances         579               85.905  %
Incorrectly Classified Instances        95               14.095  %
Kappa statistic                          0.7838
Mean absolute error                      0.1261
Root mean squared error                  0.2538
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.843     0.106      0.84      0.843     0.842      0.954    baixo
                 0.802     0.108      0.812     0.802     0.807      0.935    moderado
                 0.975     0.012      0.963     0.975     0.969      0.998    alto
Weighted Avg.    0.859     0.085      0.859     0.859     0.859      0.958

=== Confusion Matrix ===

   a   b   c   <-- classified as
 226  42   0 |   a = baixo
  43 199   6 |   b = moderado
   0   4 154 |   c = alto
```

#### `numTrees = 100`

```bash
Scheme:weka.classifiers.trees.RandomForest -I 100 -K 0 -S 1
Instances:    674
Attributes:   6
Test mode:10-fold cross-validation

Random forest of 100 trees, each constructed while considering 3 random features.
Out of bag error: 0.1231

=== Summary ===

Correctly Classified Instances         584               86.6469 %
Incorrectly Classified Instances        90               13.3531 %
Kappa statistic                          0.795 
Mean absolute error                      0.1259
Root mean squared error                  0.2524
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.854     0.101      0.848     0.854     0.851      0.956    baixo
                 0.815     0.103      0.821     0.815     0.818      0.936    moderado
                 0.968     0.01       0.968     0.968     0.968      0.998    alto
Weighted Avg.    0.866     0.08       0.866     0.866     0.866      0.959

=== Confusion Matrix ===

   a   b   c   <-- classified as
 229  39   0 |   a = baixo
  41 202   5 |   b = moderado
   0   5 153 |   c = alto
```

#### `numTrees = 200`

```bash
Scheme:weka.classifiers.trees.RandomForest -I 200 -K 0 -S 1
Instances:    674
Attributes:   6
Test mode:10-fold cross-validation

Random forest of 200 trees, each constructed while considering 3 random features.
Out of bag error: 0.1217

=== Summary ===

Correctly Classified Instances         587               87.092  %
Incorrectly Classified Instances        87               12.908  %
Kappa statistic                          0.8019
Mean absolute error                      0.1255
Root mean squared error                  0.2508
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.854     0.096      0.854     0.854     0.854      0.957    baixo
                 0.823     0.101      0.826     0.823     0.824      0.939    moderado
                 0.975     0.01       0.969     0.975     0.972      0.998    alto
Weighted Avg.    0.871     0.078      0.871     0.871     0.871      0.96 

=== Confusion Matrix ===

   a   b   c   <-- classified as
 229  39   0 |   a = baixo
  39 204   5 |   b = moderado
   0   4 154 |   c = alto
```
