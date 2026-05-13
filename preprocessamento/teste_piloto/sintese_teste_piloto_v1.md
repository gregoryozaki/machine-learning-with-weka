# Síntese do Teste Piloto Preliminar — Dataset v1

## 1. Objetivo

Este documento consolida os resultados do teste piloto preliminar realizado sobre a primeira versão do dataset sintético: **[dataset_v1](/dataset/dados/versoes_dataset_original/dataset_v3_primeira_analise_piloto.csv)**.

O objetivo desta síntese é reunir os achados produzidos pelos integrantes da equipe, identificar padrões comuns nas análises e justificar a decisão metodológica de criar uma segunda versão do dataset, **[dataset_v2](/dataset/dataset_original.arff)**, com maior grau de imperfeições controladas.

O teste piloto preliminar foi realizado antes da etapa de pré-processamento, conforme definido na metodologia do trabalho. A análise teve como finalidade verificar a integridade estrutural do dataset, observar distribuições, identificar valores faltantes, analisar ruído, detectar outliers e avaliar relações entre atributos e a classe-alvo.

---

## 2. Dataset analisado

O dataset analisado no teste piloto preliminar foi a primeira versão do dataset original, armazenada em formato ARFF.

Cada instância representa:

> Um rack de datacenter em uma hora de operação.

A tarefa de aprendizado de máquina definida para o trabalho é:

> Classificação do nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de IA.

A classe-alvo é:

```bash
environmental_waste_risk_level
```

Com três valores possíveis:

```bash
baixo
moderado
alto
```

---

## 3. Participantes do teste piloto

O teste piloto preliminar foi realizado por três integrantes da equipe:

| Integrante        | Arquivo de análise                                                 |
| ----------------- | ------------------------------------------------------------------ |
| Calil Lima        | `preprocessamento/teste_piloto/análise_calil/análise_inicial.md`   |
| Wamberson Pacheco | `preprocessamento/teste_piloto/Analise Wamberson/Teste_Piloto.md`  |
| Tiago Santos      | `preprocessamento/teste_piloto/arquivo_tiago/analise_inicial_t.md` |

Cada integrante analisou o dataset a partir da metodologia definida em `01_teste_piloto.md`, observando integridade estrutural, estatísticas descritivas, valores faltantes, ruído, outliers, classe-alvo, atributos irrelevantes e relações semânticas.

---

## 4. Síntese dos achados

### 4.1. Integridade estrutural

As três análises indicaram que o dataset v1 estava estruturalmente correto.

Foram observados os seguintes pontos:

| Critério             | Achado consolidado                                          |
| -------------------- | ----------------------------------------------------------- |
| Carregamento no Weka | O dataset abriu corretamente no Weka                        |
| Número de instâncias | 674 instâncias                                              |
| Número de atributos  | 30 atributos                                                |
| Classe-alvo          | `environmental_waste_risk_level` reconhecida como nominal   |
| Tipos dos atributos  | Atributos numéricos e nominais reconhecidos corretamente    |
| Categorias válidas   | Não foram identificadas categorias fora do domínio definido |
| Linhas quebradas     | Não foram encontrados indícios de registros mal formatados  |
| Duplicatas           | Não foram identificadas duplicatas em excesso               |

A análise do Calil registrou que o arquivo `dataset_original.arff` foi carregado corretamente no Weka, com 674 instâncias e 30 atributos, além da classe-alvo nominal e presença de valores faltantes planejados. A análise do Wamberson chegou ao mesmo resultado, confirmando abertura correta no Weka, 674 registros, 30 atributos e tipos reconhecidos adequadamente. A análise do Tiago também registrou a ausência de instâncias idênticas e valores faltantes nas colunas esperadas.

**Interpretação:**
O Dataset v1 é estruturalmente válido e compatível com o Weka. Não houve necessidade de correção estrutural antes da análise exploratória.

---

### 4.2. Valores faltantes

Os três relatórios confirmaram a presença de valores faltantes apenas nos atributos planejados.

Os valores faltantes identificados foram:

| Atributo                    | Quantidade de valores faltantes |
| --------------------------- | ------------------------------: |
| `gpu_temperature_c`         |                               7 |
| `fan_speed_rpm`             |                               7 |
| `water_usage_effectiveness` |                               7 |
| `carbon_intensity_gco2_kwh` |                               7 |
| `job_status`                |                               6 |

Total de valores faltantes identificados:

```bash
34 valores faltantes
```

Esses valores representam aproximadamente:

```bash
34 / 674 ≈ 5,04% das instâncias afetadas
34 / 20220 ≈ 0,17% das células do dataset
```

As análises indicaram que os valores faltantes estavam em baixa proporção, mas foram inseridos em atributos relevantes para o domínio, como atributos térmicos, ambientais e operacionais.

**Interpretação:**
A presença de valores faltantes no Dataset v1 atende ao requisito do trabalho. Entretanto, a quantidade é baixa. Isso permite demonstrar a necessidade de imputação, mas torna a etapa de tratamento de valores faltantes pouco expressiva em termos de impacto no dataset.

---

### 4.3. Ruído

As análises indicaram que o ruído no datset v1 era baixo e plausível.

A relação entre:

```bash
active_power_w
energy_consumption_kwh
```

foi observada como altamente coerente, o que é esperado, pois cada instância representa uma janela de uma hora de operação.

Também foi observada coerência entre:

```bash
inlet_temperature_c
exhaust_temperature_c
delta_t_c
```

indicando que o `delta_t_c` acompanhava a diferença entre temperatura de exaustão e temperatura de entrada.

Os percentuais de utilização também permaneceram dentro das faixas esperadas, sem valores inválidos fora do intervalo de 0 a 100.

**Interpretação:**
O ruído existente no Dataset v1 é controlado e não compromete a plausibilidade dos registros. Porém, por ser baixo, ele torna a etapa de identificação de ruído menos evidente. As relações físicas principais permanecem muito estáveis, o que reduz a necessidade de decisões mais fortes no pré-processamento.

---

### 4.4. Outliers

As análises identificaram outliers interpretáveis em atributos operacionais, térmicos e de infraestrutura.

Foram destacados exemplos como:

| Atributo ou relação                       | Observação                                  |
| ----------------------------------------- | ------------------------------------------- |
| `gpu_temperature_c`                       | Valores elevados, chegando a 95 °C          |
| `fan_speed_rpm`                           | Valores elevados, chegando a 22000 RPM      |
| `job_duration_hours`                      | Jobs longos, chegando a 170 horas           |
| `rack_power_density_kw`                   | Valores altos de densidade de potência      |
| `gpu_utilization_percent` × `gpu_power_w` | Possíveis combinações de uso e consumo      |
| `fan_speed_rpm` × `gpu_temperature_c`     | Relações térmicas e esforço de refrigeração |

Os outliers foram interpretados como plausíveis no domínio de datacenters de IA, especialmente em cenários de alta carga, falha operacional, refrigeração intensa ou desperdício energético.

**Interpretação:**
O dataset v1 possui outliers interpretáveis. No entanto, as análises sugerem que eles aparecem de forma controlada e sem grande severidade. Assim, o requisito de presença de outliers é atendido, mas a análise de pré-processamento poderia ficar pouco evidente caso a equipe precise justificar decisões mais fortes sobre tratamento de outliers.

---

### 4.5. Classe-alvo

A classe-alvo `environmental_waste_risk_level` foi corretamente reconhecida como nominal e apresentou três categorias:

```bash
baixo
moderado
alto
```

A distribuição observada foi:

| Classe     | Quantidade aproximada |
| ---------- | --------------------: |
| `baixo`    |                   268 |
| `moderado` |                   248 |
| `alto`     |                   158 |

As análises indicaram que a classe `alto` aparece em menor quantidade, mas ainda em número suficiente para a tarefa de classificação. Também foi observado que as classes apresentam alguma sobreposição em atributos importantes, o que evita uma separação completamente trivial.

**Interpretação:**
A distribuição das classes é aceitável, embora exista leve desbalanceamento. Esse ponto deve ser considerado posteriormente na divisão treino/teste e na avaliação dos algoritmos.

---

### 4.6. Atributos irrelevantes

Os atributos irrelevantes planejados foram:

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

As análises não identificaram relação clara entre esses atributos e a classe-alvo. Esses atributos foram mantidos na v1 para atender ao requisito de presença de atributos irrelevantes e para permitir posterior remoção no pré-processamento.

**Interpretação:**
Os atributos irrelevantes cumprem sua função metodológica. Eles devem ser candidatos à remoção durante o pré-processamento.

---

## 5. Achados consolidados

| Aspecto analisado        | Resultado observado na v1          | Interpretação                                                       |
| ------------------------ | ---------------------------------- | ------------------------------------------------------------------- |
| Integridade estrutural   | Correta                            | Dataset apto para análise no Weka                                   |
| Instâncias               | 674                                | Atende ao requisito mínimo                                          |
| Atributos                | 30                                 | Estrutura preservada                                                |
| Valores faltantes        | Presentes, mas em baixa quantidade | Requisito atendido, porém pouco expressivo                          |
| Ruído                    | Baixo e plausível                  | Preserva coerência física, mas gera pouca necessidade de tratamento |
| Outliers                 | Presentes e interpretáveis         | Requisito atendido, mas sem grande severidade                       |
| Classe-alvo              | Três classes presentes             | Leve desbalanceamento na classe `alto`                              |
| Atributos irrelevantes   | Sem relação clara com a classe     | Devem ser removidos no pré-processamento                            |
| Compatibilidade com Weka | Confirmada                         | Arquivo ARFF válido                                                 |

---

## 6. Limitação identificada na v1

Embora o Dataset v1 esteja correto e atenda aos requisitos mínimos do trabalho, os relatórios do teste piloto indicaram que as imperfeições presentes no dataset estavam em baixa proporção e baixa severidade.

Essa característica tem uma vantagem: o dataset é mais limpo e plausível.

Porém, também gera uma limitação para o trabalho:

> A etapa de pré-processamento poderia se tornar pouco demonstrativa, pois haveria poucos valores faltantes, pouco ruído visível e poucos outliers mais severos a justificar.

Como o trabalho exige que o pré-processamento seja orientado por evidências observadas no teste piloto, a baixa intensidade das imperfeições pode reduzir a clareza das decisões metodológicas posteriores.

---

## 7. Decisão metodológica

Com base na consolidação dos três relatórios do teste piloto preliminar, a equipe decidiu criar uma segunda versão do dataset, [dataset v2](/dataset/dataset_original.arff).

A criação da v2 não substitui a validade da v1. A v1 permanece registrada como uma primeira versão válida, estruturalmente correta e compatível com o Weka.

A v2 foi criada como uma iteração metodológica motivada pelos achados do teste piloto preliminar.

O objetivo da v2 é aumentar, de forma controlada, a presença de:

* valores faltantes;
* ruído;
* outliers relacionais;
* inconsistências interpretáveis.

A v2 mantém:

* 674 instâncias;
* 30 atributos;
* mesma classe-alvo;
* mesmas categorias válidas;
* compatibilidade com Weka;
* mesma unidade de análise.

---

## 8. Justificativa para a v2

A criação da v2 foi motivada pelos seguintes pontos observados no teste piloto da v1:

| Evidência da v1                      | Consequência metodológica                     |
| ------------------------------------ | --------------------------------------------- |
| Apenas 34 valores faltantes no total | Pouco impacto para justificar imputação       |
| Ruído baixo em relações físicas      | Poucas inconsistências detectáveis            |
| Outliers presentes, mas controlados  | Tratamento de outliers pouco evidente         |
| Dataset estruturalmente correto      | A v2 poderia ser criada sem refazer o esquema |
| Atributos irrelevantes confirmados   | Podem ser mantidos para posterior remoção     |

Assim, a v2 foi criada para tornar o teste piloto e o pré-processamento mais evidentes, sem transformar o dataset em um conjunto de dados incoerente ou aleatório.

---

## 9. Papel da v1 e da v2 no trabalho

| Versão     | Papel no trabalho                                                                                                   |
| ---------- | ------------------------------------------------------------------------------------------------------------------- |
| dataset v1 | Primeira versão validada, usada para teste piloto preliminar                                                        |
| dataset v2 | Versão com maior grau de imperfeições controladas, candidata a dataset principal para pré-processamento e modelagem |

A v1 será preservada no repositório como evidência da primeira geração e da primeira análise exploratória.

A v2 será utilizada caso a equipe conclua que ela oferece melhores condições para demonstrar a etapa de pré-processamento exigida pelo trabalho.

---

## 10. Próximos passos

Após a criação da v2, os próximos passos são:

1. Validar estruturalmente o Dataset v2 no Weka.
2. Realizar nova análise exploratória sobre a v2.
3. Comparar v1 e v2 quanto a valores faltantes, ruído e outliers.
4. Escolher formalmente qual versão será utilizada como base para o pré-processamento.
5. Documentar as decisões de pré-processamento com base na análise da versão escolhida.
6. Gerar o dataset pré-processado.
7. Prosseguir para a etapa de treinamento e avaliação dos algoritmos.

---

## 11. Conclusão

O teste piloto preliminar demonstrou que o dataset v1 é válido, estruturado corretamente e compatível com o Weka. As análises realizadas por Calil Lima, Wamberson Pacheco e Tiago Santos confirmaram a presença de valores faltantes, ruído e outliers, bem como a validade da classe-alvo e dos atributos planejados.

Entretanto, a síntese dos achados mostrou que as imperfeições da v1 estavam presentes em baixa proporção e com baixa severidade. Por esse motivo, a equipe decidiu criar uma segunda versão do dataset, com maior grau de imperfeições controladas, para tornar a etapa de teste piloto e pré-processamento mais robusta e melhor documentada.

A decisão de criar a v2 é, portanto, uma iteração metodológica baseada em evidências do teste piloto preliminar, e não uma alteração arbitrária do dataset.
