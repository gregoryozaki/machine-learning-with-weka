# Análise Inicial - Dataset V2

#### Responsável: `Gregory Ozaki`

## Objetivo

Esta análise tem como objetivo realizar o teste piloto da segunda versão do dataset sintético antes da aplicação de qualquer técnica de pré-processamento.

O dataset analisado corresponde ao arquivo:

```bash
dataset/dataset_original.arff
```

Esse arquivo representa a segunda versão do dataset sintético, criada após a análise preliminar da V1. A V2 preserva a estrutura original, mas aumenta de forma controlada a presença de imperfeições, tornando mais evidente a etapa de teste piloto e a necessidade de decisões justificadas no pré-processamento.

O dataset V2 contém:

- registros sintéticos gerados com apoio de LLM;
- valores faltantes inseridos de forma controlada;
- ruído controlado;
- outliers interpretáveis;
- inconsistências pontuais planejadas;
- atributos irrelevantes planejados;
- classe-alvo `environmental_waste_risk_level`.

## Justificativa da análise do Dataset V2

A primeira versão do dataset foi analisada previamente pela equipe. Essa análise indicou que a V1 estava estruturalmente correta, era compatível com o Weka e continha valores faltantes, ruído e outliers conforme planejado. Porém, essas imperfeições apareciam em baixa proporção e com baixa severidade.

Por isso, a V2 foi criada como uma iteração metodológica, e não como uma substituição arbitrária. O objetivo foi aumentar, de forma controlada, a presença de:

- valores faltantes;
- ruído;
- outliers relacionais;
- inconsistências interpretáveis.

A V2 preserva:

- a mesma quantidade de instâncias;
- a mesma quantidade de atributos;
- a mesma classe-alvo;
- as mesmas categorias válidas;
- a compatibilidade com o Weka;
- a mesma unidade de análise: um rack de datacenter em uma hora de operação.

---

# Etapa 1 - Verificação de Integridade Estrutural

## Objetivo da etapa

Verificar se o dataset está tecnicamente correto, se abre no Weka sem erro e se sua estrutura corresponde ao que foi planejado.

## Verificações realizadas

| Nº | Verificação | Resultado esperado | Resultado observado | Status |
|---:|---|---|---|---|
| 1 | Carregamento no Weka | O arquivo `.arff` deve abrir corretamente | O arquivo abriu corretamente no Weka | ✅ |
| 2 | Número de instâncias | 674 instâncias | O Weka reconheceu 674 instâncias | ✅ |
| 3 | Número de atributos | 30 atributos | O Weka reconheceu 30 atributos | ✅ |
| 4 | Classe-alvo | `environmental_waste_risk_level` reconhecida como nominal | A classe foi reconhecida como nominal | ✅ |
| 5 | Tipos dos atributos | Numéricos como `numeric` e categóricos como `nominal` | Os tipos foram reconhecidos corretamente | ✅ |
| 6 | Valores faltantes | Presença de `?` nos atributos planejados | Valores faltantes presentes apenas nos atributos planejados, conforme detalhado na Etapa 3 | ✅ |
| 7 | Categorias válidas | Apenas categorias previstas | Não foram identificadas categorias inválidas | ✅ |
| 8 | Linhas quebradas | Ausência de registros mal formatados | O arquivo foi carregado sem erro de leitura ou deslocamento aparente de colunas | ✅ |
| 9 | Duplicatas | Ausência de duplicatas em excesso | Não foram identificadas instâncias duplicadas | ✅ |

## Evidências

**1. Carregamento no Weka**

![figura 01 - integridade no weka](/imagens/prints_weka/preprocessamento/figura_01_integridade_weka.png)

**2. Número de instâncias e atributos**

![figura 02 - numero de instancias e atributos](/imagens/prints_weka/preprocessamento/figura_02_instancias_atributos.png)

**3. Classe e atributos reconhecidos corretamente**

![figura 03 - classe como nominal](/imagens/prints_weka/preprocessamento/figura_03_classe_nominal.png)

**4. Valores faltantes nos atributos planejados**

![figura 04 - missing data planejados](/imagens/prints_weka/preprocessamento/figura_04_valores_faltantes_planejados.png)

## Interpretação

A verificação estrutural indica que o Dataset V2 está apto para análise exploratória no Weka. O arquivo foi lido corretamente, a classe-alvo foi reconhecida como nominal e os atributos numéricos e categóricos foram interpretados de forma adequada.

Não há indícios de linhas quebradas, colunas deslocadas, categorias inválidas ou duplicatas em excesso. Os problemas observados nesta etapa são esperados no contexto do trabalho, principalmente a presença de valores faltantes planejados.

## Registro de achados

| ID | Eixo | Achado observado | Evidência | Hipótese | Impacto no pré-processamento | Ação sugerida |
|---|---|---|---|---|---|---|
| A1 | Integridade estrutural | Dataset carregado corretamente no Weka, com 674 instâncias e 30 atributos | Prints de carregamento e estrutura no Weka | Estrutura ARFF válida | Dataset pode seguir para análise exploratória | Manter arquivo original para o teste piloto |

---

# Etapa 2 - Análise Estatística Descritiva

## Objetivo da etapa

Investigar o comportamento geral dos atributos numéricos e categóricos, observando mínimos, máximos, médias, desvios-padrão, frequências e distribuições.

## Estatísticas principais observadas

| Atributo | Mínimo | Máximo | Média | Desvio Padrão | Interpretação |
|---|---:|---:|---:|---:|---|
| `active_power_w` | 600 | 11980 | 5831,804 | 2955,029 | Alta variação de potência ativa, indicando diferentes níveis de carga energética. |
| `energy_consumption_kwh` | 0,60 | 12,00 | 5,798 | 2,913 | Variação compatível com uma janela de uma hora de operação. |
| `water_usage_effectiveness` | 0,28 | 4,96 | 1,262 | 0,672 | Variação ambiental relevante, com valores altos indicando maior impacto hídrico. |
| `carbon_intensity_gco2_kwh` | 62 | 893 | 381,536 | 165,111 | Alta variação, representando diferentes cenários de impacto climático. |
| `inlet_temperature_c` | 18,20 | 31,00 | 23,996 | 2,466 | Faixa operacional plausível e baixa dispersão. |
| `exhaust_temperature_c` | 25,80 | 74,00 | 51,785 | 9,641 | Maior dispersão térmica, associada ao calor dissipado pelo rack. |
| `delta_t_c` | 6,60 | 43,00 | 27,764 | 7,522 | Diferença térmica relevante entre entrada e exaustão. |
| `fan_speed_rpm` | 1666 | 22000 | 11567,525 | 4266,54 | Alta dispersão, indicando diferentes níveis de esforço de refrigeração. |
| `cpu_utilization_percent` | 4 | 87 | 51,794 | 19,283 | Uso de CPU dentro da faixa esperada. |
| `memory_utilization_percent` | 12 | 92 | 64,239 | 16,279 | Uso de memória relativamente alto, coerente com workloads de IA. |
| `gpu_power_w` | 51 | 670 | 395,798 | 152,568 | Alta variação de potência da GPU. |
| `gpu_utilization_percent` | 2 | 94 | 55,104 | 23,394 | Alta dispersão, útil para identificar subutilização de GPU. |
| `gpu_temperature_c` | 32 | 95 | 74,807 | 13,424 | Temperaturas elevadas em parte dos registros, relevantes para análise térmica. |
| `gpu_core_frequency_mhz` | 600 | 2147 | 1531,74 | 283,755 | Variação plausível de frequência operacional da GPU. |
| `num_gpus` | 1 | 16 | 7,315 | 5,035 | Atributo numérico com comportamento discreto. |
| `batch_size` | 1 | 2048 | 446,712 | 466,664 | Distribuição assimétrica, com concentração em valores menores. |
| `num_epochs` | 1 | 150 | 29,076 | 38,587 | Presença de treinamentos curtos e alguns mais longos. |
| `model_parameter_size_million` | 7 | 67766 | 13174,754 | 16563,193 | Alta dispersão, representando modelos de diferentes tamanhos. |
| `training_samples` | 3160 | 97744348 | 20492296,918 | 27872744,296 | Alta variação no volume de amostras de treinamento. |
| `job_duration_hours` | 0,06 | 185,07 | 33,754 | 41,821 | Distribuição assimétrica, com jobs curtos e alguns muito longos. |
| `rack_power_density_kw` | 5 | 120 | 27,274 | 27,498 | Alta dispersão, com presença de racks de alta densidade energética. |
| `power_cap_w` | 660 | 12000 | 7027,826 | 3353,017 | Grande variação nos limites de potência configurados. |

## Histogramas dos principais atributos

#### `active_power_w`

![active_power_w](/imagens/prints_weka/preprocessamento/figura_05_active_power_w.png)

#### `energy_consumption_kwh`

![energy_consumption_kwh](/imagens/prints_weka/preprocessamento/figura_06_energy_consumption_kwh.png)

#### `water_usage_effectiveness`

![water_usage_effectiveness](/imagens/prints_weka/preprocessamento/figura_07_water_usage_effectiveness.png)

#### `carbon_intensity_gco2_kwh`

![carbon_intensity_gco2_kwh](/imagens/prints_weka/preprocessamento/figura_08_carbon_intensity_gco2_kwh.png)

#### `inlet_temperature_c`

![inlet_temperature_c](/imagens/prints_weka/preprocessamento/figura_09_inlet_temperature_c.png)

#### `exhaust_temperature_c`

![exhaust_temperature_c](/imagens/prints_weka/preprocessamento/figura_10_exhaust_temperature_c.png)

#### `delta_t_c`

![delta_t_c](/imagens/prints_weka/preprocessamento/figura_11_delta_t_c.png)

#### `fan_speed_rpm`

![fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_12_fan_speed_rpm.png)

#### `cpu_utilization_percent`

![cpu_utilization_percent](/imagens/prints_weka/preprocessamento/figura_14_cpu_utilization_percent.png)

#### `memory_utilization_percent`

![memory_utilization_percent](/imagens/prints_weka/preprocessamento/figura_15_memory_utilization_percent.png)

#### `gpu_power_w`

![gpu_power_w](/imagens/prints_weka/preprocessamento/figura_16_gpu_power_w.png)

#### `gpu_utilization_percent`

![gpu_utilization_percent](/imagens/prints_weka/preprocessamento/figura_17_gpu_utilization_percent.png)

#### `gpu_temperature_c`

![gpu_temperature_c](/imagens/prints_weka/preprocessamento/figura_18_gpu_temperature_c.png)

#### `gpu_core_frequency_mhz`

![gpu_core_frequency_mhz](/imagens/prints_weka/preprocessamento/figura_19_gpu_core_frequency_mhz.png)

#### `num_gpus`

![num_gpus](/imagens/prints_weka/preprocessamento/figura_20_num_gpus.png)

#### `batch_size`

![batch_size](/imagens/prints_weka/preprocessamento/figura_22_batch_size.png)

#### `num_epochs`

![num_epochs](/imagens/prints_weka/preprocessamento/figura_23_num_epochs.png)

#### `model_parameter_size_million`

![model_parameter_size_million](/imagens/prints_weka/preprocessamento/figura_24_model_parameter_size_million.png)

#### `training_samples`

![training_samples](/imagens/prints_weka/preprocessamento/figura_25_training_samples.png)

#### `job_duration_hours`

![job_duration_hours](/imagens/prints_weka/preprocessamento/figura_26_job_duration_hours.png)

#### `rack_power_density_kw`

![rack_power_density_kw](/imagens/prints_weka/preprocessamento/figura_28_rack_power_density_kw.png)

#### `power_cap_w`

![power_cap_w](/imagens/prints_weka/preprocessamento/figura_30_power_cap_w.png)

## Interpretação

A análise estatística indica que os atributos permanecem dentro de faixas plausíveis para o domínio. Não foram observados valores negativos, percentuais acima de 100%, categorias inválidas ou valores numericamente impossíveis nos atributos analisados.

Os atributos energéticos `active_power_w` e `energy_consumption_kwh` apresentam comportamento semelhante, o que é esperado porque cada instância representa uma hora de operação. Essa relação precisa ser observada com cuidado, pois pode indicar redundância entre atributos.

Os atributos ambientais `water_usage_effectiveness` e `carbon_intensity_gco2_kwh` possuem variação relevante e valores faltantes planejados. Eles são importantes para representar impacto hídrico e climático, principalmente quando combinados com alto consumo energético.

Os atributos térmicos `exhaust_temperature_c`, `delta_t_c`, `fan_speed_rpm` e `gpu_temperature_c` apresentam dispersão considerável. Isso indica diferentes níveis de carga térmica e esforço de refrigeração.

Os atributos computacionais `cpu_utilization_percent`, `memory_utilization_percent` e `gpu_utilization_percent` estão dentro do intervalo esperado de 0 a 100%. A utilização de GPU apresenta alta dispersão, o que é relevante para identificar possíveis situações de desperdício, principalmente quando baixa utilização aparece junto de alta potência.

Os atributos de workload, como `batch_size`, `num_epochs`, `model_parameter_size_million`, `training_samples` e `job_duration_hours`, apresentam distribuições assimétricas. Isso é coerente com workloads de diferentes tamanhos e durações.

O atributo `num_gpus`, embora numérico, tem comportamento discreto. Essa característica deve ser considerada posteriormente, pois ele pode ser mantido como numérico ordinal ou tratado como categórico, dependendo da estratégia de pré-processamento.

## Atributos que exigem maior atenção

```bash
active_power_w
energy_consumption_kwh
water_usage_effectiveness
carbon_intensity_gco2_kwh
delta_t_c
fan_speed_rpm
gpu_power_w
gpu_utilization_percent
gpu_temperature_c
num_gpus
batch_size
num_epochs
model_parameter_size_million
training_samples
job_duration_hours
rack_power_density_kw
power_cap_w
```

## Implicações para o pré-processamento

| Ponto observado | Possível impacto | Decisão a avaliar |
|---|---|---|
| Valores faltantes em atributos ambientais, térmicos e operacionais | Pode afetar modelos que não lidam bem com dados ausentes | Aplicar imputação posteriormente |
| Escalas muito diferentes entre atributos | Pode afetar KNN, SVM e outros algoritmos sensíveis à escala | Avaliar normalização ou padronização |
| Relação forte entre `active_power_w` e `energy_consumption_kwh` | Pode gerar redundância | Avaliar correlação e possível manutenção ou remoção de um atributo |
| Alta dispersão em `rack_power_density_kw` e `power_cap_w` | Pode influenciar fortemente a classificação | Avaliar risco de dominância |
| Distribuições assimétricas em atributos de workload | Pode afetar algoritmos sensíveis à escala e outliers | Avaliar normalização e tratamento de outliers |
| `num_gpus` com comportamento discreto | Pode exigir decisão de representação | Avaliar se permanece numérico ou se será tratado como categórico |

## Registro de achados

| ID | Eixo | Atributo(s) analisado(s) | Achado observado | Evidência | Hipótese | Impacto no pré-processamento | Ação sugerida |
|---|---|---|---|---|---|---|---|
| A2.1 | Estatística descritiva | `active_power_w`, `energy_consumption_kwh` | Ampla variação e comportamento semelhante | Estatísticas e histogramas do Weka | Energia acompanha potência ativa em janela de uma hora | Pode haver redundância | Avaliar correlação |
| A2.2 | Estatística descritiva | `water_usage_effectiveness`, `carbon_intensity_gco2_kwh` | Variação relevante e valores faltantes planejados | Estatísticas e histogramas | Métricas ambientais variam conforme refrigeração e matriz energética | Exige tratamento de faltantes | Avaliar imputação |
| A2.3 | Estatística descritiva | `delta_t_c`, `fan_speed_rpm`, `gpu_temperature_c` | Dispersão térmica relevante | Histogramas e estatísticas | Existem diferentes níveis de carga térmica | Pode exigir análise de ruído e outliers | Verificar relações térmicas |
| A2.4 | Estatística descritiva | `gpu_power_w`, `gpu_utilization_percent` | Potência e utilização da GPU variam bastante | Histogramas do Weka | Há diferentes perfis de uso de GPU | Pode influenciar a classe-alvo | Analisar relação com desperdício |
| A2.5 | Estatística descritiva | `num_gpus` | Atributo numérico com comportamento discreto | Histograma do Weka | Representa configurações específicas de rack | Pode exigir decisão de representação | Avaliar codificação |
| A2.6 | Estatística descritiva | `job_duration_hours`, `training_samples`, `model_parameter_size_million` | Distribuições assimétricas | Histogramas do Weka | Existem workloads de diferentes escalas | Pode afetar algoritmos sensíveis à escala | Avaliar normalização |

---

# Etapa 3 - Análise de Valores Faltantes

## Objetivo da etapa

Verificar se os valores faltantes foram inseridos conforme o planejamento e levantar hipóteses sobre o tratamento posterior.

## Atributos candidatos a valores faltantes

```bash
gpu_temperature_c
fan_speed_rpm
water_usage_effectiveness
carbon_intensity_gco2_kwh
job_status
```

## Valores faltantes identificados

A análise no Weka confirmou a presença de valores faltantes apenas nos atributos planejados. Não foram identificados valores ausentes fora do escopo definido.

| Atributo | Tipo | Valores faltantes | Percentual aproximado | Observação |
|---|---|---:|---:|---|
| `job_status` | Nominal | 26 | 4% | Atributo operacional categórico com valores ausentes planejados. |
| `gpu_temperature_c` | Numeric | 32 | 5% | Atributo térmico sujeito a falhas de sensor ou ausência de telemetria. |
| `fan_speed_rpm` | Numeric | 32 | 5% | Atributo operacional/térmico sujeito a falhas de coleta. |
| `carbon_intensity_gco2_kwh` | Numeric | 32 | 5% | Atributo ambiental que pode depender de fonte externa ou regional. |
| `water_usage_effectiveness` | Numeric | 32 | 5% | Atributo ambiental que pode não estar disponível para todos os registros. |

Total de valores faltantes identificados:

```bash
154 valores faltantes
```

Proporção geral aproximada:

```bash
154 / 20220 ≈ 0,76% das células do dataset
```

## Evidências

#### `gpu_temperature_c`

![gpu_temperature_c](/imagens/prints_weka/preprocessamento/figura_36_gpu_temperature_c_missing_data.png)

#### `fan_speed_rpm`

![fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_37_fan_speed_rpm_missing_data.png)

#### `water_usage_effectiveness`

![water_usage_effectiveness](/imagens/prints_weka/preprocessamento/figura_39_water_usage_effectiveness_missing_data.png)

#### `carbon_intensity_gco2_kwh`

![carbon_intensity_gco2_kwh](/imagens/prints_weka/preprocessamento/figura_38_carbon_intensity_gco2_kwh_missing_data.png)

#### `job_status`

![job_status](/imagens/prints_weka/preprocessamento/figura_35_job_status_missing_data.png)

## Interpretação

Os valores faltantes aparecem apenas nos atributos definidos previamente como candidatos a ausência de informação. Isso indica que a inserção de missing data foi controlada e não ocorreu de forma espalhada por todo o dataset.

Os atributos afetados fazem sentido no domínio:

- `gpu_temperature_c`: pode ficar ausente por falha de sensor térmico ou indisponibilidade momentânea da telemetria;
- `fan_speed_rpm`: pode ficar ausente por falha na leitura dos ventiladores;
- `water_usage_effectiveness`: pode não estar disponível para todos os racks ou períodos;
- `carbon_intensity_gco2_kwh`: pode depender de informação externa sobre matriz energética ou região;
- `job_status`: pode estar ausente em registros incompletos, jobs ainda em execução ou falhas de registro operacional.

A quantidade de valores faltantes é maior que na V1, tornando a etapa de pré-processamento mais evidente. Ainda assim, a proporção geral permanece controlada, pois menos de 1% das células do dataset estão ausentes.

## Impacto no pré-processamento

| Tipo de atributo | Atributos | Estratégia candidata |
|---|---|---|
| Numérico | `gpu_temperature_c`, `fan_speed_rpm`, `carbon_intensity_gco2_kwh`, `water_usage_effectiveness` | Imputação por média/mediana ou filtro `ReplaceMissingValues` do Weka |
| Categórico | `job_status` | Imputação pela moda ou filtro `ReplaceMissingValues` do Weka |

A mediana pode ser mais adequada para atributos numéricos com assimetria ou valores extremos. Porém, como o trabalho será conduzido no Weka, o filtro `ReplaceMissingValues` é uma alternativa prática para a versão inicial do dataset preprocessado.

## Registro de achados

| ID | Eixo | Atributo(s) analisado(s) | Achado observado | Evidência | Hipótese | Impacto no pré-processamento | Ação sugerida |
|---|---|---|---|---|---|---|---|
| A3 | Valores faltantes | `gpu_temperature_c`, `fan_speed_rpm`, `water_usage_effectiveness`, `carbon_intensity_gco2_kwh`, `job_status` | Valores faltantes controlados e restritos aos atributos planejados | Contagem de missing no Weka | Simulação de falhas de coleta, sensor ou registro | Exige tratamento antes do treinamento | Aplicar imputação justificada ou `ReplaceMissingValues` |

---

# Etapa 4 - Análise de Ruído

## Objetivo da etapa

Verificar se o ruído inserido no dataset é leve, plausível e compatível com o domínio.

Nesta etapa, ruído é entendido como pequenas variações, desvios ou oscilações que não invalidam o registro. Casos mais extremos ou combinações muito incomuns serão aprofundados na Etapa 5, dedicada aos outliers.

## Atributos candidatos a ruído

```bash
active_power_w
energy_consumption_kwh
gpu_power_w
cpu_utilization_percent
gpu_utilization_percent
inlet_temperature_c
exhaust_temperature_c
gpu_temperature_c
delta_t_c
fan_speed_rpm
gpu_core_frequency_mhz
```

## Critérios utilizados

| Relação analisada | Critério esperado | O que foi verificado |
|---|---|---|
| `active_power_w` × `energy_consumption_kwh` | Energia próxima da potência dividida por 1000 em uma janela de 1 hora | Se a tendência positiva foi preservada |
| `inlet_temperature_c`, `exhaust_temperature_c` e `delta_t_c` | `delta_t_c` deve acompanhar a diferença térmica entre entrada e exaustão | Se a relação térmica geral continua plausível |
| `gpu_power_w` × `gpu_utilization_percent` | Maior utilização tende a aumentar potência, mas podem existir exceções | Se a dispersão indica ruído ou possíveis casos de desperdício |
| `gpu_temperature_c` × `fan_speed_rpm` | Temperaturas mais altas tendem a exigir maior rotação dos fans | Se há coerência térmica e pontos atípicos explicáveis |
| `gpu_core_frequency_mhz` × `gpu_power_w` | Frequências mais altas tendem a maior potência | Se a tendência operacional foi mantida |
| `active_power_w` × `power_cap_w` | Potência ativa tende a respeitar ou acompanhar limites configurados | Se existem casos de subutilização ou configuração permissiva |
| `job_duration_hours` × `job_status` | Jobs falhos, abortados ou em execução podem ter duração elevada | Se a relação operacional é plausível |

---

## Comparação de atributos relacionados

### Relação entre `active_power_w` e `energy_consumption_kwh`

![Relação entre active_power_w e energy_consumption_kwh](/imagens/prints_weka/preprocessamento/figura_40_active_power_X_energy_consumption.png)

A relação entre `active_power_w` e `energy_consumption_kwh` apresenta tendência positiva clara. Isso era esperado, pois cada instância representa uma hora de operação. Logo, o consumo energético tende a acompanhar aproximadamente a potência ativa dividida por 1000.

A maior parte dos registros segue essa relação. Os pontos afastados podem representar ruído energético, erro de medição ou pequena inconsistência de registro. Como a tendência principal foi preservada, esses pontos não devem ser removidos automaticamente.

---

### Relação entre `inlet_temperature_c` e `delta_t_c`

![Relação entre inlet_temperature_c e delta_t_c](/imagens/prints_weka/preprocessamento/figura_41_inlet_temperature_X_delta_t_.png)

A relação entre `inlet_temperature_c` e `delta_t_c` apresenta dispersão moderada. A temperatura de entrada possui faixa mais limitada, enquanto o delta térmico varia de forma mais ampla.

Esse comportamento é plausível, pois o `delta_t_c` depende não apenas da temperatura de entrada, mas também da carga térmica gerada pelo rack e da temperatura de exaustão.

---

### Relação entre `exhaust_temperature_c` e `delta_t_c`

![Relação entre exhaust_temperature_c e delta_t_c](/imagens/prints_weka/preprocessamento/figura_42_exhaust_temperature_X_delta_t.png)

A relação entre `exhaust_temperature_c` e `delta_t_c` apresenta tendência positiva. Temperaturas de exaustão mais altas tendem a aumentar a diferença térmica entre entrada e saída de ar.

Alguns pontos afastados aparecem na distribuição, mas eles não anulam a coerência geral da relação. Esses casos devem ser observados posteriormente como possíveis ruídos térmicos ou outliers relacionais.

---

### Relação entre `gpu_power_w` e `gpu_utilization_percent`

![Relação entre gpu_power_w e gpu_utilization_percent](/imagens/prints_weka/preprocessamento/figura_43_gpu_power_X_gpu_utilization_percent.png)

A relação entre potência da GPU e utilização da GPU apresenta dispersão considerável. Existem registros em que maior utilização está associada a maior potência, mas também há casos de alta potência com utilização baixa ou intermediária.

Esse comportamento é importante para o problema, pois alta potência com baixa utilização pode indicar desperdício energético ou má alocação de GPU. Portanto, esses pontos não devem ser tratados automaticamente como erro.

---

### Relação entre `gpu_temperature_c` e `fan_speed_rpm`

![Relação entre gpu_temperature_c e fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_44_gpu_temperature_X_fan_speed.png)

A relação entre temperatura da GPU e velocidade dos fans apresenta tendência positiva. Em geral, temperaturas mais altas aparecem associadas a maiores rotações dos fans, o que é coerente com o comportamento esperado de refrigeração.

Ainda assim, existem pontos atípicos, como temperatura elevada com fan speed baixo ou fan speed elevado em temperaturas não tão altas. Esses casos podem representar ruído operacional, atraso de resposta térmica, falha de sensor ou inconsistência planejada.

---

### Relação entre `gpu_utilization_percent` e `fan_speed_rpm`

![Relação entre gpu_utilization_percent e fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_45_gpu_utilization_percent_X_fan_speed.png)

A relação entre utilização da GPU e fan speed apresenta dispersão elevada. Isso é plausível porque a velocidade dos fans não depende apenas da utilização instantânea da GPU. Ela também pode depender da temperatura, do consumo, do método de refrigeração e da inércia térmica do sistema.

Registros com fan speed alto mesmo em baixa ou média utilização podem indicar refrigeração excessiva, temperatura residual ou desperdício de refrigeração.

---

### Relação entre `gpu_core_frequency_mhz` e `gpu_power_w`

![Relação entre gpu_core_frequency_mhz e gpu_power_w](/imagens/prints_weka/preprocessamento/figura_46_gpu_core_frequency_X_gpu_power.png)

A relação entre frequência da GPU e potência da GPU apresenta tendência positiva. Frequências mais altas tendem a estar associadas a maior potência, o que é coerente com o comportamento esperado de GPUs em cargas computacionais.

A dispersão observada é aceitável, pois a potência também depende da utilização, do tipo de workload, do power cap e da eficiência operacional.

---

### Relação entre `active_power_w` e `power_cap_w`

![Relação entre active_power_w e power_cap_w](/imagens/prints_weka/preprocessamento/figura_47_active_power_X_power_cap.png)

A relação entre potência ativa e limite de potência apresenta tendência positiva. Em geral, valores mais altos de `active_power_w` aparecem associados a maiores valores de `power_cap_w`.

Há alguns pontos afastados, como power cap alto com potência ativa menor. Esses registros podem indicar configuração permissiva, subutilização ou inconsistência operacional planejada. Essa relação deve ser observada posteriormente, pois `power_cap_w` pode se tornar um atributo forte para a classificação.

---

### Relação entre `job_duration_hours` e `job_status`

![Relação entre job_duration_hours e job_status](/imagens/prints_weka/preprocessamento/figura_48_job_duration_hours_X_job_status.png)

A relação entre duração do job e status operacional mostra que a maioria dos jobs bem-sucedidos está concentrada em durações menores, enquanto existem registros `failed`, `aborted` e `running` com durações mais longas.

Esse comportamento é plausível no domínio. Jobs que falham ou são abortados após longos períodos podem representar desperdício operacional. Por isso, esses registros não devem ser removidos automaticamente.

---

## Síntese da análise de ruído

A análise dos gráficos de dispersão indica que o Dataset V2 apresenta ruído visível, mas ainda compatível com o domínio. As principais relações físicas e operacionais foram preservadas:

- `energy_consumption_kwh` acompanha `active_power_w`;
- `delta_t_c` acompanha o comportamento térmico entre entrada e exaustão;
- `gpu_power_w` possui relação plausível com utilização e frequência da GPU;
- `fan_speed_rpm` tende a acompanhar temperatura, ainda que com variações;
- `power_cap_w` apresenta coerência geral com a potência ativa;
- jobs falhos, abortados ou em execução podem apresentar durações elevadas.

Os desvios observados não parecem aleatórios. Eles podem representar ruído controlado, erro de medição, falha de telemetria, configuração operacional inadequada ou outliers relacionais planejados.

Portanto, o ruído da V2 é mais evidente que o da V1, mas não compromete a estrutura geral do dataset. A decisão mais adequada é manter esses registros inicialmente e aprofundar a análise na etapa de outliers.

## Impacto no pré-processamento

| Ponto observado | Possível interpretação | Decisão a avaliar |
|---|---|---|
| Desvios entre potência e energia | Ruído energético ou erro de registro | Manter inicialmente e documentar |
| Desvios na relação térmica | Ruído térmico, atraso de resposta ou falha de sensor | Verificar consistência entre `inlet`, `exhaust` e `delta_t` |
| Alta potência de GPU com baixa utilização | Possível desperdício energético | Manter como informação relevante para a classe |
| Fan speed alto com baixa/média utilização | Refrigeração excessiva ou temperatura residual | Analisar junto com temperatura da GPU |
| Power cap alto com potência ativa menor | Configuração permissiva ou subutilização | Avaliar relação com a classe-alvo |
| Jobs falhos ou abortados com longa duração | Desperdício operacional | Manter como possível evidência de risco alto |

## Registro de achados

| ID | Eixo | Atributo(s) analisado(s) | Achado observado | Evidência | Hipótese | Impacto no pré-processamento | Ação sugerida |
|---|---|---|---|---|---|---|---|
| A4.1 | Ruído energético | `active_power_w`, `energy_consumption_kwh` | Relação positiva preservada, com desvios pontuais | Gráfico de dispersão no Weka | Pequenas variações simulam erro de medição ou telemetria | Não exige remoção imediata | Manter e documentar |
| A4.2 | Ruído térmico | `inlet_temperature_c`, `exhaust_temperature_c`, `delta_t_c` | Coerência térmica geral preservada | Gráficos térmicos no Weka | Desvios pontuais podem indicar ruído ou inconsistência planejada | Exige análise posterior de outliers | Manter inicialmente |
| A4.3 | Ruído operacional de GPU | `gpu_power_w`, `gpu_utilization_percent`, `gpu_core_frequency_mhz` | Potência, utilização e frequência mantêm relações plausíveis, mas com dispersão | Gráficos de dispersão | Alta potência com baixa utilização pode indicar desperdício | Pode ser relevante para a classe `alto` | Não remover automaticamente |
| A4.4 | Ruído de refrigeração | `gpu_temperature_c`, `fan_speed_rpm` | Fan speed tende a acompanhar temperatura, mas há casos atípicos | Gráfico de dispersão | Pode haver atraso térmico, falha de sensor ou refrigeração excessiva | Deve ser analisado junto aos outliers | Manter e revisar na Etapa 5 |
| A4.5 | Ruído operacional | `job_duration_hours`, `job_status` | Jobs falhos, abortados ou em execução podem ter maior duração | Gráfico por status no Weka | Longa duração com falha pode indicar desperdício operacional | Informação útil para a classificação | Manter inicialmente |

---

# Etapa 5 - Análise de Outliers

### Objetivo da etapa

Identificar valores extremos e combinações incomuns entre atributos, avaliando se esses casos representam erros, ruídos, inconsistências ou situações críticas interpretáveis dentro do domínio de racks de datacenters voltados a cargas de IA.

Nesta etapa, os outliers não serão removidos automaticamente. A decisão de manter, tratar ou remover será baseada na plausibilidade técnica dos valores e no impacto desses registros para a tarefa de classificação do nível de risco de desperdício ambiental.

----

### Atributos analisados

Foram analisados atributos com maior chance de apresentar valores extremos, principalmente atributos energéticos, térmicos, operacionais e computacionais:

```bash
active_power_w
energy_consumption_kwh
fan_speed_rpm
gpu_temperature_c
batch_size
model_parameter_size_million
training_samples
job_duration_hours
rack_power_density_kw
power_cap_w
```

Também foram analisadas relações entre atributos, buscando identificar outliers relacionais:

```bash
active_power_w × energy_consumption_kwh
gpu_power_w × gpu_utilization_percent
gpu_temperature_c × fan_speed_rpm
job_duration_hours × job_status
rack_power_density_kw × environmental_waste_risk_level
active_power_w × gpu_utilization_percent
```

---

### Análise dos atributos individuais

#### Atributo active_power_w

![active_power_w](/imagens/prints_weka/preprocessamento/figura_05_active_power_w.png)

A variação é alta, mas compatível com racks operando em diferentes níveis de carga energética. Os maiores valores indicam situações de maior consumo, mas não representam erro estrutural, pois permanecem dentro da faixa esperada para o dataset.

**Decisão:** manter os valores extremos, pois são plausíveis e relevantes para a classificação.

---

#### Atributo energy_consumption_kwh

![energy_consumption_kwh](/imagens/prints_weka/preprocessamento/figura_06_energy_consumption_kwh.png)

O atributo `energy_consumption_kwh` variou de 0,60 kWh a 12,00 kWh, com média de 5,798 kWh e desvio padrão de 2,913. Esses valores são compatíveis com a potência ativa considerando que cada instância representa uma hora de operação.

Não foram observados valores impossíveis, como consumo negativo ou consumo muito fora da escala definida.

**Decisão:** manter os valores extremos, pois acompanham a variação esperada da potência ativa.

---

#### Atributo fan_speed_rpm

![fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_12_fan_speed_rpm.png)

O atributo `fan_speed_rpm` apresentou valores entre 1666 rpm e 22000 rpm, com média de 11567,525 rpm e desvio padrão de 4266,54. A dispersão é elevada e há registros com rotação muito alta.

Esses valores foram interpretados como situações de maior esforço de refrigeração, possivelmente associadas a maior carga térmica ou operação crítica do rack. Como os valores continuam dentro das faixas planejadas, não foram considerados erros.

**Decisão:** manter como outliers operacionais e térmicos interpretáveis.

---

#### Atributo gpu_temperature_c

![gpu_temperature_c](/imagens/prints_weka/preprocessamento/figura_18_gpu_temperature_c.png)

O atributo `gpu_temperature_c` apresentou valores entre 32°C e 95°C, com média de 74,807°C e desvio padrão de 13,424. A distribuição mostra concentração em temperaturas mais elevadas, além de poucos casos em temperaturas mais baixas.

Temperaturas próximas de 95°C indicam uma condição térmica crítica, mas ainda são interpretáveis no contexto de cargas intensivas de IA. Por isso, não devem ser removidas automaticamente.

**Decisão:** manter como outliers térmicos críticos.

---

#### Atributo batch_size

![batch_size](/imagens/prints_weka/preprocessamento/figura_22_batch_size.png)

O atributo `batch_size` apresentou valores entre 1 e 2048, com média de 446,712 e desvio padrão de 466,664. A distribuição é assimétrica, com maior concentração em valores menores e poucos registros com batch size elevado.

Valores como 2048 são altos, mas plausíveis para determinadas cargas de IA. O comportamento discreto do atributo também explica a concentração em alguns valores específicos.

**Decisão:** manter os valores extremos, pois representam configurações possíveis de treinamento.

---

#### Atributo model_parameter_size_million

![model_parameter_size_million](/imagens/prints_weka/preprocessamento/figura_24_model_parameter_size_million.png)

O atributo `model_parameter_size_million` variou de 7 a 67766 milhões de parâmetros, com média de 13174,754 e desvio padrão de 16563,193. A distribuição é fortemente assimétrica, com muitos modelos menores e poucos modelos muito grandes.

Os maiores valores representam modelos de grande porte. Embora sejam extremos em relação à distribuição principal, são coerentes com o domínio de cargas de IA.

**Decisão:** manter os valores extremos, mas considerar normalização posteriormente devido à escala elevada.

---

#### Atributo training_samples

![training_samples](/imagens/prints_weka/preprocessamento/figura_25_training_samples.png)

O atributo `training_samples` apresentou valores entre 3160 e 97744348 amostras, com média de 20492296,918 e desvio padrão de 27872744,296. A distribuição é bastante assimétrica, com poucos registros representando volumes muito altos de dados.

Esse comportamento é plausível, pois cargas de IA podem variar desde experimentos pequenos até treinamentos com grandes bases de dados.

**Decisão:** manter os valores extremos, mas considerar normalização para algoritmos sensíveis à escala.

---

#### Atributo job_duration_hours

![job_duration_hours](/imagens/prints_weka/preprocessamento/figura_26_job_duration_hours.png)

O atributo `job_duration_hours` apresentou valores entre 0,06 e 185,07 horas, com média de 33,754 e desvio padrão de 41,821. A distribuição indica concentração em jobs mais curtos e poucos registros com durações muito longas.

Jobs muito longos podem representar treinamentos extensos, execuções ineficientes, processos travados ou tarefas que permaneceram em execução por tempo excessivo. Esses casos são importantes para a análise de desperdício ambiental.

**Decisão:** manter como outliers operacionais relevantes.

---

#### Atributo rack_power_density_kw

![rack_power_density_kw](/imagens/prints_weka/preprocessamento/figura_28_rack_power_density_kw.png)

O atributo `rack_power_density_kw` variou de 5 kW a 120 kW, com média de 27,274 kW e desvio padrão de 27,498. A distribuição é assimétrica, com muitos registros em baixa densidade e poucos casos de alta densidade energética.

Valores próximos de 120 kW são extremos, mas interpretáveis como racks de alta densidade. Esse atributo pode ter forte influência na classe-alvo, por estar diretamente relacionado à pressão energética do rack.

**Decisão:** manter, mas observar possível dominância.

---

#### Atributo power_cap_w

![power_cap_w](/imagens/prints_weka/preprocessamento/figura_30_power_cap_w.png)

O atributo `power_cap_w` apresentou valores entre 660 W e 12000 W, com média de 7027,826 W e desvio padrão de 3353,017. A distribuição mostra grande variação nos limites de potência configurados.

Os valores extremos são compatíveis com diferentes configurações de limite energético dos racks. Não foram observados valores impossíveis ou fora da escala planejada.

**Decisão:** manter.

---

### Análise de outliers relacionais

#### Relação active_power_w × energy_consumption_kwh

![active_power_w_x_energy_consumption_kwh](/imagens/prints_weka/preprocessamento/figura_40_active_power_X_energy_consumption.png)

A relação entre `active_power_w` e `energy_consumption_kwh` apresentou tendência positiva clara. Isso é esperado, pois cada instância representa uma hora de operação e o consumo energético tende a acompanhar a potência ativa.

Foram observados alguns pontos afastados da tendência principal, mas sem quebra grave da relação esperada. Esses pontos podem ser interpretados como ruído controlado ou pequenas variações operacionais.

**Decisão:** manter, pois a coerência energética geral foi preservada.

---

#### Relação gpu_power_w × gpu_utilization_percent

![gpu_power_x_gpu_utilization](/imagens/prints_weka/preprocessamento/figura_43_gpu_power_X_gpu_utilization_percent.png)

A relação entre `gpu_power_w` e `gpu_utilization_percent` mostrou dispersão relevante. Em geral, maior utilização da GPU tende a estar associada a maior potência, mas também existem casos de alta potência com baixa ou média utilização.

Esses casos são importantes para o problema, pois podem representar desperdício ambiental: consumo elevado sem aproveitamento computacional proporcional.

**Decisão:** manter como outliers relacionais relevantes.

---

#### Relação gpu_temperature_c × fan_speed_rpm

![gpu_temperature_x_fan_speed](/imagens/prints_weka/preprocessamento/figura_44_gpu_temperature_X_fan_speed.png)

A relação entre `gpu_temperature_c` e `fan_speed_rpm` indica uma tendência geral de aumento da rotação das ventoinhas em temperaturas mais altas. Entretanto, há dispersão e alguns pontos incomuns, como temperaturas altas com rotação relativamente baixa ou temperaturas menores com rotação elevada.

Essas combinações podem representar ruído de medição, atraso na resposta térmica, falha pontual de sensor ou situações operacionais específicas. Como não indicam erro estrutural evidente, serão mantidas.

**Decisão:** manter, registrando como possíveis outliers relacionais térmicos.

---

#### Relação job_duration_hours × job_status

![job_duration_x_job_status](/imagens/prints_weka/preprocessamento/figura_48_job_duration_hours_X_job_status.png)

A relação entre `job_duration_hours` e `job_status` mostra maior concentração de jobs curtos, principalmente em registros de execução bem-sucedida. Também existem jobs longos associados a diferentes estados, incluindo `running`, `failed` e `aborted`.

Esses casos são relevantes, pois jobs longos que falham, são abortados ou permanecem em execução podem indicar desperdício operacional e energético.

**Decisão:** manter, pois esses registros podem ajudar a caracterizar situações de risco ambiental.

---

#### Relação rack_power_density_kw × environmental_waste_risk_level

![rack_power_density_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_49_rack_power_density_X_environmental_waste_risk_level.png)

A relação entre `rack_power_density_kw` e `environmental_waste_risk_level` indica separação visual relevante entre as classes. A classe `baixo` aparece mais concentrada em valores menores de densidade de potência, enquanto a classe `alto` aparece com maior frequência em valores elevados.

Essa relação é coerente com o domínio, pois racks com maior densidade energética tendem a representar maior pressão operacional e ambiental. Porém, esse atributo pode se tornar dominante na classificação.

**Decisão:** manter, mas observar sua influência nos modelos de aprendizado de máquina.

---

#### Relação active_power_w × gpu_utilization_percent

![active_power_x_gpu_utilization](/imagens/prints_weka/preprocessamento/figura_50_active_power_X_gpu_utilization_percent.png)

A relação entre `active_power_w` e `gpu_utilization_percent` mostra registros com alta potência ativa e baixa ou média utilização da GPU. Esses casos são especialmente importantes para o problema, pois indicam possível desperdício ambiental: o rack consome energia elevada sem entregar uso computacional proporcional.

A presença desses pontos reforça que a classe-alvo não depende apenas do consumo absoluto, mas da combinação entre consumo, utilização e contexto operacional.

**Decisão:** manter como evidência relevante da semântica do dataset.

---

### Síntese dos outliers observados

| Atributo ou relação                                      | Tipo de outlier         | Evidência observada                       | Interpretação                               | Decisão                      |
| -------------------------------------------------------- | ----------------------- | ----------------------------------------- | ------------------------------------------- | ---------------------------- |
| `active_power_w`                                         | Energético              | Máximo de 11980 W                         | Alta carga energética                       | Manter                       |
| `energy_consumption_kwh`                                 | Energético              | Máximo de 12 kWh                          | Consumo compatível com uma hora de operação | Manter                       |
| `fan_speed_rpm`                                          | Operacional/térmico     | Máximo de 22000 rpm                       | Alto esforço de refrigeração                | Manter                       |
| `gpu_temperature_c`                                      | Térmico                 | Máximo de 95°C                            | Situação térmica crítica                    | Manter                       |
| `batch_size`                                             | Computacional           | Máximo de 2048                            | Configuração elevada, mas plausível         | Manter                       |
| `model_parameter_size_million`                           | Computacional           | Máximo de 67766 milhões de parâmetros     | Modelo de grande porte                      | Manter                       |
| `training_samples`                                       | Computacional           | Máximo de 97744348 amostras               | Grande volume de treinamento                | Manter                       |
| `job_duration_hours`                                     | Operacional             | Máximo de 185,07 horas                    | Job prolongado ou execução ineficiente      | Manter                       |
| `rack_power_density_kw`                                  | Energético              | Máximo de 120 kW                          | Rack de alta densidade energética           | Manter e observar dominância |
| `power_cap_w`                                            | Operacional/energético  | Máximo de 12000 W                         | Limite alto de potência configurado         | Manter                       |
| `active_power_w × energy_consumption_kwh`                | Relacional              | Tendência linear preservada               | Coerência energética mantida                | Manter                       |
| `gpu_power_w × gpu_utilization_percent`                  | Relacional              | Alta potência com baixa/média utilização  | Possível desperdício ambiental              | Manter                       |
| `gpu_temperature_c × fan_speed_rpm`                      | Relacional térmico      | Tendência positiva com dispersão          | Esforço de refrigeração plausível           | Manter                       |
| `job_duration_hours × job_status`                        | Relacional operacional  | Jobs longos em diferentes estados         | Possível desperdício operacional            | Manter                       |
| `rack_power_density_kw × environmental_waste_risk_level` | Relacional com a classe | Maior densidade associada à classe `alto` | Relação semântica forte                     | Manter e observar dominância |
| `active_power_w × gpu_utilization_percent`               | Relacional              | Alta potência com baixa/média utilização  | Indício de baixa eficiência computacional   | Manter                       |

---

## Impacto no pré-processamento

A análise indicou a presença de valores extremos em atributos energéticos, térmicos, operacionais e computacionais. Os principais casos foram observados em `fan_speed_rpm`, `gpu_temperature_c`, `job_duration_hours`, `rack_power_density_kw`, `model_parameter_size_million` e `training_samples`.

Apesar da presença de valores extremos, esses registros não foram interpretados automaticamente como erros. Em grande parte, os outliers observados são plausíveis no domínio de datacenters voltados a cargas de IA, representando situações como alto esforço de refrigeração, temperaturas críticas, jobs prolongados, modelos de grande porte, grande volume de amostras e racks de alta densidade energética.

As relações entre atributos também indicaram outliers relacionais relevantes, especialmente nos casos de alta potência associada à baixa ou média utilização de GPU. Esses registros são importantes para o problema, pois podem representar desperdício ambiental.

Dessa forma, os outliers serão inicialmente mantidos no dataset. A remoção automática não será aplicada nesta etapa, pois poderia eliminar registros úteis para caracterizar o risco de desperdício ambiental. No pré-processamento, será avaliada a necessidade de normalização ou padronização dos atributos numéricos, principalmente para algoritmos sensíveis à escala, como KNN e SVM.

---

## Etapa 6 - Análise da Classe-Alvo

### Objetivo da etapa

Analisar a distribuição e o comportamento da classe-alvo `environmental_waste_risk_level`, verificando se as categorias `baixo`, `moderado` e `alto` apresentam coerência com o problema de classificação do risco de desperdício ambiental em racks de datacenters voltados a cargas de IA.

Esta etapa também busca identificar possível desbalanceamento entre classes, separação excessiva, sobreposição entre grupos e atributos que possam influenciar fortemente a classificação.

----

### Distribuição da classe-alvo

![environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_34_environmental_waste_risk_level.png)

A classe-alvo `environmental_waste_risk_level` foi reconhecida como atributo nominal, contendo três categorias: `baixo`, `moderado` e `alto`.

A distribuição observada indica que as classes não estão perfeitamente balanceadas. A classe `alto` possui menor quantidade de registros em comparação com as classes `baixo` e `moderado`.

| Classe | Quantidade de registros | Interpretação |
|---|---:|---|
| `baixo` | 268 | Classe mais frequente |
| `moderado` | 248 | Classe intermediária, próxima de `baixo` |
| `alto` | 158 | Classe menos frequente |

Esse comportamento caracteriza um desbalanceamento moderado. A diferença entre as classes não inviabiliza a modelagem, mas exige atenção na avaliação dos algoritmos, principalmente para verificar se os modelos conseguem identificar corretamente a classe `alto`.

Por esse motivo, a etapa de treino e teste não deverá considerar apenas a acurácia geral. Também será necessário analisar matriz de confusão, precisão, recall e F1-score por classe.

----

### Relação active_power_w × environmental_waste_risk_level

![active_power_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_51_active_power_X_environmental_waste_risk_level.png)

A relação entre `active_power_w` e `environmental_waste_risk_level` mostra separação visual entre as classes. A classe `baixo` aparece mais concentrada em valores menores de potência ativa, enquanto a classe `alto` aparece com maior frequência em valores mais elevados.

A classe `moderado` ocupa uma região intermediária, com alguma sobreposição em relação às demais classes. Esse comportamento é coerente com o problema, pois maior potência ativa tende a indicar maior consumo energético, mas não deve ser interpretada isoladamente como desperdício.

**Interpretação:** a potência ativa contribui para a separação das classes, mas não deve ser tratada como único fator determinante.

**Decisão:** manter o atributo e avaliar sua influência na modelagem.

----

### Relação energy_consumption_kwh × environmental_waste_risk_level

![energy_consumption_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_52_energy_consumption_X_environmental_waste_risk_level.png)

A relação entre `energy_consumption_kwh` e `environmental_waste_risk_level` apresenta comportamento semelhante ao observado em `active_power_w`. A classe `baixo` aparece mais associada a menores valores de consumo energético, enquanto a classe `alto` aparece em faixas de consumo mais elevadas.

Esse padrão é esperado, pois cada instância representa uma hora de operação e o consumo energético está diretamente relacionado à potência ativa. Ainda assim, há sobreposição parcial entre as classes, indicando que o consumo não define sozinho o nível de risco.

**Interpretação:** o consumo energético é um atributo relevante, mas precisa ser analisado em conjunto com utilização computacional, temperatura e densidade de potência.

**Decisão:** manter.

----

### Relação gpu_utilization_percent × environmental_waste_risk_level

![gpu_utilization_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_53_gpu_utilization_X_environmental_waste_risk_level.png)

A relação entre `gpu_utilization_percent` e `environmental_waste_risk_level` é uma das mais importantes para a semântica do dataset. A classe `baixo` aparece com maior frequência em regiões de alta utilização de GPU. A classe `alto`, por outro lado, aparece mais concentrada em faixas de utilização menor ou intermediária.

Esse comportamento é coerente com a definição do problema. O risco de desperdício ambiental não está associado apenas ao consumo elevado, mas principalmente ao consumo elevado sem aproveitamento computacional proporcional.

Assim, registros com baixa utilização de GPU podem indicar ineficiência, principalmente quando combinados com alta potência ativa, alto consumo energético ou alta densidade de potência.

**Interpretação:** a utilização da GPU ajuda a diferenciar uso eficiente de possível desperdício ambiental.

**Decisão:** manter como atributo relevante para a classificação.

----

### Relação gpu_temperature_c × environmental_waste_risk_level

![gpu_temperature_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_54_gpu_temperature_X_environmental_waste_risk_level.png)

A relação entre `gpu_temperature_c` e `environmental_waste_risk_level` indica que a classe `alto` aparece com maior frequência em faixas mais elevadas de temperatura da GPU. A classe `baixo` aparece mais concentrada em temperaturas menores, enquanto `moderado` ocupa uma faixa intermediária.

Esse comportamento é coerente com o domínio, pois temperaturas mais altas podem estar associadas a maior carga térmica, maior esforço de refrigeração e maior pressão operacional sobre o rack. Entretanto, a temperatura também não deve ser analisada de forma isolada, pois uma GPU quente pode estar executando uma carga útil intensa e bem aproveitada.

**Interpretação:** a temperatura da GPU contribui para caracterizar criticidade operacional, mas precisa ser combinada com consumo e utilização.

**Decisão:** manter.

----

### Relação rack_power_density_kw × environmental_waste_risk_level

![rack_power_density_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_49_rack_power_density_X_environmental_waste_risk_level.png)

A relação entre `rack_power_density_kw` e `environmental_waste_risk_level` mostra separação visual forte entre as classes. A classe `baixo` aparece mais concentrada em valores baixos de densidade de potência, enquanto a classe `alto` aparece com maior frequência em valores elevados.

Essa relação é coerente com o domínio, pois racks com maior densidade energética tendem a representar maior pressão operacional e ambiental. Porém, esse atributo exige cuidado, pois pode se tornar dominante na etapa de modelagem.

**Interpretação:** relação semanticamente coerente, mas com possível risco de dominância.

**Decisão:** manter o atributo, mas observar sua influência nos resultados dos modelos.

----

### Separação e sobreposição entre classes

A análise visual indica que as classes apresentam separação parcial em atributos energéticos, térmicos e computacionais. A classe `baixo` aparece mais associada a menor consumo, menor temperatura e maior utilização relativa da GPU. A classe `alto` aparece mais associada a maior consumo, maior temperatura, maior densidade de potência e menor eficiência relativa de uso computacional.

Entretanto, a separação não é absoluta. Existem regiões de sobreposição entre `baixo`, `moderado` e `alto`, principalmente em atributos que não explicam sozinhos o risco de desperdício ambiental.

Essa sobreposição é positiva para o dataset, pois evita que o problema de classificação fique artificialmente simples. A classe-alvo parece depender da combinação entre consumo, utilização, temperatura, densidade energética e contexto operacional.

----

### Coerência semântica das classes

A análise indica coerência geral na construção da classe-alvo:

| Classe | Comportamento observado/esperado | Interpretação |
|---|---|---|
| `baixo` | Menor consumo, menor temperatura, menor densidade energética ou maior aproveitamento computacional | Baixo risco de desperdício |
| `moderado` | Valores intermediários ou combinações parcialmente críticas | Risco intermediário |
| `alto` | Maior consumo, maior temperatura, maior densidade energética ou baixa utilização relativa | Alto risco de desperdício |

A classe `moderado` tem papel importante no dataset porque representa casos de fronteira. Por isso, é provável que parte dos erros dos algoritmos ocorra entre `moderado` e as classes vizinhas `baixo` e `alto`.

----

### Risco de desbalanceamento

A menor quantidade de registros da classe `alto` pode afetar o desempenho dos classificadores. Um modelo pode obter boa acurácia geral classificando corretamente as classes mais frequentes, mas ainda apresentar dificuldade para reconhecer os casos de maior risco ambiental.

Por isso, na etapa de treino e teste, será necessário avaliar:

```bash
matriz de confusão
acurácia
precisão por classe
recall por classe
F1-score por classe
```

O recall da classe `alto` será especialmente importante, pois erros nessa classe representam falhas na identificação de situações críticas de desperdício ambiental.

---

### Síntese da análise da classe-alvo

| Aspecto analisado          | Achado                                                                                 | Interpretação                                      | Impacto futuro                         |
| -------------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------------- | -------------------------------------- |
| Distribuição das classes   | Classe `alto` possui menos registros                                                   | Desbalanceamento moderado                          | Avaliar métricas por classe            |
| `active_power_w`           | Classes mais críticas aparecem em potências maiores                                    | Relação coerente com consumo energético            | Manter                                 |
| `energy_consumption_kwh`   | Classe `alto` aparece em faixas maiores de consumo                                     | Relação coerente com operação de maior impacto     | Manter                                 |
| `gpu_utilization_percent`  | Classe `baixo` aparece com maior utilização; `alto` aparece com menor/média utilização | Indício de eficiência ou desperdício computacional | Manter                                 |
| `gpu_temperature_c`        | Classe `alto` aparece mais associada a temperaturas elevadas                           | Relação coerente com criticidade térmica           | Manter                                 |
| `rack_power_density_kw`    | Forte separação visual entre classes                                                   | Relação coerente, mas possivelmente dominante      | Monitorar                 |
| Sobreposição entre classes | Há regiões compartilhadas entre grupos                                                 | Problema menos trivial e mais realista             | Analisar matriz de confusão            |
| Classe `moderado`          | Ocupa faixa intermediária                                                              | Classe de fronteira                                | Pode concentrar erros de classificação |

---

### Decisão da etapa

A análise da classe-alvo indica que o dataset apresenta desbalanceamento moderado, com menor quantidade de registros na classe `alto`. Esse desbalanceamento não impede a modelagem, mas exige uma avaliação cuidadosa dos algoritmos.

As classes apresentam separação visual coerente em atributos energéticos, térmicos e computacionais, especialmente em `active_power_w`, `energy_consumption_kwh`, `gpu_utilization_percent`, `gpu_temperature_c` e `rack_power_density_kw`. Entretanto, também há sobreposição entre classes, o que torna o problema de classificação menos artificial.

A classe `alto` será tratada com atenção especial na etapa de modelagem, pois representa os casos de maior risco ambiental. Assim, além da acurácia, serão analisadas métricas por classe e matriz de confusão, com foco especial no desempenho dos modelos na identificação dos registros de alto risco.

O atributo `rack_power_density_kw` será mantido, mas deverá ser observado durante os testes, pois apresenta forte associação visual com a classe-alvo e pode influenciar significativamente alguns algoritmos.

----

# Etapa 7 - Análise dos Atributos Irrelevantes

### Objetivo da etapa

Verificar se os atributos planejados como irrelevantes apresentam relação semântica direta com a classe-alvo `environmental_waste_risk_level`.

Essa etapa é importante porque o dataset foi construído com atributos administrativos que não deveriam explicar o risco de desperdício ambiental. Caso algum desses atributos apresente associação visual com a classe, essa associação será tratada como possível viés sintético, e não como relação causal válida.

----

### Atributos analisados

Foram analisados os seguintes atributos planejados como irrelevantes:

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

Esses atributos representam informações administrativas ou artificiais associadas ao rack, mas não devem ser determinantes para classificar o risco ambiental.

---

### Atributo manufacturer_sku_id

![manufacturer_sku_id](/imagens/prints_weka/preprocessamento/figura_31_manufacturer_sku_id.png)

O atributo `manufacturer_sku_id` foi reconhecido como nominal e apresentou cinco categorias: `sku_a`, `sku_b`, `sku_c`, `sku_d` e `sku_e`.

A distribuição das categorias foi relativamente equilibrada:

| Categoria | Quantidade |
| --------- | ---------: |
| `sku_a`   |        139 |
| `sku_b`   |        159 |
| `sku_c`   |        129 |
| `sku_d`   |        118 |
| `sku_e`   |        129 |

Esse atributo representa um identificador de fabricante ou modelo. Embora esse tipo de informação possa existir em um inventário real de datacenter, ele não possui relação direta com consumo energético, temperatura, utilização computacional, refrigeração ou desperdício ambiental.

---

#### Relação manufacturer_sku_id × environmental_waste_risk_level

![manufacturer_sku_id_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_55_manufacturer_sku_id_X_environmental_waste_risk_level.png)

A relação entre `manufacturer_sku_id` e `environmental_waste_risk_level` mostra que as classes aparecem distribuídas entre as diferentes categorias de SKU. Não há evidência visual forte de que uma categoria específica determine sozinha uma classe de risco.

Mesmo que exista alguma variação entre categorias, ela não deve ser interpretada como causal. O identificador do fabricante não explica diretamente o desperdício ambiental; no máximo, poderia capturar algum viés artificial criado durante a geração sintética.

**Interpretação:** atributo administrativo, sem relação causal direta com a classe-alvo.

**Decisão:** remover no pré-processamento.

---

### Atributo rack_label_color

![rack_label_color](/imagens/prints_weka/preprocessamento/figura_32_rack_label_color.png)

O atributo `rack_label_color` foi reconhecido como nominal e apresentou cinco categorias: `blue`, `green`, `yellow`, `red` e `white`.

A distribuição das categorias foi relativamente equilibrada:

| Categoria | Quantidade |
| --------- | ---------: |
| `blue`    |        147 |
| `green`   |        131 |
| `yellow`  |        129 |
| `red`     |        139 |
| `white`   |        128 |

Esse atributo representa a cor da etiqueta do rack. Ele não possui relação técnica com o nível de desperdício ambiental, pois não influencia potência, consumo energético, utilização de GPU, temperatura, refrigeração ou duração dos jobs.

---

#### Relação rack_label_color × environmental_waste_risk_level

![rack_label_color_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_56_rack_label_color_X_environmental_waste_risk_level.png)

A relação entre `rack_label_color` e `environmental_waste_risk_level` mostra presença das três classes nas diferentes cores de etiqueta. Não há justificativa de domínio para considerar a cor da etiqueta como variável explicativa do risco ambiental.

Caso algum modelo utilize esse atributo para separar classes, isso indicaria aprendizado de um padrão artificial, não de uma relação real do problema.

**Interpretação:** atributo claramente irrelevante para o domínio.

**Decisão:** remover no pré-processamento.

---

### Atributo rack_inventory_zone

![rack_inventory_zone](/imagens/prints_weka/preprocessamento/figura_33_rack_inventory_zone.png)

O atributo `rack_inventory_zone` foi reconhecido como nominal e apresentou quatro categorias: `zone_a`, `zone_b`, `zone_c` e `zone_d`.

A distribuição das categorias foi a seguinte:

| Categoria | Quantidade |
| --------- | ---------: |
| `zone_a`  |        183 |
| `zone_b`  |        161 |
| `zone_c`  |        155 |
| `zone_d`  |        175 |

Esse atributo representa uma zona administrativa ou física do inventário do rack. Em um cenário real, a localização física poderia eventualmente estar associada a diferenças de infraestrutura, refrigeração ou densidade energética. Entretanto, neste dataset, esse atributo foi planejado como irrelevante e não deve ser usado como explicação principal para o risco ambiental.

---

#### Relação rack_inventory_zone × environmental_waste_risk_level

![rack_inventory_zone_x_environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_57_rack_inventory_zone_X_environmental_waste_risk_level.png)

A relação entre `rack_inventory_zone` e `environmental_waste_risk_level` mostra que as classes aparecem distribuídas entre as zonas. Embora possa haver pequenas diferenças de frequência entre categorias, não há evidência suficiente para tratar a zona de inventário como fator explicativo direto do risco de desperdício ambiental.

Como o atributo foi planejado como irrelevante, qualquer associação observada deve ser interpretada com cautela, pois pode representar viés sintético.

**Interpretação:** atributo administrativo, sem relação causal validada no dataset.

**Decisão:** remover no pré-processamento principal.

---

### Síntese da análise dos atributos irrelevantes

| Atributo              | Tipo de informação                 | Evidência observada                      | Interpretação                                              | Decisão |
| --------------------- | ---------------------------------- | ---------------------------------------- | ---------------------------------------------------------- | ------- |
| `manufacturer_sku_id` | Identificador de fabricante/modelo | Categorias distribuídas entre as classes | Não possui relação causal direta com desperdício ambiental | Remover |
| `rack_label_color`    | Cor da etiqueta do rack            | Classes distribuídas entre as cores      | Atributo artificial/administrativo                         | Remover |
| `rack_inventory_zone` | Zona administrativa/física         | Classes distribuídas entre zonas         | Pode representar viés sintético se usado pelo modelo       | Remover |

---

### Decisão da etapa

A análise dos atributos `manufacturer_sku_id`, `rack_label_color` e `rack_inventory_zone` indica que eles não possuem relação semântica direta com a classificação do risco de desperdício ambiental.

Esses atributos representam informações administrativas ou artificiais do rack e não explicam, por si só, consumo energético, temperatura, utilização computacional, refrigeração, densidade de potência ou duração dos jobs.

Dessa forma, recomenda-se remover esses atributos no pré-processamento principal. Essa decisão reduz o risco de os algoritmos aprenderem padrões artificiais da geração sintética e torna a modelagem mais coerente com o domínio do problema.

---

## Etapa 8 - Análise de Relações Semânticas

### Objetivo da etapa

Verificar se as principais relações semânticas utilizadas na construção do dataset aparecem de forma coerente nos dados.

Esta etapa busca avaliar se os atributos mantêm relações plausíveis entre si e se a classe-alvo `environmental_waste_risk_level` parece ser explicada por combinações de fatores energéticos, térmicos, computacionais e operacionais, e não por valores isolados ou padrões artificiais.

----

### Relações analisadas

Foram analisadas relações consideradas importantes para o domínio do problema:

```bash
active_power_w × energy_consumption_kwh
inlet_temperature_c × delta_t_c
exhaust_temperature_c × delta_t_c
gpu_power_w × gpu_utilization_percent
gpu_temperature_c × fan_speed_rpm
job_duration_hours × job_status
active_power_w × gpu_utilization_percent
rack_power_density_kw × environmental_waste_risk_level
gpu_utilization_percent × environmental_waste_risk_level
```

Essas relações foram escolhidas porque representam regras esperadas no domínio de datacenters voltados a cargas de IA, como coerência energética, coerência térmica, relação entre consumo e utilização computacional, esforço de refrigeração e impacto operacional.

---

### Relação active_power_w × energy_consumption_kwh

![active_power_w_x_energy_consumption_kwh](/imagens/prints_weka/preprocessamento/figura_40_active_power_X_energy_consumption.png)

A relação entre `active_power_w` e `energy_consumption_kwh` apresentou tendência positiva clara. Esse comportamento é esperado, pois cada instância representa uma hora de operação e o consumo energético tende a acompanhar a potência ativa.

A relação geral indica coerência energética no dataset. Pequenos desvios em torno da tendência principal podem ser interpretados como ruído controlado, mas não comprometem a estrutura da base.

**Interpretação:** a regra energética principal foi preservada.

**Impacto no pré-processamento:** manter os atributos, avaliando posteriormente possível redundância parcial entre potência ativa e consumo energético.

---

### Relação inlet_temperature_c × delta_t_c

![inlet_temperature_x_delta_t](/imagens/prints_weka/preprocessamento/figura_41_inlet_temperature_X_delta_t_.png)

A relação entre `inlet_temperature_c` e `delta_t_c` permite observar a variação da diferença térmica em função da temperatura de entrada. A dispersão indica que o delta térmico não depende apenas da temperatura de entrada, mas também de fatores como carga computacional, potência dissipada, refrigeração e temperatura de exaustão.

Esse comportamento é coerente, pois uma temperatura de entrada baixa não garante, isoladamente, baixo aquecimento do rack.

**Interpretação:** relação plausível e dependente de múltiplos fatores.

**Impacto no pré-processamento:** manter os atributos térmicos, pois eles representam aspectos complementares.

---

### Relação exhaust_temperature_c × delta_t_c

![exhaust_temperature_x_delta_t](/imagens/prints_weka/preprocessamento/figura_42_exhaust_temperature_X_delta_t.png)

A relação entre `exhaust_temperature_c` e `delta_t_c` mostrou coerência térmica mais clara. Valores maiores de temperatura de exaustão tendem a estar associados a maiores diferenças térmicas.

Esse resultado é esperado, pois o `delta_t_c` representa a diferença entre a temperatura de exaustão e a temperatura de entrada. Assim, a relação observada indica que a estrutura térmica do dataset foi preservada.

**Interpretação:** relação térmica semanticamente coerente.

**Impacto no pré-processamento:** manter, mas observar possível redundância entre `exhaust_temperature_c`, `inlet_temperature_c` e `delta_t_c`.

---

### Relação gpu_power_w × gpu_utilization_percent

![gpu_power_x_gpu_utilization](/imagens/prints_weka/preprocessamento/figura_43_gpu_power_X_gpu_utilization_percent.png)

A relação entre `gpu_power_w` e `gpu_utilization_percent` apresentou dispersão relevante. Em geral, maior utilização de GPU tende a estar associada a maior potência, mas existem registros com alta potência e baixa ou média utilização.

Esses casos são importantes para o problema, pois podem indicar consumo energético sem aproveitamento computacional proporcional. Portanto, a dispersão não deve ser tratada automaticamente como erro.

**Interpretação:** relação útil para identificar possível desperdício ambiental.

**Impacto no pré-processamento:** manter ambos os atributos, pois a combinação entre potência e utilização é relevante para a classe-alvo.

---

### Relação gpu_temperature_c × fan_speed_rpm

![gpu_temperature_x_fan_speed](/imagens/prints_weka/preprocessamento/figura_44_gpu_temperature_X_fan_speed.png)

A relação entre `gpu_temperature_c` e `fan_speed_rpm` mostrou tendência geral coerente: temperaturas mais altas tendem a estar associadas a maior rotação das ventoinhas. Também foram observados pontos dispersos, como temperaturas altas com rotação menor ou temperaturas mais baixas com rotação elevada.

Esses casos podem representar ruído de medição, atraso na resposta térmica, diferenças no método de refrigeração ou situações operacionais específicas.

**Interpretação:** relação térmica plausível, com ruído controlado.

**Impacto no pré-processamento:** manter os atributos e tratar os valores faltantes antes do treinamento.

---

### Relação job_duration_hours × job_status

![job_duration_x_job_status](/imagens/prints_weka/preprocessamento/figura_48_job_duration_hours_X_job_status.png)

A relação entre `job_duration_hours` e `job_status` indica maior concentração de jobs curtos, principalmente em registros de execução bem-sucedida. Também existem jobs longos em estados como `running`, `failed` e `aborted`.

Esses casos são relevantes porque execuções longas sem conclusão eficiente podem representar desperdício operacional e energético.

**Interpretação:** relação operacional coerente com a proposta do dataset.

**Impacto no pré-processamento:** manter ambos os atributos, tratando valores faltantes de `job_status`.

---

### Relação active_power_w × gpu_utilization_percent

![active_power_x_gpu_utilization](/imagens/prints_weka/preprocessamento/figura_50_active_power_X_gpu_utilization_percent.png)

A relação entre `active_power_w` e `gpu_utilization_percent` mostra registros com alta potência ativa e baixa ou média utilização da GPU. Esses casos são especialmente importantes, pois representam uma das ideias centrais do problema: o desperdício ambiental ocorre quando há alto consumo sem aproveitamento computacional proporcional.

A presença desses registros indica que a classe-alvo não depende apenas de consumo absoluto, mas da combinação entre consumo e eficiência de uso.

**Interpretação:** relação central para a definição de desperdício ambiental no dataset.

**Impacto no pré-processamento:** preservar os dois atributos e evitar transformações que eliminem essa relação.

---

### Relação rack_power_density_kw × environmental_waste_risk_level

A relação entre `rack_power_density_kw` e `environmental_waste_risk_level`, analisada na etapa anterior, mostrou forte associação visual entre maior densidade de potência e a classe `alto`.

Essa relação é coerente com o domínio, pois racks de maior densidade energética tendem a representar maior pressão operacional e ambiental. No entanto, esse atributo deve ser acompanhado na modelagem, pois pode se tornar dominante em alguns algoritmos.

**Interpretação:** relação semanticamente coerente, mas com risco de dominância.

**Impacto no pré-processamento:** manter inicialmente e observar sua influência nos modelos.

---

### Síntese das relações semânticas

| Relação analisada                                        | Comportamento observado                                 | Interpretação                              | Decisão            |
| -------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------ | ------------------ |
| `active_power_w × energy_consumption_kwh`                | Tendência positiva clara                                | Coerência energética preservada            | Manter             |
| `inlet_temperature_c × delta_t_c`                        | Relação dispersa, mas plausível                         | Delta térmico depende de múltiplos fatores | Manter             |
| `exhaust_temperature_c × delta_t_c`                      | Relação positiva mais clara                             | Coerência térmica preservada               | Manter             |
| `gpu_power_w × gpu_utilization_percent`                  | Dispersão com casos de alta potência e baixa utilização | Possível desperdício ambiental             | Manter             |
| `gpu_temperature_c × fan_speed_rpm`                      | Tendência positiva com dispersão                        | Esforço de refrigeração plausível          | Manter             |
| `job_duration_hours × job_status`                        | Jobs longos em diferentes estados                       | Possível desperdício operacional           | Manter             |
| `active_power_w × gpu_utilization_percent`               | Alta potência com baixa/média utilização                | Relação central para desperdício           | Manter             |
| `rack_power_density_kw × environmental_waste_risk_level` | Forte associação com a classe `alto`                    | Coerente, mas possivelmente dominante      | Manter e monitorar |

---

### Decisão da etapa

A análise das relações semânticas indica que o dataset preserva coerência geral com o domínio do problema. As relações energéticas, térmicas, computacionais e operacionais observadas são plausíveis e contribuem para explicar a classe-alvo.

A classe `environmental_waste_risk_level` não parece depender apenas de um único atributo isolado. Em vez disso, o risco de desperdício ambiental é representado pela combinação entre consumo energético, utilização computacional, temperatura, esforço de refrigeração, duração dos jobs e densidade de potência.

Foram observadas relações fortes, especialmente envolvendo `rack_power_density_kw`, `active_power_w`, `energy_consumption_kwh` e `gpu_utilization_percent`. Essas relações serão mantidas, mas alguns atributos deverão ser observados na etapa de modelagem para verificar possível dominância.

Com isso, o dataset é considerado adequado para seguir para a etapa de pré-processamento, desde que sejam aplicadas decisões justificadas, como tratamento de valores faltantes, remoção de atributos irrelevantes e normalização de atributos numéricos para algoritmos sensíveis à escala.

---

# Síntese Geral do Teste Piloto e Encaminhamento para o Pré-processamento

## Síntese do teste piloto

O teste piloto do Dataset V2 indicou que a base está estruturalmente adequada para seguir para a etapa de pré-processamento. O arquivo foi carregado corretamente no Weka, com 674 instâncias, 30 atributos, classe-alvo nominal e tipos de atributos reconhecidos de forma adequada. Não foram observados problemas estruturais graves, como linhas quebradas, categorias inválidas ou duplicatas em excesso.

A análise exploratória confirmou que o dataset contém os elementos exigidos para o trabalho: valores faltantes, ruído, outliers, atributos irrelevantes e relações semânticas planejadas. Esses elementos não aparecem de forma aleatória, mas como imperfeições controladas inseridas para tornar o dataset mais realista e metodologicamente adequado.

Os valores faltantes aparecem apenas nos atributos planejados: `gpu_temperature_c`, `fan_speed_rpm`, `water_usage_effectiveness`, `carbon_intensity_gco2_kwh` e `job_status`. A proporção geral de valores ausentes é controlada, mas eles precisam ser tratados antes do treinamento dos modelos, pois alguns desses atributos são relevantes para a classificação.

Também foram observados ruídos e outliers em atributos energéticos, térmicos, operacionais e computacionais. Porém, esses casos não foram interpretados automaticamente como erros. Em grande parte, eles representam situações plausíveis no domínio, como alto esforço de refrigeração, temperaturas críticas, jobs longos, modelos de grande porte, grande volume de amostras e alta densidade energética.

A análise da classe-alvo indicou desbalanceamento moderado, com menor quantidade de registros na classe `alto`. Esse desbalanceamento não impede a modelagem, mas exige avaliação por métricas além da acurácia, principalmente matriz de confusão, precisão, recall e F1-score por classe.

Os atributos `manufacturer_sku_id`, `rack_label_color` e `rack_inventory_zone` foram confirmados como atributos irrelevantes ou administrativos. Eles não possuem relação semântica direta com consumo energético, temperatura, utilização computacional, refrigeração ou risco de desperdício ambiental. Por isso, deverão ser removidos no pré-processamento principal.

As relações semânticas analisadas indicaram coerência geral no dataset. Relações como `active_power_w × energy_consumption_kwh`, `gpu_power_w × gpu_utilization_percent`, `gpu_temperature_c × fan_speed_rpm`, `job_duration_hours × job_status` e `active_power_w × gpu_utilization_percent` apresentaram comportamento plausível. Isso indica que o risco de desperdício ambiental não depende apenas de um atributo isolado, mas da combinação entre consumo, utilização, temperatura, esforço de refrigeração, duração dos jobs e densidade de potência.

## Interpretação para o pré-processamento

O fato de ruídos e outliers terem sido mantidos na análise inicial não significa ausência de pré-processamento. A decisão de manter alguns registros extremos foi tomada porque eles são interpretáveis e relevantes para o problema. Removê-los automaticamente poderia eliminar justamente os casos críticos que ajudam a caracterizar a classe `alto`.

Assim, o pré-processamento será aplicado de forma seletiva e justificada. A equipe não irá limpar o dataset de forma mecânica, mas aplicar filtros e transformações compatíveis com os achados do teste piloto.

## Decisões de pré-processamento

| Problema identificado | Decisão de pré-processamento | Justificativa |
|---|---|---|
| Valores faltantes em atributos numéricos | Aplicar `ReplaceMissingValues` no Weka | Os faltantes são controlados, mas precisam ser tratados antes do treino |
| Valor faltante em atributo nominal `job_status` | Aplicar `ReplaceMissingValues` no Weka | O filtro substitui valores ausentes em atributos nominais pela moda |
| Atributos irrelevantes | Remover `manufacturer_sku_id`, `rack_label_color` e `rack_inventory_zone` | Não possuem relação causal com desperdício ambiental e podem gerar viés sintético |
| Escalas numéricas muito diferentes | Aplicar normalização ou padronização | Necessário para algoritmos sensíveis à escala, como KNN e SVM |
| Outliers interpretáveis | Manter inicialmente | Representam casos críticos plausíveis no domínio |
| Ruído leve e plausível | Manter | Simula variações operacionais realistas |
| Possível atributo dominante `rack_power_density_kw` | Manter, mas monitorar na modelagem | Relação é coerente, mas pode influenciar excessivamente alguns algoritmos |
| Atributo `num_gpus` com comportamento discreto | Manter inicialmente como numérico | Representa quantidade ordinal de GPUs; decisão pode ser revista na modelagem |
| Redundância entre `active_power_w` e `energy_consumption_kwh` | Manter inicialmente | A relação é esperada; possível redundância será avaliada nos resultados dos modelos |

## Filtros do Weka previstos

O pré-processamento será realizado exclusivamente no Weka, utilizando filtros da aba `Preprocess`. A sequência inicial proposta é:

| Ordem | Filtro do Weka | Finalidade |
|---:|---|---|
| 1 | `ReplaceMissingValues` | Substituir valores faltantes em atributos numéricos e nominais |
| 2 | `Remove` | Remover atributos irrelevantes confirmados |
| 3 | `Normalize` ou `Standardize` | Ajustar escalas numéricas para algoritmos sensíveis à distância |
| 4 | Filtro adicional não explorado em sala | Atender ao requisito do trabalho e justificar sua utilidade |

## Filtro adicional sugerido

Como o enunciado exige pelo menos um filtro do Weka não explorado em sala, recomenda-se testar o filtro:

```bash
weka.filters.supervised.attribute.AttributeSelection
````

Esse filtro pode ser usado para avaliar a relevância dos atributos em relação à classe-alvo. Ele é adequado porque o teste piloto identificou:

* atributos irrelevantes planejados;
* possível redundância entre `active_power_w` e `energy_consumption_kwh`;
* possível dominância de `rack_power_density_kw`;
* atributos com diferentes níveis de contribuição para a classe.

O uso desse filtro não substitui a análise manual realizada no teste piloto. Ele será utilizado como apoio para comparar se o Weka também identifica atributos pouco relevantes ou dominantes.

Uma alternativa possível é usar:

```bash
weka.filters.unsupervised.attribute.Discretize
```

Esse filtro pode ser testado em uma versão alternativa do dataset, principalmente para algoritmos que podem se beneficiar de faixas discretas de atributos contínuos. Porém, ele deve ser usado com cuidado, pois pode reduzir informação numérica importante.

## Estratégia de geração do dataset preprocessado

A partir dos achados do teste piloto, será criada uma versão preprocessada do dataset:

```bash
dataset/dataset_preprocessado.arff
```

Essa versão deverá conter:

* valores faltantes tratados;
* atributos irrelevantes removidos;
* atributos numéricos normalizados ou padronizados, quando necessário;
* outliers interpretáveis preservados;
* relações semânticas principais mantidas;
* classe-alvo preservada.

O dataset original será mantido sem alterações como referência:

```bash
dataset/dataset_original.arff
```

## Justificativa para manter ruídos e outliers interpretáveis

Os ruídos e outliers observados não serão removidos automaticamente porque fazem parte da proposta do dataset sintético. O trabalho exige a presença desses elementos, e a análise mostrou que eles são, em sua maioria, plausíveis e relacionados ao domínio.

A manutenção desses registros é importante porque a classe `alto` depende justamente de situações críticas, como alto consumo, baixa utilização relativa, temperatura elevada, jobs longos e alta densidade energética. Remover esses casos poderia empobrecer o dataset e tornar a classificação artificialmente simples.

Portanto, a decisão correta não é eliminar todos os outliers, mas distinguir entre:

| Tipo de caso                        | Decisão                              |
| ----------------------------------- | ------------------------------------ |
| Outlier interpretável               | Manter                               |
| Ruído leve e plausível              | Manter                               |
| Valor faltante                      | Tratar com filtro                    |
| Atributo irrelevante                | Remover                              |
| Valor impossível ou erro estrutural | Corrigir ou remover, se identificado |
| Atributo dominante                  | Manter inicialmente e monitorar      |

## Conclusão

O Dataset V2 está apto para seguir para a fase de pré-processamento. A análise inicial cumpriu seu papel ao identificar valores faltantes, ruídos, outliers, atributos irrelevantes, possível dominância de atributos e relações semânticas relevantes.

A próxima fase não consistirá em uma limpeza automática da base, mas em um pré-processamento justificado por evidências. As principais ações serão: aplicar `ReplaceMissingValues`, remover atributos irrelevantes com `Remove`, aplicar normalização ou padronização para algoritmos sensíveis à escala e testar pelo menos um filtro adicional do Weka, como `AttributeSelection`.

Com essas decisões, o dataset preprocessado deverá ficar mais adequado para a etapa de modelagem, preservando os padrões relevantes do problema e reduzindo interferências artificiais que poderiam prejudicar a comparação entre os algoritmos de aprendizado de máquina.
