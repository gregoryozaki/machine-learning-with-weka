# Naive Bayes

## Funcionamento do algoritmo

O `NaiveBayes` é um classificador probabilístico baseado no Teorema de Bayes. Ele estima a probabilidade de uma instância pertencer a cada classe a partir dos valores dos atributos.

Uma característica importante do algoritmo é a suposição de independência condicional entre os atributos. Ou seja, ele assume que os atributos contribuem de forma independente para a classe, mesmo que na prática existam relações entre eles.

No contexto deste dataset, essa suposição é uma limitação, pois algumas variáveis possuem relação forte entre si, como:

```bash
active_power_w
energy_consumption_kwh
```

e também:

```bash
gpu_temperature_c
fan_speed_rpm
```

Mesmo assim, o `NaiveBayes` é útil como baseline probabilístico, permitindo verificar se uma abordagem simples baseada em probabilidades consegue capturar padrões relevantes do dataset.

---

## Configuração no Weka

```bash
Scheme: weka.classifiers.bayes.NaiveBayes
Test mode: 10-fold cross-validation
Class: environmental_waste_risk_level
```

O algoritmo foi executado com validação cruzada estratificada de 10 folds, mantendo a mesma estratégia de avaliação usada para os demais modelos.

---

## Resultados obtidos

| Dataset                                 | Instâncias | Atributos | Acurácia |     Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` |
| --------------------------------------- | ---------: | --------: | -------: | -------: | -----: | -----------: | -------------: | ----------------: | ------------: |
| `dataset_preprocessado.arff`            |        674 |        27 | 67,6558% | 32,3442% | 0,5063 |        0,676 |          0,526 |             0,706 |         0,886 |
| `dataset_preprocessado_attrselect.arff` |        674 |         6 | 74,7774% | 25,2226% | 0,6111 |        0,743 |          0,840 |             0,552 |         0,899 |

---

## Matriz de confusão

### `dataset_preprocessado.arff`

```bash
   a   b   c   <-- classified as
 141 127   0 |   a = baixo
  53 175  20 |   b = moderado
   1  17 140 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

```bash
   a   b   c   <-- classified as
 225  42   1 |   a = baixo
  99 137  12 |   b = moderado
   0  16 142 |   c = alto
```

---

## Análise dos resultados

No `dataset_preprocessado.arff`, o `NaiveBayes` obteve acurácia de **67,6558%**, com Kappa de **0,5063** e F1 ponderado de **0,676**.

Esse desempenho ficou próximo ao `OneR`, mas com comportamento diferente entre as classes. O `NaiveBayes` teve boa capacidade de identificar a classe `alto`, alcançando recall de **0,886**, mas teve dificuldade na classe `baixo`, com recall de apenas **0,526**.

A matriz de confusão mostra que o principal erro no dataset completo foi classificar registros `baixo` como `moderado`:

```bash
baixo -> moderado: 127 casos
```

Isso indica que, para o Naive Bayes, as classes `baixo` e `moderado` ficaram bastante sobrepostas. Esse comportamento é compatível com a análise visual anterior, em que essas duas classes apareciam menos separadas do que a classe `alto`.

A classe `alto`, por outro lado, foi bem recuperada:

```bash
alto: 140 acertos de 158
```

Isso é positivo, pois `alto` representa os casos mais críticos de risco de desperdício ambiental.

---

## Análise do dataset com AttributeSelection

No `dataset_preprocessado_attrselect.arff`, o desempenho do `NaiveBayes` melhorou de forma clara.

A acurácia passou de:

```bash
67,6558% -> 74,7774%
```

O Kappa também aumentou:

```bash
0,5063 -> 0,6111
```

E o F1 ponderado subiu de:

```bash
0,676 -> 0,743
```

Esse resultado indica que o `AttributeSelection` favoreceu o `NaiveBayes`. Isso faz sentido porque o algoritmo assume independência entre atributos, e o dataset completo possui várias relações e possíveis redundâncias entre variáveis.

Ao reduzir o conjunto para atributos mais fortes:

```bash
water_usage_effectiveness
inlet_temperature_c
gpu_utilization_percent
job_status
rack_power_density_kw
```

o modelo ficou menos exposto a atributos redundantes ou correlacionados.

No dataset reduzido, a classe `baixo` melhorou bastante:

```bash
recall baixo: 0,526 -> 0,840
```

A classe `alto` também melhorou levemente:

```bash
recall alto: 0,886 -> 0,899
```

O ponto negativo foi a queda no recall da classe `moderado`:

```bash
recall moderado: 0,706 -> 0,552
```

Isso mostra que a seleção de atributos ajudou o modelo a separar melhor `baixo` e `alto`, mas prejudicou a identificação da classe intermediária.

---

## Comparação entre os datasets

| Critério          | Dataset completo | Dataset com AttributeSelection |
| ----------------- | ---------------: | -----------------------------: |
| Acurácia          |         67,6558% |                       74,7774% |
| Kappa             |           0,5063 |                         0,6111 |
| F1 ponderado      |            0,676 |                          0,743 |
| Recall `baixo`    |            0,526 |                          0,840 |
| Recall `moderado` |            0,706 |                          0,552 |
| Recall `alto`     |            0,886 |                          0,899 |

O dataset com `AttributeSelection` foi superior em desempenho geral. Isso indica que a redução de atributos foi benéfica para o `NaiveBayes`.

Porém, a melhoria não foi uniforme entre as classes. O modelo passou a identificar melhor `baixo` e `alto`, mas perdeu desempenho em `moderado`, que é justamente a classe intermediária e mais sujeita à sobreposição.

---

## Síntese

O `NaiveBayes` apresentou desempenho moderado no dataset completo e melhorou no dataset com `AttributeSelection`.

A melhor configuração para esse algoritmo foi:

```bash
dataset_preprocessado_attrselect.arff
```

com acurácia de **74,7774%** e F1 ponderado de **0,743**.

O resultado confirma que o `NaiveBayes` é sensível à presença de atributos correlacionados. Como o dataset completo possui relações fortes entre variáveis energéticas, térmicas e operacionais, a redução de atributos ajudou o modelo.

Apesar da melhora, o `NaiveBayes` ainda não é o modelo mais forte. Ele funciona bem como baseline probabilístico e apresenta bom recall para a classe `alto`, mas sua dificuldade com a classe `moderado` limita seu desempenho geral.

---

## Resposta Weka

### `dataset_preprocessado.arff`

A saída completa do Weka para o dataset principal inclui a distribuição probabilística estimada para todos os atributos. Para evitar excesso de repetição, foram preservados abaixo os principais atributos interpretáveis e os resultados de avaliação.

```bash
=== Run information ===

Scheme:weka.classifiers.bayes.NaiveBayes 
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

=== Classifier model (full training set) ===

Naive Bayes Classifier

                                  Class
Attribute                         baixo moderado     alto
                                  (0.4)   (0.37)   (0.23)
==========================================================

active_power_w
  mean                            0.3651   0.4295   0.6678
  std. dev.                       0.2439   0.2404   0.1886

energy_consumption_kwh
  mean                            0.3642   0.4285    0.655
  std. dev.                       0.2432    0.238   0.1842

water_usage_effectiveness
  mean                            0.1239   0.2121   0.3529
  std. dev.                       0.0885   0.1102   0.1361

inlet_temperature_c
  mean                            0.3242   0.4434   0.6842
  std. dev.                       0.1387    0.114   0.1517

gpu_utilization_percent
  mean                            0.6882   0.6062   0.3436
  std. dev.                       0.2848   0.1699   0.1257

job_status
  success                          252.0    215.0     47.0
  failed                             2.0     10.0     49.0
  aborted                            3.0      5.0     31.0
  running                           15.0     22.0     35.0

rack_power_density_kw
  mean                            0.0675   0.1186   0.5255
  std. dev.                       0.0595   0.1166   0.2647

power_cap_w
  mean                            0.4464    0.533   0.8019
  std. dev.                       0.2824   0.2818   0.1762

Time taken to build model: 0.01 seconds

=== Stratified cross-validation ===
=== Summary ===

Correctly Classified Instances         456               67.6558 %
Incorrectly Classified Instances       218               32.3442 %
Kappa statistic                          0.5063
Mean absolute error                      0.213 
Root mean squared error                  0.4181
Relative absolute error                 49.0393 %
Root relative squared error             89.7113 %
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.526     0.133      0.723     0.526     0.609      0.848    baixo
                 0.706     0.338      0.549     0.706     0.617      0.771    moderado
                 0.886     0.039      0.875     0.886     0.881      0.99     alto
Weighted Avg.    0.677     0.186      0.694     0.677     0.676      0.853

=== Confusion Matrix ===

   a   b   c   <-- classified as
 141 127   0 |   a = baixo
  53 175  20 |   b = moderado
   1  17 140 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

A saída do Weka para a versão com `AttributeSelection` é mais compacta porque o dataset possui apenas 6 atributos. O modelo foi treinado com os atributos selecionados pelo filtro.

```bash
=== Run information ===

Scheme:weka.classifiers.bayes.NaiveBayes 
Instances:    674
Attributes:   6
              water_usage_effectiveness
              inlet_temperature_c
              gpu_utilization_percent
              job_status
              rack_power_density_kw
              environmental_waste_risk_level
Test mode:10-fold cross-validation

=== Classifier model (full training set) ===

Naive Bayes Classifier

                               Class
Attribute                      baixo moderado     alto
                               (0.4)   (0.37)   (0.23)
=======================================================

water_usage_effectiveness
  mean                         0.1239   0.2121   0.3529
  std. dev.                    0.0885   0.1102   0.1361

inlet_temperature_c
  mean                         0.3242   0.4434   0.6842
  std. dev.                    0.1387    0.114   0.1517

gpu_utilization_percent
  mean                         0.6882   0.6062   0.3436
  std. dev.                    0.2848   0.1699   0.1257

job_status
  success                       252.0    215.0     47.0
  failed                          2.0     10.0     49.0
  aborted                         3.0      5.0     31.0
  running                        15.0     22.0     35.0

rack_power_density_kw
  mean                         0.0675   0.1186   0.5255
  std. dev.                    0.0595   0.1166   0.2647

Time taken to build model: 0 seconds

=== Stratified cross-validation ===
=== Summary ===

Correctly Classified Instances         504               74.7774 %
Incorrectly Classified Instances       170               25.2226 %
Kappa statistic                          0.6111
Mean absolute error                      0.1925
Root mean squared error                  0.3391
Relative absolute error                 44.3003 %
Root relative squared error             72.7653 %
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.84      0.244      0.694     0.84      0.76       0.887    baixo
                 0.552     0.136      0.703     0.552     0.619      0.832    moderado
                 0.899     0.025      0.916     0.899     0.907      0.99     alto
Weighted Avg.    0.748     0.153      0.749     0.748     0.743      0.891

=== Confusion Matrix ===

   a   b   c   <-- classified as
 225  42   1 |   a = baixo
  99 137  12 |   b = moderado
   0  16 142 |   c = alto
```
