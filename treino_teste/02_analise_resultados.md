# Análise Geral dos Resultados de Treino e Teste

#### Responsável: `Gregory Ozaki`

## Objetivo

Este documento consolida os resultados obtidos na etapa de treino e teste dos classificadores no Weka.

Foram avaliadas duas versões do dataset:

```bash
dataset/dataset_preprocessado.arff
dataset/dataset_preprocessado_attrselect.arff
```

A avaliação foi feita com:

```bash
Cross-validation
Folds: 10
```

A classe-alvo foi:

```bash
environmental_waste_risk_level
```

com as classes:

```bash
baixo
moderado
alto
```

A análise considera acurácia, Kappa, F1 ponderado, recall da classe `alto` e matriz de confusão. A classe `alto` recebeu atenção especial por representar os casos mais críticos de risco de desperdício ambiental.

---

## 1. Síntese dos algoritmos avaliados

Foram avaliados sete algoritmos de classificação:

| Algoritmo     | Nome no Weka   | Papel na análise                      |
| ------------- | -------------- | ------------------------------------- |
| ZeroR         | `ZeroR`        | Baseline mínimo                       |
| OneR          | `OneR`         | Baseline simples baseado em uma regra |
| Naive Bayes   | `NaiveBayes`   | Baseline probabilístico               |
| J48           | `J48`          | Árvore de decisão interpretável       |
| Random Forest | `RandomForest` | Ensemble de árvores                   |
| IBk / KNN     | `IBk`          | Classificador baseado em vizinhança   |
| SMO / SVM     | `SMO`          | Classificador baseado em margem       |

Além das execuções padrão, foram feitos ajustes de hiperparâmetros em:

```bash
RandomForest
IBk
```

No `RandomForest`, foi alterado o número de árvores. No `IBk`, foi alterado o número de vizinhos `K`.

---

## 2. Tabela comparativa geral

| Dataset                                 | Algoritmo    | Configuração   | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| --------------------------------------- | ------------ | -------------- | -------: | -----: | -----------: | ------------: |
| `dataset_preprocessado.arff`            | ZeroR        | padrão         | 39,7626% | 0,0000 |        0,226 |         0,000 |
| `dataset_preprocessado.arff`            | OneR         | padrão         | 68,3976% | 0,5175 |        0,683 |         0,791 |
| `dataset_preprocessado.arff`            | NaiveBayes   | padrão         | 67,6558% | 0,5063 |        0,676 |         0,886 |
| `dataset_preprocessado.arff`            | J48          | padrão         | 86,4985% | 0,7928 |        0,865 |         0,930 |
| `dataset_preprocessado.arff`            | SMO          | padrão         | 80,8605% | 0,7065 |        0,808 |         0,981 |
| `dataset_preprocessado.arff`            | IBk          | K = 1          | 75,5193% | 0,6236 |        0,757 |         0,892 |
| `dataset_preprocessado.arff`            | IBk          | K = 3          | 75,0742% | 0,6146 |        0,752 |         0,861 |
| `dataset_preprocessado.arff`            | IBk          | K = 5          | 74,0356% | 0,5985 |        0,741 |         0,867 |
| `dataset_preprocessado.arff`            | IBk          | K = 7          | 72,1068% | 0,5693 |        0,723 |         0,861 |
| `dataset_preprocessado.arff`            | RandomForest | numTrees = 50  | 91,5430% | 0,8700 |        0,915 |         0,975 |
| `dataset_preprocessado.arff`            | RandomForest | numTrees = 100 | 91,6914% | 0,8723 |        0,917 |         0,975 |
| `dataset_preprocessado.arff`            | RandomForest | numTrees = 200 | 91,9881% | 0,8769 |        0,920 |         0,975 |
| `dataset_preprocessado_attrselect.arff` | ZeroR        | padrão         | 39,7626% | 0,0000 |        0,226 |         0,000 |
| `dataset_preprocessado_attrselect.arff` | OneR         | padrão         | 68,3976% | 0,5175 |        0,683 |         0,791 |
| `dataset_preprocessado_attrselect.arff` | NaiveBayes   | padrão         | 74,7774% | 0,6111 |        0,743 |         0,899 |
| `dataset_preprocessado_attrselect.arff` | J48          | padrão         | 84,4214% | 0,7615 |        0,844 |         0,956 |
| `dataset_preprocessado_attrselect.arff` | SMO          | padrão         | 76,7062% | 0,6403 |        0,761 |         0,911 |
| `dataset_preprocessado_attrselect.arff` | IBk          | K = 1          | 82,9377% | 0,7384 |        0,829 |         0,968 |
| `dataset_preprocessado_attrselect.arff` | IBk          | K = 3          | 85,0148% | 0,7701 |        0,850 |         0,968 |
| `dataset_preprocessado_attrselect.arff` | IBk          | K = 5          | 83,3828% | 0,7451 |        0,834 |         0,956 |
| `dataset_preprocessado_attrselect.arff` | IBk          | K = 7          | 83,2344% | 0,7430 |        0,832 |         0,949 |
| `dataset_preprocessado_attrselect.arff` | RandomForest | numTrees = 50  | 85,9050% | 0,7838 |        0,859 |         0,975 |
| `dataset_preprocessado_attrselect.arff` | RandomForest | numTrees = 100 | 86,6469% | 0,7950 |        0,866 |         0,968 |
| `dataset_preprocessado_attrselect.arff` | RandomForest | numTrees = 200 | 87,0920% | 0,8019 |        0,871 |         0,975 |

---

## 3. Análise dos baselines

## 3.1. ZeroR

O `ZeroR` teve o pior desempenho, com acurácia de **39,7626%** nos dois datasets.

Esse valor corresponde à proporção da classe majoritária `baixo`:

```bash
268 / 674 = 39,7626%
```

O modelo classificou todas as instâncias como `baixo`, obtendo recall igual a 1,000 para a classe `baixo` e recall igual a 0,000 para `moderado` e `alto`.

Isso mostra que o `ZeroR` não aprendeu nenhum padrão real dos dados. Ele serve apenas como baseline mínimo.

Para este problema, o comportamento é inadequado, pois a classe `alto` representa os casos mais críticos e não foi identificada nenhuma vez.

---

## 3.2. OneR

O `OneR` teve acurácia de **68,3976%** nos dois datasets.

O atributo escolhido pelo algoritmo foi:

```bash
gpu_utilization_percent
```

Esse resultado confirma uma evidência observada na visualização dos dados: a utilização da GPU é um atributo individualmente forte para a classificação do risco de desperdício ambiental.

O modelo teve recall de **0,791** para a classe `alto`, o que é relevante para um algoritmo baseado em uma única regra. Porém, a acurácia e o F1 ponderado mostram que um único atributo não é suficiente para representar toda a complexidade do problema.

O desempenho foi idêntico nos dois datasets porque o atributo `gpu_utilization_percent` está presente tanto na versão principal quanto na versão com `AttributeSelection`.

---

## 4. Análise dos modelos probabilísticos

## 4.1. Naive Bayes

O `NaiveBayes` apresentou desempenho moderado no dataset completo e melhorou no dataset com `AttributeSelection`.

| Dataset                                 | Acurácia | F1 ponderado | Recall `alto` |
| --------------------------------------- | -------: | -----------: | ------------: |
| `dataset_preprocessado.arff`            | 67,6558% |        0,676 |         0,886 |
| `dataset_preprocessado_attrselect.arff` | 74,7774% |        0,743 |         0,899 |

A melhora no dataset reduzido indica que o `NaiveBayes` foi prejudicado pela presença de muitos atributos correlacionados no dataset completo. Isso faz sentido, pois o algoritmo assume independência condicional entre os atributos.

No dataset completo, houve forte confusão entre `baixo` e `moderado`:

```bash
baixo -> moderado: 127 casos
```

No dataset com `AttributeSelection`, a classe `baixo` melhorou bastante, mas a classe `moderado` piorou. O recall de `moderado` caiu de **0,706** para **0,552**.

Assim, o `NaiveBayes` funciona como baseline probabilístico útil, mas não foi um dos modelos mais fortes.

---

## 5. Análise dos modelos baseados em árvore

## 5.1. J48

O `J48` apresentou desempenho forte e boa interpretabilidade.

| Dataset                                 | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| --------------------------------------- | -------: | -----: | -----------: | ------------: |
| `dataset_preprocessado.arff`            | 86,4985% | 0,7928 |        0,865 |         0,930 |
| `dataset_preprocessado_attrselect.arff` | 84,4214% | 0,7615 |        0,844 |         0,956 |

No dataset completo, a árvore usou como primeira divisão o atributo:

```bash
rack_power_density_kw
```

Isso confirma a importância desse atributo já observada na etapa de visualização. A árvore também utilizou atributos como `water_usage_effectiveness`, `gpu_utilization_percent`, `inlet_temperature_c`, `delta_t_c`, `gpu_power_w` e `job_status`.

A versão com `AttributeSelection` gerou uma árvore menor:

| Dataset                                 | Folhas | Tamanho da árvore |
| --------------------------------------- | -----: | ----------------: |
| `dataset_preprocessado.arff`            |     48 |                88 |
| `dataset_preprocessado_attrselect.arff` |     18 |                35 |

A árvore reduzida foi mais simples e teve melhor recall para a classe `alto`, mas perdeu desempenho geral. Isso mostra que o dataset completo fornece mais informação para separar as classes de forma equilibrada.

O principal erro do J48 ocorreu entre `baixo` e `moderado`, o que confirma a sobreposição entre essas classes.

---

## 5.2. Random Forest

O `RandomForest` foi o melhor algoritmo geral.

Foram testadas três configurações:

```bash
numTrees = 50
numTrees = 100
numTrees = 200
```

### Resultados no dataset completo

| numTrees | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| -------: | -------: | -----: | -----------: | ------------: |
|       50 | 91,5430% | 0,8700 |        0,915 |         0,975 |
|      100 | 91,6914% | 0,8723 |        0,917 |         0,975 |
|      200 | 91,9881% | 0,8769 |        0,920 |         0,975 |

### Resultados no dataset com AttributeSelection

| numTrees | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| -------: | -------: | -----: | -----------: | ------------: |
|       50 | 85,9050% | 0,7838 |        0,859 |         0,975 |
|      100 | 86,6469% | 0,7950 |        0,866 |         0,968 |
|      200 | 87,0920% | 0,8019 |        0,871 |         0,975 |

A melhor configuração foi:

```bash
dataset_preprocessado.arff
RandomForest
numTrees = 200
```

com:

```bash
Acurácia: 91,9881%
Kappa: 0,8769
F1 ponderado: 0,920
Recall alto: 0,975
```

O ajuste de hiperparâmetro mostrou ganho consistente, ainda que moderado, ao aumentar o número de árvores. No dataset completo, a acurácia subiu de **91,5430%** para **91,9881%**.

A classe `alto` foi muito bem identificada:

```bash
alto: 154 acertos de 158
```

Nenhum registro da classe `alto` foi classificado como `baixo`. Esse é um ponto importante, pois evita o erro mais grave do problema.

O dataset com `AttributeSelection` teve desempenho inferior para o `RandomForest`. Isso indica que o algoritmo se beneficiou da riqueza informacional do dataset completo. Como o próprio Random Forest já seleciona subconjuntos aleatórios de atributos em cada árvore, a remoção prévia de atributos não trouxe vantagem.

---

## 6. Análise dos modelos baseados em distância e margem

## 6.1. IBk / KNN

O `IBk` foi avaliado com quatro valores de `K`:

```bash
K = 1
K = 3
K = 5
K = 7
```

### Melhor resultado por dataset

| Dataset                                 | Melhor K | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| --------------------------------------- | -------: | -------: | -----: | -----------: | ------------: |
| `dataset_preprocessado.arff`            |        1 | 75,5193% | 0,6236 |        0,757 |         0,892 |
| `dataset_preprocessado_attrselect.arff` |        3 | 85,0148% | 0,7701 |        0,850 |         0,968 |

No dataset completo, o melhor resultado ocorreu com `K = 1`. Quando o valor de `K` aumentou, o desempenho caiu gradualmente. Isso indica que considerar mais vizinhos suavizou demais as fronteiras e aumentou a mistura entre classes próximas.

No dataset com `AttributeSelection`, o melhor resultado ocorreu com `K = 3`. Isso mostra que a redução de atributos favoreceu o KNN, pois a distância entre instâncias ficou mais informativa.

Esse comportamento é esperado: algoritmos baseados em distância são sensíveis a atributos irrelevantes, redundantes ou pouco informativos. Ao reduzir o dataset para atributos mais fortes, o `IBk` passou a formar vizinhanças mais coerentes.

A classe `alto` teve bom desempenho no dataset reduzido:

```bash
alto: 153 acertos de 158
recall alto: 0,968
```

Assim, o `IBk` foi muito beneficiado pelo `AttributeSelection`.

---

## 6.2. SMO / SVM

O `SMO` foi executado com kernel linear:

```bash
PolyKernel -E 1.0
```

| Dataset                                 | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| --------------------------------------- | -------: | -----: | -----------: | ------------: |
| `dataset_preprocessado.arff`            | 80,8605% | 0,7065 |        0,808 |         0,981 |
| `dataset_preprocessado_attrselect.arff` | 76,7062% | 0,6403 |        0,761 |         0,911 |

O melhor resultado do SMO ocorreu no dataset completo.

O principal destaque foi o recall da classe `alto` no dataset completo:

```bash
recall alto: 0,981
```

O modelo acertou 155 de 158 registros da classe `alto`, cometendo apenas três erros nessa classe.

Por outro lado, o SMO teve dificuldade maior entre `baixo` e `moderado`:

```bash
baixo -> moderado: 62 casos
moderado -> baixo: 59 casos
```

No dataset com `AttributeSelection`, o desempenho geral caiu. O recall da classe `alto` também caiu de **0,981** para **0,911**, e a classe `moderado` foi bastante prejudicada, com recall de apenas **0,560**.

Isso indica que a redução de atributos removeu informações úteis para o SMO separar melhor as classes intermediárias.

---

## 7. Comparação entre os datasets

## 7.1. Dataset preprocessado completo

O `dataset_preprocessado.arff` foi melhor para:

* Random Forest;
* J48;
* SMO.

Esses modelos se beneficiaram da maior quantidade de atributos, pois conseguem explorar múltiplas relações entre consumo, utilização, temperatura, densidade energética e status operacional.

O melhor resultado geral do trabalho ocorreu nesse dataset:

```bash
RandomForest com numTrees = 200
Acurácia: 91,9881%
F1 ponderado: 0,920
Recall alto: 0,975
```

---

## 7.2. Dataset com AttributeSelection

O `dataset_preprocessado_attrselect.arff` foi melhor para:

* IBk;
* Naive Bayes.

Isso ocorreu porque esses algoritmos são mais sensíveis a atributos redundantes ou correlacionados.

No caso do `IBk`, a redução de atributos melhorou a distância entre instâncias. No caso do `NaiveBayes`, a redução diminuiu o impacto de dependências entre atributos.

Porém, o dataset reduzido não superou o melhor resultado geral obtido com o dataset completo.

---

## 8. Análise da classe `alto`

A classe `alto` é a mais importante do problema, pois representa maior risco de desperdício ambiental.

Os melhores resultados de recall para `alto` foram:

| Algoritmo    | Dataset                                 | Configuração              | Recall `alto` |
| ------------ | --------------------------------------- | ------------------------- | ------------: |
| SMO          | `dataset_preprocessado.arff`            | padrão                    |         0,981 |
| RandomForest | `dataset_preprocessado.arff`            | numTrees = 50, 100 ou 200 |         0,975 |
| RandomForest | `dataset_preprocessado_attrselect.arff` | numTrees = 50 ou 200      |         0,975 |
| IBk          | `dataset_preprocessado_attrselect.arff` | K = 1 ou 3                |         0,968 |
| J48          | `dataset_preprocessado_attrselect.arff` | padrão                    |         0,956 |

Embora o SMO tenha obtido o maior recall para `alto`, o Random Forest apresentou melhor equilíbrio geral entre as classes. Além disso, no melhor Random Forest, nenhum registro `alto` foi classificado como `baixo`.

No problema estudado, esse ponto é relevante, porque classificar um caso de alto risco como baixo risco seria o erro mais grave.

---

## 9. Erros observados nas matrizes de confusão

De modo geral, os modelos tiveram mais dificuldade em separar:

```bash
baixo
moderado
```

Isso apareceu em praticamente todos os algoritmos.

A classe `moderado` foi a mais difícil de classificar, porque ocupa uma região intermediária entre `baixo` e `alto`. Isso é coerente com o próprio significado da classe e com as visualizações feitas anteriormente.

A classe `alto`, por outro lado, foi bem identificada pelos modelos mais fortes, especialmente:

* Random Forest;
* SMO;
* J48;
* IBk com `AttributeSelection`.

---

## 10. Ranking dos melhores modelos

Considerando acurácia, Kappa, F1 ponderado, recall da classe `alto` e matriz de confusão, o ranking geral ficou:

| Posição | Modelo        | Dataset                                 | Configuração   | Justificativa                                                                  |
| ------: | ------------- | --------------------------------------- | -------------- | ------------------------------------------------------------------------------ |
|       1 | Random Forest | `dataset_preprocessado.arff`            | numTrees = 200 | Melhor desempenho geral e ótimo recall para `alto`                             |
|       2 | Random Forest | `dataset_preprocessado_attrselect.arff` | numTrees = 200 | Bom desempenho, mas inferior ao dataset completo                               |
|       3 | J48           | `dataset_preprocessado.arff`            | padrão         | Bom desempenho e alta interpretabilidade                                       |
|       4 | IBk           | `dataset_preprocessado_attrselect.arff` | K = 3          | Forte desempenho após seleção de atributos                                     |
|       5 | SMO           | `dataset_preprocessado.arff`            | padrão         | Excelente recall para `alto`, mas pior equilíbrio geral                        |
|       6 | Naive Bayes   | `dataset_preprocessado_attrselect.arff` | padrão         | Baseline probabilístico razoável                                               |
|       7 | OneR          | ambos                                   | padrão         | Baseline simples, útil para confirmar importância de `gpu_utilization_percent` |
|       8 | ZeroR         | ambos                                   | padrão         | Baseline mínimo, sem aprendizado real                                          |

---

## 11. Melhor algoritmo

O melhor algoritmo geral foi:

```bash
RandomForest
```

com a configuração:

```bash
numTrees = 200
```

aplicado ao dataset:

```bash
dataset_preprocessado.arff
```

Resultado:

```bash
Acurácia: 91,9881%
Kappa: 0,8769
F1 ponderado: 0,920
Recall alto: 0,975
```

Esse modelo apresentou o melhor equilíbrio entre desempenho geral e identificação da classe crítica.

A matriz de confusão confirma esse comportamento:

```bash
   a   b   c   <-- classified as
 249  19   0 |   a = baixo
  28 217   3 |   b = moderado
   0   4 154 |   c = alto
```

O modelo classificou corretamente 154 de 158 registros da classe `alto`. Os quatro erros foram classificados como `moderado`, e nenhum caso `alto` foi classificado como `baixo`.

Esse comportamento é adequado para o problema, pois evita o erro mais grave: tratar um caso de alto risco ambiental como baixo risco.

---

## 12. Melhor dataset

O melhor dataset para o desempenho geral foi:

```bash
dataset_preprocessado.arff
```

Esse dataset preserva mais atributos e permitiu que modelos como `RandomForest`, `J48` e `SMO` explorassem relações mais completas entre variáveis energéticas, térmicas, operacionais e ambientais.

O dataset com `AttributeSelection` foi útil em alguns casos, especialmente para `IBk` e `NaiveBayes`, mas não produziu o melhor resultado geral.

Portanto, a versão com `AttributeSelection` deve ser mantida como análise complementar, não como base principal do modelo final.

---

## 13. Impacto dos ajustes de hiperparâmetros

## 13.1. Random Forest

O ajuste de `numTrees` melhorou o desempenho de forma gradual nos dois datasets.

No dataset completo:

```bash
50 árvores  -> 91,5430%
100 árvores -> 91,6914%
200 árvores -> 91,9881%
```

No dataset com `AttributeSelection`:

```bash
50 árvores  -> 85,9050%
100 árvores -> 86,6469%
200 árvores -> 87,0920%
```

O melhor valor testado foi:

```bash
numTrees = 200
```

O ganho não foi grande, mas foi consistente. Isso mostra que o aumento do número de árvores melhorou a estabilidade do modelo.

---

## 13.2. IBk

No IBk, o melhor valor de `K` dependeu do dataset.

No dataset completo:

```bash
K = 1
```

foi o melhor, com acurácia de **75,5193%**.

No dataset com `AttributeSelection`:

```bash
K = 3
```

foi o melhor, com acurácia de **85,0148%**.

Isso mostra que o KNN é sensível à quantidade e qualidade dos atributos. Com muitos atributos, o aumento de `K` suavizou demais as decisões. Com atributos selecionados, `K = 3` melhorou a estabilidade sem perder muita separação entre classes.

---

## 14. Conclusão da etapa de treino e teste

A etapa de treino e teste mostrou que os modelos mais simples serviram bem como referência, mas não foram suficientes para capturar a complexidade do problema.

O `ZeroR` demonstrou o desempenho mínimo esperado ao sempre prever a classe majoritária. O `OneR` mostrou que `gpu_utilization_percent` é um atributo forte, mas insuficiente sozinho.

Entre os modelos principais, o `RandomForest` apresentou o melhor desempenho geral, especialmente no `dataset_preprocessado.arff` com `numTrees = 200`. Esse modelo combinou alta acurácia, bom F1 ponderado e excelente recall para a classe `alto`.

O `J48` também teve bom desempenho e se destacou pela interpretabilidade. O `SMO` teve excelente recall para a classe `alto`, mas menor equilíbrio geral. O `IBk` teve desempenho forte apenas após `AttributeSelection`, confirmando sua sensibilidade à escolha de atributos. O `NaiveBayes` melhorou com seleção de atributos, mas permaneceu abaixo dos melhores modelos.

A melhor combinação final foi:

```bash
Modelo: RandomForest
Dataset: dataset_preprocessado.arff
Configuração: numTrees = 200
```