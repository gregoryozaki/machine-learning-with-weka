# OneR

## Funcionamento do algoritmo

O `OneR` é um classificador simples baseado em regras. Ele avalia os atributos disponíveis e escolhe aquele que gera a menor taxa de erro, criando uma regra de classificação baseada em apenas um atributo.

Neste experimento, o atributo selecionado pelo algoritmo foi:

```bash
gpu_utilization_percent
````

Isso significa que o `OneR` considerou a utilização da GPU como o atributo individual mais informativo para prever a classe `environmental_waste_risk_level`.

A regra gerada foi:

```bash
gpu_utilization_percent:
	< 0.15760849999999998	-> baixo
	< 0.4836955	-> alto
	< 0.7663044999999999	-> moderado
	< 0.8097825	-> baixo
	< 0.8206519999999999	-> moderado
	>= 0.8206519999999999	-> baixo
```

A regra acertou:

```bash
491/674 instances correct
```

no treinamento completo usado para gerar o modelo.

---

## Configuração no Weka

```bash
Scheme: weka.classifiers.rules.OneR -B 6
Test mode: 10-fold cross-validation
Class: environmental_waste_risk_level
```

O parâmetro `-B 6` indica o número mínimo de instâncias por intervalo usado pelo `OneR` ao discretizar atributos numéricos.

O algoritmo foi executado com validação cruzada estratificada de 10 folds, mantendo a mesma estratégia de avaliação usada para os demais modelos.

---

## Resultados obtidos

| Dataset                                 | Instâncias | Atributos | Acurácia |     Erro |  Kappa | F1 ponderado | Recall `baixo` | Recall `moderado` | Recall `alto` |
| --------------------------------------- | ---------: | --------: | -------: | -------: | -----: | -----------: | -------------: | ----------------: | ------------: |
| `dataset_preprocessado.arff`            |        674 |        27 | 68,3976% | 31,6024% | 0,5175 |        0,683 |          0,683 |             0,617 |         0,791 |
| `dataset_preprocessado_attrselect.arff` |        674 |         6 | 68,3976% | 31,6024% | 0,5175 |        0,683 |          0,683 |             0,617 |         0,791 |

---

## Matriz de confusão

### `dataset_preprocessado.arff`

```bash
   a   b   c   <-- classified as
 183  77   8 |   a = baixo
  56 153  39 |   b = moderado
  14  19 125 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

```bash
   a   b   c   <-- classified as
 183  77   8 |   a = baixo
  56 153  39 |   b = moderado
  14  19 125 |   c = alto
```

---

## Análise dos resultados

O `OneR` obteve acurácia de **68,3976%** nos dois datasets. Esse desempenho é bem superior ao `ZeroR`, que obteve apenas **39,7626%**, mostrando que o uso de um único atributo já melhora consideravelmente a classificação.

O atributo escolhido foi `gpu_utilization_percent`, o que confirma uma evidência observada nas etapas anteriores de visualização: a utilização da GPU possui forte relação com o nível de risco de desperdício ambiental.

A métrica `Kappa statistic` foi igual a **0,5175**, indicando concordância moderada além do acaso. Isso mostra que o modelo aprendeu algum padrão útil, mas ainda está longe de representar adequadamente toda a complexidade do problema.

A matriz de confusão mostra que o modelo teve melhor desempenho na classe `alto` do que na classe `moderado`:

* `baixo`: 183 acertos em 268 registros, com recall de **0,683**;
* `moderado`: 153 acertos em 248 registros, com recall de **0,617**;
* `alto`: 125 acertos em 158 registros, com recall de **0,791**.

O resultado da classe `alto` é relevante, pois essa é a classe mais crítica do problema. Mesmo usando apenas um atributo, o `OneR` conseguiu identificar boa parte dos casos de maior risco.

Por outro lado, a classe `moderado` foi a mais difícil para o modelo. Ela teve 56 registros classificados como `baixo` e 39 classificados como `alto`, indicando que ocupa uma região intermediária e mais sujeita a confusão.

---

## Comparação entre os datasets

O desempenho foi idêntico nos dois datasets porque o atributo escolhido pelo `OneR`, `gpu_utilization_percent`, está presente tanto no dataset principal quanto no dataset com `AttributeSelection`.

Além disso, como o `OneR` usa apenas um atributo para construir a regra, a remoção dos demais atributos não teve impacto no resultado.

Isso indica que o `AttributeSelection` preservou um atributo importante para a classificação, mas também mostra uma limitação do `OneR`: mesmo com outros atributos disponíveis no dataset completo, ele utiliza apenas um deles.

---

## Síntese

O `OneR` apresentou desempenho superior ao `ZeroR` e mostrou que `gpu_utilization_percent` é um atributo individualmente forte para o problema.

Entretanto, sua acurácia de **68,3976%** e seu F1 ponderado de **0,683** indicam que uma única variável não é suficiente para capturar toda a complexidade do risco de desperdício ambiental.

O modelo é útil como baseline interpretável, pois ajuda a confirmar a importância da utilização da GPU. Porém, para obter melhor desempenho, são necessários algoritmos capazes de combinar múltiplos atributos, como J48, Random Forest, IBk, SMO ou Naive Bayes.

---

## Resposta Weka

### `dataset_preprocessado.arff`

```bash
=== Run information ===

Scheme:weka.classifiers.rules.OneR -B 6
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

gpu_utilization_percent:
	< 0.15760849999999998	-> baixo
	< 0.4836955	-> alto
	< 0.7663044999999999	-> moderado
	< 0.8097825	-> baixo
	< 0.8206519999999999	-> moderado
	>= 0.8206519999999999	-> baixo
(491/674 instances correct)


Time taken to build model: 0.03 seconds

=== Stratified cross-validation ===
=== Summary ===

Correctly Classified Instances         461               68.3976 %
Incorrectly Classified Instances       213               31.6024 %
Kappa statistic                          0.5175
Mean absolute error                      0.2107
Root mean squared error                  0.459 
Relative absolute error                 48.497  %
Root relative squared error             98.4903 %
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.683     0.172      0.723     0.683     0.702      0.755    baixo
                 0.617     0.225      0.614     0.617     0.616      0.696    moderado
                 0.791     0.091      0.727     0.791     0.758      0.85     alto
Weighted Avg.    0.684     0.173      0.684     0.684     0.683      0.756

=== Confusion Matrix ===

   a   b   c   <-- classified as
 183  77   8 |   a = baixo
  56 153  39 |   b = moderado
  14  19 125 |   c = alto
```

### `dataset_preprocessado_attrselect.arff`

A saída do Weka para o dataset com `AttributeSelection` apresentou os mesmos resultados quantitativos do dataset principal. A diferença está apenas na quantidade de atributos: a versão reduzida possui 6 atributos em vez de 27.

Isso ocorreu porque o `OneR` escolheu o mesmo atributo nos dois datasets:

```bash
gpu_utilization_percent
```

```bash
=== Run information ===

Scheme:weka.classifiers.rules.OneR -B 6
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

gpu_utilization_percent:
	< 0.15760849999999998	-> baixo
	< 0.4836955	-> alto
	< 0.7663044999999999	-> moderado
	< 0.8097825	-> baixo
	< 0.8206519999999999	-> moderado
	>= 0.8206519999999999	-> baixo
(491/674 instances correct)

=== Summary ===

Correctly Classified Instances         461               68.3976 %
Incorrectly Classified Instances       213               31.6024 %
Kappa statistic                          0.5175
Mean absolute error                      0.2107
Root mean squared error                  0.459 
Total Number of Instances              674     

=== Detailed Accuracy By Class ===

               TP Rate   FP Rate   Precision   Recall  F-Measure   ROC Area  Class
                 0.683     0.172      0.723     0.683     0.702      0.755    baixo
                 0.617     0.225      0.614     0.617     0.616      0.696    moderado
                 0.791     0.091      0.727     0.791     0.758      0.85     alto
Weighted Avg.    0.684     0.173      0.684     0.684     0.683      0.756

=== Confusion Matrix ===

   a   b   c   <-- classified as
 183  77   8 |   a = baixo
  56 153  39 |   b = moderado
  14  19 125 |   c = alto
```
