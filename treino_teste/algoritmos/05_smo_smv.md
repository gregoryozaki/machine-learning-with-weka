# SMO / SVM

## Funcionamento do algoritmo

O `SMO` é a implementação do SVM no Weka. O SVM busca construir fronteiras de decisão que separem as classes com a maior margem possível.

Neste experimento, o SMO foi executado com kernel polinomial de grau 1, que corresponde a uma separação linear:

```bash
PolyKernel -E 1.0
```

Na saída do Weka, isso aparece como:

```bash
Linear Kernel: K(x,y) = <x,y>
```

Como o SVM é sensível à escala dos atributos, a normalização aplicada no pré-processamento foi importante para evitar que atributos com valores originalmente maiores dominassem a margem de separação.

---

## Configuração no Weka

```bash
Scheme: weka.classifiers.functions.SMO
Kernel: PolyKernel -E 1.0
C: 1.0
Test mode: 10-fold cross-validation
Class: environmental_waste_risk_level
```

O algoritmo foi executado com validação cruzada estratificada de 10 folds, mantendo a mesma estratégia de avaliação usada para os demais classificadores.

---

## Resultados obtidos

| Dataset                                 | Instâncias | Atributos | Acurácia |     Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` |
| --------------------------------------- | ---------: | --------: | -------: | -------: | -----: | -----------: | -------------: | ----------------: | ------------: |
| `dataset_preprocessado.arff`            |        674 |        27 | 80,8605% | 19,1395% | 0,7065 |        0,808 |          0,769 |             0,742 |         0,981 |
| `dataset_preprocessado_attrselect.arff` |        674 |         6 | 76,7062% | 23,2938% | 0,6403 |        0,761 |          0,873 |             0,560 |         0,911 |

---

## Matriz de confusão

### `dataset_preprocessado.arff`

```bash
   a   b   c   <-- classified as
 206  62   0 |   a = baixo
  59 184   5 |   b = moderado
   1   2 155 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

```bash
   a   b   c   <-- classified as
 234  34   0 |   a = baixo
 100 139   9 |   b = moderado
   0  14 144 |   c = alto
```

---

## Análise dos resultados

No `dataset_preprocessado.arff`, o SMO obteve acurácia de **80,8605%**, Kappa de **0,7065** e F1 ponderado de **0,808**. Esse é um resultado forte, especialmente considerando que foi usado um kernel linear.

O melhor comportamento ocorreu na classe `alto`, com recall de **0,981**. Isso significa que o modelo identificou quase todos os casos críticos:

```bash
alto: 155 acertos de 158
```

Esse resultado é importante para o problema, pois a classe `alto` representa maior risco de desperdício ambiental. O modelo praticamente não classificou registros `alto` como `baixo`, cometendo apenas:

```bash
alto -> baixo: 1 caso
alto -> moderado: 2 casos
```

O principal problema ocorreu entre as classes `baixo` e `moderado`:

```bash
baixo -> moderado: 62 casos
moderado -> baixo: 59 casos
```

Isso indica que o SMO conseguiu separar bem a classe `alto`, mas teve dificuldade na fronteira entre as classes de menor risco. Esse comportamento é coerente com as visualizações anteriores, em que `baixo` e `moderado` apresentaram maior sobreposição.

---

## Análise do dataset com AttributeSelection

No `dataset_preprocessado_attrselect.arff`, o desempenho geral caiu para **76,7062%** de acurácia, com Kappa de **0,6403** e F1 ponderado de **0,761**.

A redução de atributos melhorou o recall da classe `baixo`:

```bash
recall baixo: 0,769 -> 0,873
```

Porém, piorou bastante o recall da classe `moderado`:

```bash
recall moderado: 0,742 -> 0,560
```

A classe `alto` continuou com bom desempenho, mas também caiu:

```bash
recall alto: 0,981 -> 0,911
```

A matriz de confusão mostra que a principal perda ocorreu na classe `moderado`, com muitos registros classificados como `baixo`:

```bash
moderado -> baixo: 100 casos
```

Isso sugere que o `AttributeSelection` removeu atributos que ajudavam o SMO a separar melhor a classe intermediária. Apesar de o dataset reduzido manter atributos fortes, como `gpu_utilization_percent` e `rack_power_density_kw`, ele perdeu parte da informação complementar necessária para definir melhor as fronteiras entre `baixo` e `moderado`.

---

## Interpretação dos pesos do modelo

A saída do SMO mostra pesos para os atributos nos classificadores binários entre pares de classes.

No dataset completo, alguns atributos aparecem com peso relevante em diferentes separações, como:

```bash
gpu_utilization_percent
rack_power_density_kw
water_usage_effectiveness
inlet_temperature_c
power_cap_w
job_duration_hours
```

No dataset com `AttributeSelection`, os pesos se concentram principalmente em:

```bash
water_usage_effectiveness
inlet_temperature_c
gpu_utilization_percent
rack_power_density_kw
job_status
```

Isso é coerente com a etapa de visualização. Os atributos `gpu_utilization_percent` e `rack_power_density_kw` aparecem novamente como centrais para separar as classes, especialmente a classe `alto`.

---

## Comparação entre os datasets

| Critério          | Dataset completo | Dataset com AttributeSelection |
| ----------------- | ---------------: | -----------------------------: |
| Acurácia          |         80,8605% |                       76,7062% |
| Kappa             |           0,7065 |                         0,6403 |
| F1 ponderado      |            0,808 |                          0,761 |
| Recall `baixo`    |            0,769 |                          0,873 |
| Recall `moderado` |            0,742 |                          0,560 |
| Recall `alto`     |            0,981 |                          0,911 |

O dataset completo foi melhor para o SMO em desempenho geral e, principalmente, na identificação da classe `alto`.

A versão com `AttributeSelection` simplificou o espaço de atributos, mas prejudicou a separação da classe `moderado`. Isso indica que, para o SMO, a redução automática de atributos removeu informações úteis para definir melhor as margens entre as classes.

---

## Síntese

O `SMO` apresentou bom desempenho, principalmente no `dataset_preprocessado.arff`.

A melhor configuração para esse algoritmo foi:

```bash
dataset_preprocessado.arff
SMO com PolyKernel -E 1.0
```

com:

```bash
Acurácia: 80,8605%
F1 ponderado: 0,808
Recall alto: 0,981
```

O modelo foi especialmente eficiente na identificação da classe `alto`, o que é positivo para o problema de risco ambiental. Porém, teve dificuldade maior na separação entre `baixo` e `moderado`.

O `AttributeSelection` não favoreceu o SMO neste caso, pois reduziu o desempenho geral e prejudicou a classe intermediária.

---

## Resposta Weka

### `dataset_preprocessado.arff`

A saída completa do Weka para o SMO é extensa porque o algoritmo gera classificadores binários entre pares de classes. Para evitar excesso de repetição, foram preservados abaixo a configuração, os principais pesos interpretáveis e os resultados de avaliação.

```bash
=== Run information ===

Scheme:weka.classifiers.functions.SMO -C 1.0 -L 0.001 -P 1.0E-12 -N 0 -V -1 -W 1 -K "weka.classifiers.functions.supportVector.PolyKernel -C 250007 -E 1.0"
Instances:    674
Attributes:   27
Test mode:10-fold cross-validation

=== Classifier model (full training set) ===

SMO

Kernel used:
  Linear Kernel: K(x,y) = <x,y>

Classifier for classes: baixo, moderado

BinarySMO

Machine linear: showing attribute weights, not support vectors.

        -0.531  * (normalized) active_power_w
 +      -0.5276 * (normalized) energy_consumption_kwh
 +       2.2657 * (normalized) water_usage_effectiveness
 +       2.0022 * (normalized) inlet_temperature_c
 +      -4.135  * (normalized) gpu_utilization_percent
 +       2.7985 * (normalized) rack_power_density_kw
 +       1.15   * (normalized) power_cap_w
 -       0.1812

Classifier for classes: baixo, alto

BinarySMO

Machine linear: showing attribute weights, not support vectors.

         0.1549 * (normalized) active_power_w
 +       0.1119 * (normalized) energy_consumption_kwh
 +       0.8667 * (normalized) inlet_temperature_c
 +       1.2014 * (normalized) delta_t_c
 +      -1.3533 * (normalized) gpu_utilization_percent
 +       0.961  * (normalized) gpu_temperature_c
 +       1.7726 * (normalized) rack_power_density_kw
 +       1.0952 * (normalized) power_cap_w
 -       1.6553

Classifier for classes: moderado, alto

BinarySMO

Machine linear: showing attribute weights, not support vectors.

         0.4919 * (normalized) active_power_w
 +       0.8908 * (normalized) carbon_intensity_gco2_kwh
 +       1.5833 * (normalized) inlet_temperature_c
 +       1.3756 * (normalized) gpu_temperature_c
 +      -1.4137 * (normalized) gpu_utilization_percent
 +       0.9623 * (normalized) job_duration_hours
 +       2.3256 * (normalized) rack_power_density_kw
 +       1.8446 * (normalized) power_cap_w
 -       3.2832

Time taken to build model: 0.09 seconds

=== Stratified cross-validation ===
=== Summary ===

Correctly Classified Instances         545               80.8605 %
Incorrectly Classified Instances       129               19.1395 %
Kappa statistic                          0.7065
Mean absolute error                      0.2648
Root mean squared error                  0.3412
Relative absolute error                 60.9439 %
Root relative squared error             73.2033 %
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.769     0.148      0.774     0.769     0.772      0.865    baixo
                 0.742     0.15       0.742     0.742     0.742      0.796    moderado
                 0.981     0.01       0.969     0.981     0.975      0.995    alto
Weighted Avg.    0.809     0.116      0.808     0.809     0.808      0.87 

=== Confusion Matrix ===

   a   b   c   <-- classified as
 206  62   0 |   a = baixo
  59 184   5 |   b = moderado
   1   2 155 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

A versão com `AttributeSelection` possui 6 atributos. A saída do SMO ficou mais compacta porque o modelo foi treinado apenas com os atributos selecionados.

```bash
=== Run information ===

Scheme:weka.classifiers.functions.SMO -C 1.0 -L 0.001 -P 1.0E-12 -N 0 -V -1 -W 1 -K "weka.classifiers.functions.supportVector.PolyKernel -C 250007 -E 1.0"
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

SMO

Kernel used:
  Linear Kernel: K(x,y) = <x,y>

Classifier for classes: baixo, moderado

BinarySMO

Machine linear: showing attribute weights, not support vectors.

         4.1102 * (normalized) water_usage_effectiveness
 +       4.1818 * (normalized) inlet_temperature_c
 +      -2.2724 * (normalized) gpu_utilization_percent
 +      -0.4278 * (normalized) job_status=success
 +       0.4278 * (normalized) job_status=failed
 +       2.3493 * (normalized) rack_power_density_kw
 -       0.8143

Classifier for classes: baixo, alto

BinarySMO

Machine linear: showing attribute weights, not support vectors.

         0.9306 * (normalized) water_usage_effectiveness
 +       3.154  * (normalized) inlet_temperature_c
 +      -2.0784 * (normalized) gpu_utilization_percent
 +      -0.4914 * (normalized) job_status=success
 +       3.4329 * (normalized) rack_power_density_kw
 -       1.1941

Classifier for classes: moderado, alto

BinarySMO

Machine linear: showing attribute weights, not support vectors.

         0.7607 * (normalized) water_usage_effectiveness
 +       2.8161 * (normalized) inlet_temperature_c
 +      -3.7005 * (normalized) gpu_utilization_percent
 +      -0.6247 * (normalized) job_status=success
 +       3.5351 * (normalized) rack_power_density_kw
 -       0.9464

Time taken to build model: 0.03 seconds

=== Stratified cross-validation ===
=== Summary ===

Correctly Classified Instances         517               76.7062 %
Incorrectly Classified Instances       157               23.2938 %
Kappa statistic                          0.6403
Mean absolute error                      0.2753
Root mean squared error                  0.3566
Relative absolute error                 63.3725 %
Root relative squared error             76.5155 %
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.873     0.246      0.701     0.873     0.777      0.843    baixo
                 0.56      0.113      0.743     0.56      0.639      0.724    moderado
                 0.911     0.017      0.941     0.911     0.926      0.978    alto
Weighted Avg.    0.767     0.143      0.773     0.767     0.761      0.831

=== Confusion Matrix ===

   a   b   c   <-- classified as
 234  34   0 |   a = baixo
 100 139   9 |   b = moderado
   0  14 144 |   c = alto
```
