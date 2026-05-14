# IBk / KNN

## Funcionamento do algoritmo

O `IBk` é a implementação do algoritmo KNN no Weka. Ele classifica uma instância com base nas classes dos vizinhos mais próximos no espaço de atributos.

Neste experimento, foi usada a distância euclidiana:

```bash
weka.core.EuclideanDistance
```

Como o KNN depende diretamente de distância, a normalização feita no pré-processamento foi essencial. Sem normalização, atributos com valores originalmente maiores poderiam dominar o cálculo de distância.

Foram testados quatro valores de `K`:

```bash
K = 1
K = 3
K = 5
K = 7
```

O objetivo foi verificar se o aumento do número de vizinhos melhora a estabilidade do classificador ou se suaviza demais a separação entre as classes.

---

## Configuração no Weka

```bash
Scheme: weka.classifiers.lazy.IBk
Distance: weka.core.EuclideanDistance
Search: LinearNNSearch
Test mode: 10-fold cross-validation
Class: environmental_waste_risk_level
```

O algoritmo foi executado com validação cruzada estratificada de 10 folds, mantendo a mesma estratégia usada nos demais classificadores.

---

## Resultados obtidos

### `dataset_preprocessado.arff`

|  K | Acurácia |     Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` |
| -: | -------: | -------: | -----: | -----------: | -------------: | ----------------: | ------------: |
|  1 | 75,5193% | 24,4807% | 0,6236 |        0,757 |          0,728 |             0,698 |         0,892 |
|  3 | 75,0742% | 24,9258% | 0,6146 |        0,752 |          0,791 |             0,637 |         0,861 |
|  5 | 74,0356% | 25,9644% | 0,5985 |        0,741 |          0,791 |             0,605 |         0,867 |
|  7 | 72,1068% | 27,8932% | 0,5693 |        0,723 |          0,728 |             0,625 |         0,861 |

### `dataset_preprocessado_attrselect.arff`

|  K | Acurácia |     Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` |
| -: | -------: | -------: | -----: | -----------: | -------------: | ----------------: | ------------: |
|  1 | 82,9377% | 17,0623% | 0,7384 |        0,829 |          0,806 |             0,766 |         0,968 |
|  3 | 85,0148% | 14,9852% | 0,7701 |        0,850 |          0,836 |             0,790 |         0,968 |
|  5 | 83,3828% | 16,6172% | 0,7451 |        0,834 |          0,813 |             0,778 |         0,956 |
|  7 | 83,2344% | 16,7656% | 0,7430 |        0,832 |          0,813 |             0,778 |         0,949 |

---

## Melhor configuração por dataset

| Dataset                                 | Melhor K | Acurácia | F1 ponderado | Recall `alto` |
| --------------------------------------- | -------: | -------: | -----------: | ------------: |
| `dataset_preprocessado.arff`            |        1 | 75,5193% |        0,757 |         0,892 |
| `dataset_preprocessado_attrselect.arff` |        3 | 85,0148% |        0,850 |         0,968 |

---

## Matrizes de confusão

### `dataset_preprocessado.arff` — melhor configuração: `K = 1`

```bash
   a   b   c   <-- classified as
 195  71   2 |   a = baixo
  67 173   8 |   b = moderado
   1  16 141 |   c = alto
```

### `dataset_preprocessado_attrselect.arff` — melhor configuração: `K = 3`

```bash
   a   b   c   <-- classified as
 224  44   0 |   a = baixo
  45 196   7 |   b = moderado
   0   5 153 |   c = alto
```

---

## Análise dos resultados

No `dataset_preprocessado.arff`, o melhor resultado ocorreu com `K = 1`, alcançando **75,5193%** de acurácia e F1 ponderado de **0,757**.

Esse resultado indica que, no dataset completo, os padrões locais foram mais úteis quando o classificador considerou apenas o vizinho mais próximo. Quando `K` aumentou para 3, 5 e 7, a acurácia caiu gradualmente.

Isso sugere que valores maiores de `K` suavizaram demais a decisão. Em outras palavras, ao considerar mais vizinhos, o modelo passou a misturar regiões próximas de classes diferentes, principalmente entre `baixo` e `moderado`.

A classe `alto` teve bom desempenho em todos os valores de `K`, com recall acima de **0,86**. No melhor caso do dataset completo, com `K = 1`, o recall da classe `alto` foi **0,892**, com 141 acertos em 158 registros.

O principal problema do dataset completo foi a confusão entre `baixo` e `moderado`:

```bash
baixo -> moderado: 71 casos
moderado -> baixo: 67 casos
```

Isso confirma uma dificuldade já observada nas visualizações: as classes `baixo` e `moderado` possuem maior sobreposição.

---

## Análise do dataset com AttributeSelection

No `dataset_preprocessado_attrselect.arff`, o desempenho do IBk melhorou bastante. A melhor configuração foi `K = 3`, com:

```bash
Acurácia: 85,0148%
Kappa: 0,7701
F1 ponderado: 0,850
Recall alto: 0,968
```

Esse resultado mostra que o `AttributeSelection` favoreceu o KNN. Isso é coerente com o funcionamento do algoritmo, porque o KNN é sensível à dimensionalidade e à presença de atributos pouco relevantes ou redundantes.

Ao reduzir o dataset para atributos mais fortes, como:

```bash
water_usage_effectiveness
inlet_temperature_c
gpu_utilization_percent
job_status
rack_power_density_kw
```

a distância entre instâncias ficou mais informativa.

A matriz de confusão da melhor configuração mostra excelente desempenho na classe `alto`:

```bash
alto: 153 acertos de 158
```

Houve apenas 5 registros da classe `alto` classificados como `moderado`, e nenhum classificado como `baixo`. Esse é um ponto positivo importante, pois a classe `alto` representa os casos mais críticos do problema.

---

## Impacto do ajuste de hiperparâmetro

O ajuste de `K` teve impacto claro no comportamento do modelo.

No dataset completo, o melhor valor foi:

```bash
K = 1
```

No dataset com `AttributeSelection`, o melhor valor foi:

```bash
K = 3
```

Isso indica que a melhor configuração do KNN depende da estrutura do dataset. No dataset completo, aumentar `K` prejudicou o desempenho. Já no dataset reduzido, `K = 3` foi melhor que `K = 1`, provavelmente porque reduziu sensibilidade a ruído sem suavizar demais as fronteiras.

---

## Comparação entre os datasets

O `dataset_preprocessado_attrselect.arff` foi claramente melhor para o IBk.

| Critério             | Dataset completo | Dataset com AttributeSelection |
| -------------------- | ---------------: | -----------------------------: |
| Melhor K             |                1 |                              3 |
| Melhor acurácia      |         75,5193% |                       85,0148% |
| Melhor F1 ponderado  |            0,757 |                          0,850 |
| Melhor recall `alto` |            0,892 |                          0,968 |

A diferença de quase 10 pontos percentuais na acurácia mostra que o IBk se beneficiou da redução de atributos.

Isso não significa que o dataset reduzido seja melhor para todos os algoritmos. Significa especificamente que, para um algoritmo baseado em distância, a seleção de atributos reduziu ruído dimensional e melhorou a vizinhança entre instâncias.

---

## Síntese

O `IBk` apresentou desempenho intermediário no dataset completo e desempenho forte no dataset com `AttributeSelection`.

A melhor configuração geral foi:

```bash
dataset_preprocessado_attrselect.arff
K = 3
```

Esse resultado confirma que o KNN é sensível à escolha de atributos e ao valor de `K`.

Para este problema, o IBk funcionou melhor quando aplicado ao dataset reduzido, principalmente porque os atributos mantidos pelo `AttributeSelection` concentram boa parte da separação visual entre as classes.

Apesar do bom desempenho, o IBk ainda deve ser comparado com algoritmos como J48 e Random Forest, que podem lidar melhor com relações não lineares e múltiplas combinações de atributos.

---

## Resposta Weka

### `dataset_preprocessado.arff`

Como as quatro execuções no dataset completo possuem a mesma estrutura de atributos, a saída abaixo foi resumida para evitar repetição. A diferença principal entre elas é o valor de `K`.

#### `K = 1`

```bash
Scheme:weka.classifiers.lazy.IBk -K 1 -W 0 -A "weka.core.neighboursearch.LinearNNSearch -A \"weka.core.EuclideanDistance -R first-last\""
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

=== Classifier model (full training set) ===

IB1 instance-based classifier
using 1 nearest neighbour(s) for classification

=== Summary ===

Correctly Classified Instances         509               75.5193 %
Incorrectly Classified Instances       165               24.4807 %
Kappa statistic                          0.6236
Mean absolute error                      0.1646
Root mean squared error                  0.403 
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.728     0.167      0.741     0.728     0.734      0.787    baixo
                 0.698     0.204      0.665     0.698     0.681      0.746    moderado
                 0.892     0.019      0.934     0.892     0.913      0.932    alto
Weighted Avg.    0.755     0.146      0.759     0.755     0.757      0.806

=== Confusion Matrix ===

   a   b   c   <-- classified as
 195  71   2 |   a = baixo
  67 173   8 |   b = moderado
   1  16 141 |   c = alto
```

#### `K = 3`

```bash
Scheme:weka.classifiers.lazy.IBk -K 3
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

=== Summary ===

Correctly Classified Instances         506               75.0742 %
Incorrectly Classified Instances       168               24.9258 %
Kappa statistic                          0.6146
Mean absolute error                      0.1992
Root mean squared error                  0.3465
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.791     0.222      0.702     0.791     0.744      0.844    baixo
                 0.637     0.171      0.684     0.637     0.66       0.798    moderado
                 0.861     0.01       0.965     0.861     0.91       0.976    alto
Weighted Avg.    0.751     0.153      0.757     0.751     0.752      0.858

=== Confusion Matrix ===

   a   b   c   <-- classified as
 212  56   0 |   a = baixo
  85 158   5 |   b = moderado
   5  17 136 |   c = alto
```

#### `K = 5`

```bash
Scheme:weka.classifiers.lazy.IBk -K 5
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

=== Summary ===

Correctly Classified Instances         499               74.0356 %
Incorrectly Classified Instances       175               25.9644 %
Kappa statistic                          0.5985
Mean absolute error                      0.2135
Root mean squared error                  0.3403
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.791     0.236      0.688     0.791     0.736      0.856    baixo
                 0.605     0.174      0.67      0.605     0.636      0.806    moderado
                 0.867     0.01       0.965     0.867     0.913      0.981    alto
Weighted Avg.    0.74      0.16       0.746     0.74      0.741      0.867

=== Confusion Matrix ===

   a   b   c   <-- classified as
 212  56   0 |   a = baixo
  93 150   5 |   b = moderado
   3  18 137 |   c = alto
```

#### `K = 7`

```bash
Scheme:weka.classifiers.lazy.IBk -K 7
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

=== Summary ===

Correctly Classified Instances         486               72.1068 %
Incorrectly Classified Instances       188               27.8932 %
Kappa statistic                          0.5693
Mean absolute error                      0.2203
Root mean squared error                  0.3354
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.728     0.219      0.687     0.728     0.707      0.866    baixo
                 0.625     0.218      0.625     0.625     0.625      0.816    moderado
                 0.861     0.012      0.958     0.861     0.907      0.982    alto
Weighted Avg.    0.721     0.17       0.728     0.721     0.723      0.875

=== Confusion Matrix ===

   a   b   c   <-- classified as
 195  73   0 |   a = baixo
  87 155   6 |   b = moderado
   2  20 136 |   c = alto
```

---

### `dataset_preprocessado_attrselect.arff`

A versão com `AttributeSelection` possui 6 atributos. O comportamento do IBk melhorou nessa versão, principalmente para `K = 3`.

#### `K = 1`

```bash
Scheme:weka.classifiers.lazy.IBk -K 1
Instances:    674
Attributes:   6
Test mode:10-fold cross-validation

=== Summary ===

Correctly Classified Instances         559               82.9377 %
Incorrectly Classified Instances       115               17.0623 %
Kappa statistic                          0.7384
Mean absolute error                      0.1154
Root mean squared error                  0.3364
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.806     0.123      0.812     0.806     0.809      0.854    baixo
                 0.766     0.134      0.769     0.766     0.768      0.822    moderado
                 0.968     0.016      0.95      0.968     0.959      0.974    alto
Weighted Avg.    0.829     0.102      0.829     0.829     0.829      0.87 

=== Confusion Matrix ===

   a   b   c   <-- classified as
 216  52   0 |   a = baixo
  50 190   8 |   b = moderado
   0   5 153 |   c = alto
```

#### `K = 3`

```bash
Scheme:weka.classifiers.lazy.IBk -K 3
Instances:    674
Attributes:   6
Test mode:10-fold cross-validation

=== Summary ===

Correctly Classified Instances         573               85.0148 %
Incorrectly Classified Instances       101               14.9852 %
Kappa statistic                          0.7701
Mean absolute error                      0.1229
Root mean squared error                  0.279 
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.836     0.111      0.833     0.836     0.834      0.928    baixo
                 0.79      0.115      0.8       0.79      0.795      0.899    moderado
                 0.968     0.014      0.956     0.968     0.962      0.991    alto
Weighted Avg.    0.85      0.09       0.85      0.85      0.85       0.932

=== Confusion Matrix ===

   a   b   c   <-- classified as
 224  44   0 |   a = baixo
  45 196   7 |   b = moderado
   0   5 153 |   c = alto
```

#### `K = 5`

```bash
Scheme:weka.classifiers.lazy.IBk -K 5
Instances:    674
Attributes:   6
Test mode:10-fold cross-validation

=== Summary ===

Correctly Classified Instances         562               83.3828 %
Incorrectly Classified Instances       112               16.6172 %
Kappa statistic                          0.7451
Mean absolute error                      0.1344
Root mean squared error                  0.2704
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.813     0.116      0.823     0.813     0.818      0.946    baixo
                 0.778     0.134      0.772     0.778     0.775      0.915    moderado
                 0.956     0.016      0.95      0.956     0.953      0.995    alto
Weighted Avg.    0.834     0.099      0.834     0.834     0.834      0.946

=== Confusion Matrix ===

   a   b   c   <-- classified as
 218  50   0 |   a = baixo
  47 193   8 |   b = moderado
   0   7 151 |   c = alto
```

#### `K = 7`

```bash
Scheme:weka.classifiers.lazy.IBk -K 7
Instances:    674
Attributes:   6
Test mode:10-fold cross-validation

=== Summary ===

Correctly Classified Instances         561               83.2344 %
Incorrectly Classified Instances       113               16.7656 %
Kappa statistic                          0.743 
Mean absolute error                      0.1445
Root mean squared error                  0.2728
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.813     0.108      0.832     0.813     0.823      0.947    baixo
                 0.778     0.136      0.769     0.778     0.774      0.916    moderado
                 0.949     0.021      0.932     0.949     0.94       0.996    alto
Weighted Avg.    0.832     0.098      0.832     0.832     0.832      0.947

=== Confusion Matrix ===

   a   b   c   <-- classified as
 218  50   0 |   a = baixo
  44 193  11 |   b = moderado
   0   8 150 |   c = alto
```
