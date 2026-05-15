# Classificação do Nível de Risco de Desperdício Ambiental em Racks de Datacenter Voltadas a Cargas de IA

Projeto desenvolvido para a disciplina **Inteligência Artificial**, ministrada pelo Prof. Dr. **Andrey Rodrigues**, no curso do Instituto de Ciências Exatas e Tecnologia da Universidade Federal do Amazonas (**ICET/UFAM**).

## Integrantes

- [Gregory Ozaki](https://github.com/gregoryozaki)
- [Ana Paula Xavier](https://github.com/ana-xavier19)
- [Calil Lima](https://github.com/Kallicco)
- [Gabriel Batista](https://github.com/Gaabrhiel)
- [Tiago Santos](https://github.com/TiagoSE)
- [Wamberson Pacheco](https://github.com/Dev-WambersonPacheco)

---

## Sumário

1. [Descrição do Projeto](#descrição-do-projeto)
2. [Definição do Problema](#definição-do-problema)
3. [Dataset](#dataset)
4. [Estrutura do Repositório](#estrutura-do-repositório)
5. [Fases do Trabalho](#fases-do-trabalho)
6. [Resultados Gerais](#resultados-gerais)
7. [Relatório Final](#relatório-final)
8. [Tecnologias Utilizadas](#tecnologias-utilizadas)
9. [Licença](#licença)

---

## Descrição do Projeto

Este projeto tem como objetivo construir, pré-processar, visualizar e avaliar um dataset sintético voltado à **classificação do nível de risco de desperdício ambiental em racks de datacenter**.

O dataset representa situações operacionais de racks em datacenters voltados a cargas de inteligência artificial, considerando atributos energéticos, térmicos, computacionais, operacionais e ambientais. Cada instância representa o comportamento de um rack durante uma hora de operação.

A classe-alvo do problema é:

```bash
environmental_waste_risk_level
```

com três classes:

```bash
baixo
moderado
alto
```

A construção do dataset foi realizada com apoio de **Modelos de Linguagem de Grande Escala (LLMs)**, seguindo um processo documentado, rastreável e baseado em regras semânticas. O dataset também inclui valores faltantes, ruídos, outliers interpretáveis e atributos irrelevantes, permitindo a aplicação de uma etapa completa de pré-processamento no **Weka**.

Após a geração do dataset, foram realizadas as etapas de teste piloto, pré-processamento, visualização de dados, treinamento e avaliação de classificadores.

---

## Definição do Problema

| Item                     | Definição                                                                       |
| ------------------------ | ------------------------------------------------------------------------------- |
| **Tema**                 | Classificação do nível de risco de desperdício ambiental em racks de datacenter |
| **Tarefa de ML**         | Classificação supervisionada                                                    |
| **Unidade da instância** | Um rack em uma hora de operação                                                 |
| **Entrada**              | Atributos energéticos, térmicos, computacionais, operacionais e ambientais      |
| **Saída esperada**       | Classe de risco de desperdício ambiental                                        |
| **Classes**              | `baixo`, `moderado`, `alto`                                                     |
| **Ferramenta principal** | Weka                                                                            |

O problema consiste em classificar o nível de risco de desperdício ambiental de um rack de datacenter a partir de variáveis relacionadas ao seu funcionamento. O risco não é definido apenas pelo consumo absoluto de energia, mas pela relação entre consumo, utilização computacional, comportamento térmico, eficiência operacional e indicadores ambientais.

---

## Dataset

Os principais arquivos do dataset estão na pasta [`dataset/`](dataset/).

| Arquivo                                                                                                                | Descrição                                     |
| ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| [`dataset/dataset_original.arff`](dataset/dataset_original.arff)                                                       | Dataset original antes do pré-processamento   |
| [`dataset/dataset_preprocessamento.arff`](dataset/dataset_preprocessamento.arff)                                       | Dataset preprocessado principal               |
| [`dataset/dataset_preprocessamento_attributeSelection.arff`](dataset/dataset_preprocessamento_attributeSelection.arff) | Versão preprocessada com seleção de atributos |
| [`dataset/README.md`](dataset/README.md)                                                                               | Documentação específica do dataset            |

A documentação da geração do dataset está organizada em:

| Documento                                                                                                      | Conteúdo                                                              |
| -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| [`dataset/01_pipeline_geracao_dataset.md`](dataset/01_pipeline_geracao_dataset.md)                             | Pipeline de geração do dataset sintético                              |
| [`dataset/02_engenharia_atributos.md`](dataset/02_engenharia_atributos.md)                                     | Definição e justificativa dos atributos                               |
| [`dataset/03_regras_semanticas.md`](dataset/03_regras_semanticas.md)                                           | Regras de coerência entre atributos e classes                         |
| [`dataset/04_planejamento_anomalias_inconsistencias.md`](dataset/04_planejamento_anomalias_inconsistencias.md) | Planejamento de valores faltantes, ruídos, outliers e inconsistências |

As versões intermediárias em CSV estão disponíveis em:

```bash
dataset/dados/versoes_dataset_original/
```

---

## Estrutura do Repositório

```bash
.
├── dataset
│   ├── 01_pipeline_geracao_dataset.md
│   ├── 02_engenharia_atributos.md
│   ├── 03_regras_semanticas.md
│   ├── 04_planejamento_anomalias_inconsistencias.md
│   ├── dados
│   │   ├── amostras
│   │   ├── lotes_classe_dataset
│   │   └── versoes_dataset_original
│   ├── dataset_original.arff
│   ├── dataset_preprocessamento.arff
│   ├── dataset_preprocessamento_attributeSelection.arff
│   └── README.md
├── imagens
│   ├── pipeline_geracao.png
│   ├── prints_weka
│   └── visualizacao
├── mapeamento_sistematico
│   └── protocolo_msl.md
├── preprocessamento
│   ├── 01_teste_piloto.md
│   ├── 02_analise_inicial.md
│   ├── 03_descricao_etapas.md
│   ├── 04_visualizacao_dados.md
│   └── teste_piloto
├── prompts
│   ├── geraracao_dataset
│   └── mapeamento_sistematico
├── relatorio
│   └── relatorio_final.md
├── treino_teste
│   ├── 01_metodo_treino_teste.md
│   ├── 02_analise_resultados.md
│   └── algoritmos
├── LICENSE
└── README.md
```

---

## Fases do Trabalho

| Fase | Descrição                                                                     | Documentação                                                                                                                                                         |
| ---: | ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|    1 | Definição do problema, classe-alvo, unidade de análise e atributos principais | [`dataset/01_pipeline_geracao_dataset.md`](dataset/01_pipeline_geracao_dataset.md)                                                                                   |
|    2 | Fundamentação teórica e mapeamento sistemático da literatura                  | [`mapeamento_sistematico/protocolo_msl.md`](mapeamento_sistematico/protocolo_msl.md)                                                                                 |
|    3 | Engenharia de atributos e definição das regras semânticas                     | [`dataset/02_engenharia_atributos.md`](dataset/02_engenharia_atributos.md), [`dataset/03_regras_semanticas.md`](dataset/03_regras_semanticas.md)                     |
|    4 | Planejamento de valores faltantes, ruídos, outliers e atributos irrelevantes  | [`dataset/04_planejamento_anomalias_inconsistencias.md`](dataset/04_planejamento_anomalias_inconsistencias.md)                                                       |
|    5 | Teste piloto do dataset original no Weka                                      | [`preprocessamento/01_teste_piloto.md`](preprocessamento/01_teste_piloto.md), [`preprocessamento/02_analise_inicial.md`](preprocessamento/02_analise_inicial.md)     |
|    6 | Pré-processamento no Weka                                                     | [`preprocessamento/03_descricao_etapas.md`](preprocessamento/03_descricao_etapas.md)                                                                                 |
|    7 | Visualização dos dados após o pré-processamento                               | [`preprocessamento/04_visualizacao_dados.md`](preprocessamento/04_visualizacao_dados.md)                                                                             |
|    8 | Treinamento e avaliação dos classificadores                                   | [`treino_teste/01_metodo_treino_teste.md`](treino_teste/01_metodo_treino_teste.md), [`treino_teste/02_analise_resultados.md`](treino_teste/02_analise_resultados.md) |
|    9 | Consolidação dos resultados no relatório final                                | [`relatorio/relatorio_final.md`](relatorio/relatorio_final.md)                                                                                                       |

---

## Pré-processamento

O pré-processamento foi realizado no Weka com os seguintes filtros:

| Filtro                 | Finalidade                                        |
| ---------------------- | ------------------------------------------------- |
| `ReplaceMissingValues` | Tratamento de valores faltantes                   |
| `Remove`               | Remoção de atributos administrativos irrelevantes |
| `NumericToNominal`     | Conversão de `num_gpus` para nominal              |
| `RemoveUseless`        | Verificação de atributos sem variação útil        |
| `Normalize`            | Normalização dos atributos numéricos              |
| `AttributeSelection`   | Criação de versão alternativa reduzida            |

Foram removidos os atributos:

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

A versão com `AttributeSelection` selecionou os seguintes atributos:

```bash
water_usage_effectiveness
inlet_temperature_c
gpu_utilization_percent
job_status
rack_power_density_kw
environmental_waste_risk_level
```

---

## Visualização dos Dados

As visualizações foram realizadas no Weka após o pré-processamento, com foco em distribuições, relações entre atributos e separação entre classes.

A documentação completa está em:

* [`preprocessamento/04_visualizacao_dados.md`](preprocessamento/04_visualizacao_dados.md)

As imagens estão organizadas em:

```bash
imagens/visualizacao/dataset_preprocessado/
imagens/visualizacao/datatset_preprocessado_attrselect/
```

As visualizações indicaram maior relevância visual para atributos como:

```bash
rack_power_density_kw
gpu_utilization_percent
water_usage_effectiveness
inlet_temperature_c
job_status
```

Também foi observada maior sobreposição entre as classes `baixo` e `moderado`, enquanto a classe `alto` apresentou separação mais evidente em vários cenários.

---

## Treinamento e Avaliação

A etapa de treino e teste foi realizada no Weka com:

```bash
Cross-validation
Folds: 10
```

Foram avaliados os seguintes algoritmos:

| Algoritmo         | Nome no Weka   |
| ----------------- | -------------- |
| ZeroR             | `ZeroR`        |
| OneR              | `OneR`         |
| Naive Bayes       | `NaiveBayes`   |
| Árvore de decisão | `J48`          |
| Random Forest     | `RandomForest` |
| KNN               | `IBk`          |
| SVM               | `SMO`          |

Os resultados individuais estão em:

| Algoritmo     | Arquivo                                                                                      |
| ------------- | -------------------------------------------------------------------------------------------- |
| ZeroR         | [`treino_teste/algoritmos/01_zeror.md`](treino_teste/algoritmos/01_zeror.md)                 |
| OneR          | [`treino_teste/algoritmos/02_oner.md`](treino_teste/algoritmos/02_oner.md)                   |
| IBk / KNN     | [`treino_teste/algoritmos/03_ibk_knn.md`](treino_teste/algoritmos/03_ibk_knn.md)             |
| Naive Bayes   | [`treino_teste/algoritmos/04_naive_bayes.md`](treino_teste/algoritmos/04_naive_bayes.md)     |
| SMO / SVM     | [`treino_teste/algoritmos/05_smo_smv.md`](treino_teste/algoritmos/05_smo_smv.md)             |
| J48           | [`treino_teste/algoritmos/06_j48.md`](treino_teste/algoritmos/06_j48.md)                     |
| Random Forest | [`treino_teste/algoritmos/07_random_forest.md`](treino_teste/algoritmos/07_random_forest.md) |

A análise consolidada está em:

* [`treino_teste/02_analise_resultados.md`](treino_teste/02_analise_resultados.md)

---

## Resultados Gerais

O melhor desempenho geral foi obtido com:

```bash
Algoritmo: RandomForest
Dataset: dataset/dataset_preprocessamento.arff
Configuração: numTrees = 200
```

Resultado:

| Métrica                 |    Valor |
| ----------------------- | -------: |
| Acurácia                | 91,9881% |
| Kappa                   |   0,8769 |
| F1 ponderado            |    0,920 |
| Recall da classe `alto` |    0,975 |

Matriz de confusão do melhor modelo:

```bash
   a   b   c   <-- classified as
 249  19   0 |   a = baixo
  28 217   3 |   b = moderado
   0   4 154 |   c = alto
```

O modelo não classificou nenhum registro da classe `alto` como `baixo`, o que é relevante para o problema, pois a classe `alto` representa os casos mais críticos de risco de desperdício ambiental.

---

## Relatório Final

O relatório final está disponível em:

* [`relatorio/relatorio_final.md`](relatorio/relatorio_final.md)

Ele reúne a definição do problema, fundamentação teórica, metodologia, resultados, discussão, conclusão e referências.

---

## Tecnologias Utilizadas

* **Weka** — pré-processamento, visualização, treinamento e avaliação dos modelos.
* **LLMs** — apoio à geração sintética do dataset.
* **SciSpace** — apoio ao mapeamento sistemático da literatura.
* **NotebookLM** — apoio à leitura e extração de informações dos artigos.
* **Markdown** — documentação do projeto.
* **Git/GitHub** — versionamento, organização e publicação do repositório.

---

## Observações

Este projeto utiliza um dataset sintético. Portanto, os resultados devem ser interpretados como uma avaliação experimental e metodológica, não como validação definitiva em ambiente real de datacenter.

Para aplicação prática, seria necessário validar a abordagem com dados reais de telemetria operacional.

---

## Licença

Este projeto está licenciado conforme os termos definidos no arquivo [LICENSE](LICENSE).
