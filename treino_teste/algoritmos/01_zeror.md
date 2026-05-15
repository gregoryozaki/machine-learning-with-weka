# ZeroR

## Funcionamento do algoritmo

O `ZeroR` é o classificador mais simples usado nesta etapa. Ele ignora todos os atributos preditores e sempre prediz a classe majoritária do conjunto de treinamento.

Neste experimento, a classe majoritária foi:

```bash
baixo
```

Por isso, o modelo classificou todas as instâncias como `baixo`, tanto no `dataset_preprocessado.arff` quanto no `dataset_preprocessado_attrselect.arff`.

---

## Configuração no Weka

```bash
Scheme: weka.classifiers.rules.ZeroR
Test mode: 10-fold cross-validation
Class: environmental_waste_risk_level
```

O algoritmo foi executado com validação cruzada estratificada de 10 folds, mantendo a mesma estratégia de avaliação usada para os demais modelos.

---

## Resultados obtidos

| Dataset                                 | Instâncias | Atributos | Acurácia |     Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` |
| --------------------------------------- | ---------: | --------: | -------: | -------: | -----: | -----------: | -------------: | ----------------: | ------------: |
| `dataset_preprocessado.arff`            |        674 |        27 | 39,7626% | 60,2374% | 0,0000 |        0,226 |          1,000 |             0,000 |         0,000 |
| `dataset_preprocessado_attrselect.arff` |        674 |         6 | 39,7626% | 60,2374% | 0,0000 |        0,226 |          1,000 |             0,000 |         0,000 |

---

## Matriz de confusão

### `dataset_preprocessado.arff`

```bash
   a   b   c   <-- classified as
 268   0   0 |   a = baixo
 248   0   0 |   b = moderado
 158   0   0 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

```bash
   a   b   c   <-- classified as
 268   0   0 |   a = baixo
 248   0   0 |   b = moderado
 158   0   0 |   c = alto
```

---

## Análise dos resultados

O `ZeroR` obteve acurácia de **39,7626%** nos dois datasets. Esse valor corresponde exatamente à proporção da classe majoritária `baixo` no conjunto de dados:

```bash
268 / 674 = 39,7626%
```

Isso confirma que o modelo não aprendeu nenhum padrão real dos atributos. Ele apenas repetiu a classe mais frequente.

A métrica `Kappa statistic` foi igual a **0**, indicando ausência de concordância além do acaso. Esse resultado é esperado, pois o `ZeroR` não utiliza nenhuma informação dos atributos para tomar decisão.

A matriz de confusão mostra o principal problema do modelo: todas as instâncias foram classificadas como `baixo`. Com isso, o modelo acertou os 268 registros da classe `baixo`, mas errou todos os registros das classes `moderado` e `alto`.

Esse comportamento aparece também nas métricas por classe:

* `baixo`: recall igual a **1,000**, pois todos os registros da classe foram classificados como `baixo`;
* `moderado`: recall igual a **0,000**, pois nenhum registro foi classificado como `moderado`;
* `alto`: recall igual a **0,000**, pois nenhum registro foi classificado como `alto`.

Para o problema estudado, isso é crítico. A classe `alto` representa maior risco de desperdício ambiental, e o `ZeroR` não identificou nenhum caso dessa classe.

---

## Comparação entre os datasets

O desempenho foi idêntico nos dois datasets porque o `ZeroR` ignora os atributos. Portanto, a redução de atributos feita pelo `AttributeSelection` não altera o resultado.

Isso confirma que o `ZeroR` serve apenas como linha de base mínima. Ele não deve ser interpretado como modelo útil para classificação, mas como referência para verificar se os outros algoritmos realmente conseguem aprender padrões acima da simples escolha da classe majoritária.

---

## Síntese

O `ZeroR` apresentou o pior comportamento entre os modelos avaliados, mas cumpre papel importante como baseline. Sua acurácia de **39,7626%** representa o desempenho mínimo esperado ao sempre escolher a classe majoritária.

Como o modelo não identificou nenhuma instância das classes `moderado` e `alto`, ele é inadequado para o problema de classificação do risco de desperdício ambiental.

Qualquer algoritmo útil deverá superar o `ZeroR`, principalmente nas métricas de recall e F1-score da classe `alto`.

---

## Resposta Weka

### `dataset_preprocessado.arff`

```bash
=== Run information ===

Scheme:weka.classifiers.rules.ZeroR 
Relation:     datacenter_ai_environmental_waste_risk_v2-weka.filters.unsupervised.attribute.ReplaceMissingValues-weka.filters.unsupervised.attribute.Remove-R27-29-weka.filters.unsupervised.attribute.NumericToNominal-R16-weka.filters.unsupervised.attribute.RemoveUseless-M99.0-weka.filters.unsupervised.attribute.Normalize-S1.0-T0.0
Instances:    674
Attributes:   27
              active_power_w
              energy_consumption_kwh
              water_usage_effectiveness
              carbon_intensity_gco2_kwh
              inlet_temperature_c
              exhaust_temperature_c
              delta_t_c
              fan_speed_rpm
              cooling_method
              cpu_utilization_percent
              memory_utilization_percent
              gpu_power_w
              gpu_utilization_percent
              gpu_temperature_c
              gpu_core_frequency_mhz
              num_gpus
              ai_workload_type
              batch_size
              num_epochs
              model_parameter_size_million
              training_samples
              job_duration_hours
              job_status
              rack_power_density_kw
              gpu_sharing_mode
              power_cap_w
              environmental_waste_risk_level
Test mode:10-fold cross-validation

=== Classifier model (full training set) ===

ZeroR predicts class value: baixo

Time taken to build model: 0 seconds

=== Stratified cross-validation ===
=== Summary ===

Correctly Classified Instances         268               39.7626 %
Incorrectly Classified Instances       406               60.2374 %
Kappa statistic                          0     
Mean absolute error                      0.4344
Root mean squared error                  0.466 
Relative absolute error                100      %
Root relative squared error            100      %
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 1         1          0.398     1         0.569      0.494    baixo
                 0         0          0         0         0          0.494    moderado
                 0         0          0         0         0          0.493    alto
Weighted Avg.    0.398     0.398      0.158     0.398     0.226      0.494

=== Confusion Matrix ===

   a   b   c   <-- classified as
 268   0   0 |   a = baixo
 248   0   0 |   b = moderado
 158   0   0 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

A saída do Weka para o dataset com `AttributeSelection` apresentou os mesmos resultados quantitativos do dataset principal. A diferença está apenas na quantidade de atributos: a versão reduzida possui 6 atributos em vez de 27.

```bash
=== Run information ===

Scheme:weka.classifiers.rules.ZeroR 
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

ZeroR predicts class value: baixo

=== Summary ===

Correctly Classified Instances         268               39.7626 %
Incorrectly Classified Instances       406               60.2374 %
Kappa statistic                          0     
Mean absolute error                      0.4344
Root mean squared error                  0.466 
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 1         1          0.398     1         0.569      0.494    baixo
                 0         0          0         0         0          0.494    moderado
                 0         0          0         0         0          0.493    alto
Weighted Avg.    0.398     0.398      0.158     0.398     0.226      0.494

=== Confusion Matrix ===

   a   b   c   <-- classified as
 268   0   0 |   a = baixo
 248   0   0 |   b = moderado
 158   0   0 |   c = alto
```