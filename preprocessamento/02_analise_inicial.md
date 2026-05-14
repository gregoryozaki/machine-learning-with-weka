# Análise inicial - Dataset v2

#### Responsável: `Gregory Ozaki`

## Objetivo

Esta análise registra o teste piloto do `dataset/dataset_original.arff` antes da aplicação de qualquer técnica de pré-processamento.

O arquivo corresponde ao **dataset v2**, criado após a análise preliminar da **v1**. A v1 estava estruturalmente correta, mas apresentava valores faltantes, ruído e outliers em baixa proporção. Por isso, a v2 foi produzida como uma iteração metodológica, preservando a estrutura original e aumentando de forma controlada as imperfeições necessárias ao trabalho.

O dataset v2 contém:

- registros sintéticos gerados com apoio de LLM;
- valores faltantes planejados;
- ruído controlado;
- outliers interpretáveis;
- inconsistências pontuais;
- atributos irrelevantes;
- classe-alvo `environmental_waste_risk_level`.

A unidade de análise permanece a mesma: **um rack de datacenter em uma hora de operação**.

---

# Etapa 1 - Verificação de integridade estrutural

## Objetivo da etapa

Verificar se o dataset abre corretamente no Weka e se sua estrutura está compatível com o planejamento.

## Verificações realizadas

| Nº | Verificação | Resultado observado | Status |
|---:|---|---|---|
| 1 | Carregamento no Weka | O arquivo `.arff` abriu corretamente | ✅ |
| 2 | Número de instâncias | 674 instâncias | ✅ |
| 3 | Número de atributos | 30 atributos | ✅ |
| 4 | Classe-alvo | `environmental_waste_risk_level` reconhecida como nominal | ✅ |
| 5 | Tipos dos atributos | Numéricos como `numeric` e categóricos como `nominal` | ✅ |
| 6 | Valores faltantes | Presentes apenas nos atributos planejados | ✅ |
| 7 | Categorias válidas | Não foram identificadas categorias inválidas | ✅ |
| 8 | Linhas quebradas | Não houve erro de leitura ou deslocamento aparente de colunas | ✅ |
| 9 | Duplicatas | Não foram identificadas instâncias duplicadas | ✅ |

## Evidências

![figura 01 - integridade no Weka](/imagens/prints_weka/preprocessamento/figura_01_integridade_weka.png)

![figura 02 - instâncias e atributos](/imagens/prints_weka/preprocessamento/figura_02_instancias_atributos.png)

![figura 03 - classe nominal](/imagens/prints_weka/preprocessamento/figura_03_classe_nominal.png)

![figura 04 - valores faltantes planejados](/imagens/prints_weka/preprocessamento/figura_04_valores_faltantes_planejados.png)

## Interpretação

O dataset v2 está estruturalmente válido para análise exploratória no Weka. Não há indícios de problemas técnicos graves, como linhas quebradas, colunas deslocadas, categorias inválidas ou duplicatas em excesso.

Os problemas observados nesta etapa são esperados no contexto do trabalho, principalmente a presença de valores faltantes planejados.

## Achado principal

| ID | Eixo | Achado | Impacto no pré-processamento | Ação sugerida |
|---|---|---|---|---|
| A1 | Integridade estrutural | Dataset carregado corretamente, com 674 instâncias e 30 atributos | Permite seguir para análise exploratória | Manter o arquivo original como referência |

---

# Etapa 2 - Análise estatística descritiva

## Objetivo da etapa

Investigar mínimos, máximos, médias, desvios-padrão e distribuições dos atributos, buscando valores inválidos, dispersões relevantes e possíveis impactos para o pré-processamento.

## Estatísticas principais

| Atributo | Mínimo | Máximo | Média | Desvio padrão | Interpretação |
|---|---:|---:|---:|---:|---|
| `active_power_w` | 600 | 11980 | 5831,804 | 2955,029 | Alta variação de potência ativa. |
| `energy_consumption_kwh` | 0,60 | 12,00 | 5,798 | 2,913 | Compatível com janela de uma hora. |
| `water_usage_effectiveness` | 0,28 | 4,96 | 1,262 | 0,672 | Variação ambiental relevante. |
| `carbon_intensity_gco2_kwh` | 62 | 893 | 381,536 | 165,111 | Alta variação de impacto climático. |
| `inlet_temperature_c` | 18,20 | 31,00 | 23,996 | 2,466 | Faixa operacional plausível. |
| `exhaust_temperature_c` | 25,80 | 74,00 | 51,785 | 9,641 | Maior dispersão térmica. |
| `delta_t_c` | 6,60 | 43,00 | 27,764 | 7,522 | Diferença térmica relevante. |
| `fan_speed_rpm` | 1666 | 22000 | 11567,525 | 4266,54 | Alto esforço de refrigeração em parte dos registros. |
| `cpu_utilization_percent` | 4 | 87 | 51,794 | 19,283 | Dentro da faixa esperada. |
| `memory_utilization_percent` | 12 | 92 | 64,239 | 16,279 | Uso de memória coerente com workloads de IA. |
| `gpu_power_w` | 51 | 670 | 395,798 | 152,568 | Alta variação de potência da GPU. |
| `gpu_utilization_percent` | 2 | 94 | 55,104 | 23,394 | Importante para identificar subutilização. |
| `gpu_temperature_c` | 32 | 95 | 74,807 | 13,424 | Temperaturas elevadas em parte dos registros. |
| `gpu_core_frequency_mhz` | 600 | 2147 | 1531,74 | 283,755 | Frequência operacional plausível. |
| `num_gpus` | 1 | 16 | 7,315 | 5,035 | Numérico com comportamento discreto. |
| `batch_size` | 1 | 2048 | 446,712 | 466,664 | Distribuição assimétrica. |
| `num_epochs` | 1 | 150 | 29,076 | 38,587 | Treinamentos curtos e alguns longos. |
| `model_parameter_size_million` | 7 | 67766 | 13174,754 | 16563,193 | Modelos de diferentes escalas. |
| `training_samples` | 3160 | 97744348 | 20492296,918 | 27872744,296 | Grande variação de volume de dados. |
| `job_duration_hours` | 0,06 | 185,07 | 33,754 | 41,821 | Jobs curtos e alguns muito longos. |
| `rack_power_density_kw` | 5 | 120 | 27,274 | 27,498 | Alta dispersão e possível dominância. |
| `power_cap_w` | 660 | 12000 | 7027,826 | 3353,017 | Grande variação nos limites de potência. |

## Evidências principais

![active_power_w](/imagens/prints_weka/preprocessamento/figura_05_active_power_w.png)

![energy_consumption_kwh](/imagens/prints_weka/preprocessamento/figura_06_energy_consumption_kwh.png)

![fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_12_fan_speed_rpm.png)

![gpu_utilization_percent](/imagens/prints_weka/preprocessamento/figura_17_gpu_utilization_percent.png)

![gpu_temperature_c](/imagens/prints_weka/preprocessamento/figura_18_gpu_temperature_c.png)

![job_duration_hours](/imagens/prints_weka/preprocessamento/figura_26_job_duration_hours.png)

![rack_power_density_kw](/imagens/prints_weka/preprocessamento/figura_28_rack_power_density_kw.png)

## Interpretação

Os atributos permanecem dentro de faixas plausíveis para o domínio. Não foram observados valores negativos, percentuais acima de 100%, categorias inválidas ou valores numericamente impossíveis.

Os atributos energéticos `active_power_w` e `energy_consumption_kwh` apresentam comportamento semelhante, o que é esperado para registros de uma hora de operação. Essa relação pode indicar redundância parcial.

Os atributos térmicos e operacionais, como `gpu_temperature_c`, `fan_speed_rpm`, `delta_t_c` e `job_duration_hours`, apresentam dispersão relevante. Isso é coerente com diferentes perfis de carga, refrigeração e duração de workloads.

Atributos como `batch_size`, `model_parameter_size_million` e `training_samples` são assimétricos e possuem escalas elevadas. Por isso, podem exigir normalização ou padronização antes de algoritmos sensíveis à escala, como KNN e SVM.

O atributo `num_gpus` foi reconhecido como numérico, mas possui comportamento discreto, representando quantidades específicas de GPUs por rack. Por isso, ele será convertido para nominal no pré-processamento com o filtro `NumericToNominal`, evitando que seja tratado como uma variável contínua.

## Implicações para o pré-processamento

| Ponto observado | Impacto | Decisão a avaliar |
|---|---|---|
| Valores faltantes em atributos ambientais, térmicos e operacionais | Modelos podem não lidar bem com ausência de dados | Aplicar `ReplaceMissingValues` |
| Escalas numéricas muito diferentes | Pode afetar KNN e SVM | Aplicar normalização ou padronização |
| Relação forte entre `active_power_w` e `energy_consumption_kwh` | Pode indicar redundância | Avaliar influência na modelagem |
| Alta dispersão em `rack_power_density_kw` | Pode dominar a classificação | Manter e monitorar |
| Distribuições assimétricas em atributos de workload | Pode afetar modelos sensíveis à escala | Normalizar/padronizar |
| `num_gpus` discreto | Pode ser interpretado indevidamente como variável contínua | Converter para nominal com `NumericToNominal` |

---

# Etapa 3 - Análise de valores faltantes

## Objetivo da etapa

Verificar se os valores faltantes foram inseridos conforme o planejamento e definir encaminhamentos para o pré-processamento.

## Valores faltantes identificados

| Atributo | Tipo | Valores faltantes | Percentual aproximado | Interpretação |
|---|---|---:|---:|---|
| `job_status` | Nominal | 26 | 4% | Registro operacional incompleto ou indefinido. |
| `gpu_temperature_c` | Numeric | 32 | 5% | Falha de sensor térmico ou ausência de telemetria. |
| `fan_speed_rpm` | Numeric | 32 | 5% | Falha de leitura dos ventiladores. |
| `carbon_intensity_gco2_kwh` | Numeric | 32 | 5% | Informação ambiental externa ou regional indisponível. |
| `water_usage_effectiveness` | Numeric | 32 | 5% | Métrica ambiental não disponível em todos os registros. |

Total identificado:

```bash
154 valores faltantes
```

Proporção geral aproximada:

```bash
154 / 20220 ≈ 0,76% das células do dataset
```

## Evidências

![gpu_temperature_c missing](/imagens/prints_weka/preprocessamento/figura_36_gpu_temperature_c_missing_data.png)

![fan_speed_rpm missing](/imagens/prints_weka/preprocessamento/figura_37_fan_speed_rpm_missing_data.png)

![carbon_intensity_gco2_kwh missing](/imagens/prints_weka/preprocessamento/figura_38_carbon_intensity_gco2_kwh_missing_data.png)

![water_usage_effectiveness missing](/imagens/prints_weka/preprocessamento/figura_39_water_usage_effectiveness_missing_data.png)

![job_status missing](/imagens/prints_weka/preprocessamento/figura_35_job_status_missing_data.png)

## Interpretação

Os valores faltantes aparecem apenas nos atributos planejados. A ausência de dados é controlada e semanticamente plausível, pois afeta sensores, métricas ambientais e registros operacionais.

A quantidade de faltantes é maior que na v1, tornando a etapa de pré-processamento mais evidente, mas a proporção geral permanece baixa.

## Decisão para o pré-processamento

| Tipo de atributo | Atributos | Estratégia |
|---|---|---|
| Numérico | `gpu_temperature_c`, `fan_speed_rpm`, `carbon_intensity_gco2_kwh`, `water_usage_effectiveness` | Aplicar `ReplaceMissingValues` no Weka |
| Nominal | `job_status` | Aplicar `ReplaceMissingValues` no Weka |

O filtro `ReplaceMissingValues` será usado na versão inicial do dataset preprocessado por ser compatível com atributos numéricos e nominais no Weka.

---

# Etapa 4 - Análise de ruído

## Objetivo da etapa

Verificar se o ruído inserido no dataset é plausível e compatível com o domínio. Nesta etapa, ruído é entendido como variação ou desvio que não invalida o registro.

## Relações analisadas

| Relação | Critério esperado |
|---|---|
| `active_power_w × energy_consumption_kwh` | Energia próxima da potência dividida por 1000 em uma hora. |
| `inlet_temperature_c`, `exhaust_temperature_c` e `delta_t_c` | `delta_t_c` deve acompanhar a diferença entre entrada e exaustão. |
| `gpu_power_w × gpu_utilization_percent` | Maior uso tende a maior potência, com exceções possíveis. |
| `gpu_temperature_c × fan_speed_rpm` | Temperaturas maiores tendem a exigir maior rotação dos fans. |
| `gpu_core_frequency_mhz × gpu_power_w` | Frequências maiores tendem a maior potência. |
| `active_power_w × power_cap_w` | Potência ativa tende a acompanhar limites configurados. |
| `job_duration_hours × job_status` | Jobs falhos, abortados ou em execução podem ter duração elevada. |

## Evidências principais

![active_power_w x energy_consumption_kwh](/imagens/prints_weka/preprocessamento/figura_40_active_power_X_energy_consumption.png)

![inlet_temperature_c x delta_t_c](/imagens/prints_weka/preprocessamento/figura_41_inlet_temperature_X_delta_t_.png)

![exhaust_temperature_c x delta_t_c](/imagens/prints_weka/preprocessamento/figura_42_exhaust_temperature_X_delta_t.png)

![gpu_power_w x gpu_utilization_percent](/imagens/prints_weka/preprocessamento/figura_43_gpu_power_X_gpu_utilization_percent.png)

![gpu_temperature_c x fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_44_gpu_temperature_X_fan_speed.png)

![job_duration_hours x job_status](/imagens/prints_weka/preprocessamento/figura_48_job_duration_hours_X_job_status.png)

## Interpretação

As principais relações físicas e operacionais foram preservadas:

- `energy_consumption_kwh` acompanha `active_power_w`;
- `delta_t_c` mantém coerência com as temperaturas de entrada e exaustão;
- `gpu_power_w` mantém relação plausível com utilização e frequência da GPU;
- `fan_speed_rpm` tende a acompanhar temperatura, ainda que com dispersão;
- `power_cap_w` apresenta coerência geral com a potência ativa;
- jobs `failed`, `aborted` ou `running` podem apresentar maior duração.

Os desvios observados não invalidam o dataset. Eles podem representar ruído controlado, erro de medição, falha de telemetria, configuração operacional inadequada ou outliers relacionais planejados.

## Impacto no pré-processamento

| Ponto observado | Interpretação | Decisão |
|---|---|---|
| Desvios entre potência e energia | Ruído energético leve | Manter e documentar |
| Desvios térmicos | Ruído térmico ou atraso de resposta | Manter e observar na análise de outliers |
| Alta potência de GPU com baixa utilização | Possível desperdício energético | Manter como informação relevante |
| Fan speed alto com baixa/média utilização | Refrigeração excessiva ou temperatura residual | Manter e analisar junto da temperatura |
| Jobs longos com falha ou aborto | Possível desperdício operacional | Manter |

---

# Etapa 5 - Análise de outliers

## Objetivo da etapa

Identificar valores extremos e combinações incomuns, distinguindo erros reais de situações críticas interpretáveis.

Outliers não serão removidos automaticamente. A decisão será baseada na plausibilidade técnica e no impacto para a classificação.

## Atributos e relações analisados

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

```bash
active_power_w × energy_consumption_kwh
gpu_power_w × gpu_utilization_percent
gpu_temperature_c × fan_speed_rpm
job_duration_hours × job_status
rack_power_density_kw × environmental_waste_risk_level
active_power_w × gpu_utilization_percent
```

## Evidências principais

![fan_speed_rpm](/imagens/prints_weka/preprocessamento/figura_12_fan_speed_rpm.png)

![gpu_temperature_c](/imagens/prints_weka/preprocessamento/figura_18_gpu_temperature_c.png)

![job_duration_hours](/imagens/prints_weka/preprocessamento/figura_26_job_duration_hours.png)

![rack_power_density_kw](/imagens/prints_weka/preprocessamento/figura_28_rack_power_density_kw.png)

![gpu_power_w x gpu_utilization_percent](/imagens/prints_weka/preprocessamento/figura_43_gpu_power_X_gpu_utilization_percent.png)

![active_power_w x gpu_utilization_percent](/imagens/prints_weka/preprocessamento/figura_50_active_power_X_gpu_utilization_percent.png)

## Síntese dos outliers observados

| Atributo ou relação | Evidência | Interpretação | Decisão |
|---|---|---|---|
| `active_power_w` | Máximo de 11980 W | Alta carga energética | Manter |
| `energy_consumption_kwh` | Máximo de 12 kWh | Consumo compatível com uma hora de operação | Manter |
| `fan_speed_rpm` | Máximo de 22000 rpm | Alto esforço de refrigeração | Manter |
| `gpu_temperature_c` | Máximo de 95°C | Situação térmica crítica | Manter |
| `batch_size` | Máximo de 2048 | Configuração elevada, mas plausível | Manter |
| `model_parameter_size_million` | Máximo de 67766 milhões de parâmetros | Modelo de grande porte | Manter |
| `training_samples` | Máximo de 97744348 amostras | Grande volume de treinamento | Manter |
| `job_duration_hours` | Máximo de 185,07 horas | Job prolongado ou execução ineficiente | Manter |
| `rack_power_density_kw` | Máximo de 120 kW | Rack de alta densidade energética | Manter e monitorar |
| `gpu_power_w × gpu_utilization_percent` | Alta potência com baixa/média utilização | Possível desperdício ambiental | Manter |
| `active_power_w × gpu_utilization_percent` | Alta potência com baixa/média utilização | Baixa eficiência computacional | Manter |

## Interpretação

Os valores extremos observados são plausíveis no domínio de datacenters voltados a cargas de IA. Eles representam situações como alto esforço de refrigeração, temperaturas críticas, jobs prolongados, modelos de grande porte, grande volume de amostras e racks de alta densidade.

Remover esses casos automaticamente poderia eliminar registros importantes para a classe `alto`. Portanto, a decisão nesta fase é manter os outliers interpretáveis e tratar apenas problemas objetivos, como valores faltantes e atributos irrelevantes.

## Impacto no pré-processamento

Os outliers interpretáveis serão preservados na versão inicial do dataset preprocessado. A ação principal será normalizar ou padronizar atributos numéricos para reduzir problemas de escala em algoritmos como KNN e SVM.

---

# Etapa 6 - Análise da classe-alvo

## Objetivo da etapa

Analisar a distribuição da classe `environmental_waste_risk_level` e verificar se as categorias `baixo`, `moderado` e `alto` apresentam coerência com o problema.

## Distribuição da classe-alvo

![environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_34_environmental_waste_risk_level.png)

| Classe | Quantidade | Interpretação |
|---|---:|---|
| `baixo` | 268 | Classe mais frequente |
| `moderado` | 248 | Classe intermediária |
| `alto` | 158 | Classe menos frequente |

Há desbalanceamento moderado. Ele não impede a modelagem, mas exige avaliação além da acurácia, principalmente matriz de confusão, precisão, recall e F1-score por classe.

## Relações com a classe

![active_power_w x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_51_active_power_X_environmental_waste_risk_level.png)

![energy_consumption_kwh x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_52_energy_consumption_X_environmental_waste_risk_level.png)

![gpu_utilization_percent x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_53_gpu_utilization_X_environmental_waste_risk_level.png)

![gpu_temperature_c x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_54_gpu_temperature_X_environmental_waste_risk_level.png)

![rack_power_density_kw x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_49_rack_power_density_X_environmental_waste_risk_level.png)

## Interpretação

As classes apresentam separação parcial em atributos energéticos, térmicos e computacionais:

- `baixo`: menor consumo, menor temperatura, menor densidade energética ou maior aproveitamento computacional;
- `moderado`: valores intermediários ou combinações parcialmente críticas;
- `alto`: maior consumo, maior temperatura, maior densidade energética ou baixa utilização relativa.

A separação não é absoluta, o que é positivo. A sobreposição entre classes torna o problema menos artificial e mais adequado para comparação entre algoritmos.

O atributo `rack_power_density_kw` apresenta forte associação visual com a classe-alvo. Ele será mantido, mas sua influência deverá ser monitorada na modelagem.

## Impacto na avaliação dos modelos

A classe `alto` representa os casos de maior risco ambiental e possui menor frequência. Por isso, o desempenho dos algoritmos deverá ser avaliado com atenção especial ao **recall da classe `alto`**, além da acurácia geral.

---

# Etapa 7 - Análise dos atributos irrelevantes

## Objetivo da etapa

Verificar se os atributos planejados como irrelevantes realmente não possuem relação semântica direta com a classe-alvo.

## Atributos analisados

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

## Evidências

![manufacturer_sku_id](/imagens/prints_weka/preprocessamento/figura_31_manufacturer_sku_id.png)

![rack_label_color](/imagens/prints_weka/preprocessamento/figura_32_rack_label_color.png)

![rack_inventory_zone](/imagens/prints_weka/preprocessamento/figura_33_rack_inventory_zone.png)

![manufacturer_sku_id x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_55_manufacturer_sku_id_X_environmental_waste_risk_level.png)

![rack_label_color x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_56_rack_label_color_X_environmental_waste_risk_level.png)

![rack_inventory_zone x environmental_waste_risk_level](/imagens/prints_weka/preprocessamento/figura_57_rack_inventory_zone_X_environmental_waste_risk_level.png)

## Interpretação

Os três atributos representam informações administrativas ou artificiais do rack. Eles não explicam diretamente consumo energético, temperatura, utilização computacional, refrigeração, densidade de potência ou duração dos jobs.

Mesmo que alguma associação visual apareça, ela deve ser tratada como possível viés sintético, e não como relação causal válida.

## Decisão

| Atributo | Tipo de informação | Decisão |
|---|---|---|
| `manufacturer_sku_id` | Identificador de fabricante/modelo | Remover |
| `rack_label_color` | Cor da etiqueta do rack | Remover |
| `rack_inventory_zone` | Zona administrativa/física | Remover |

Esses atributos serão removidos no pré-processamento principal com o filtro `Remove` do Weka.

---

# Etapa 8 - Análise de relações semânticas

## Objetivo da etapa

Verificar se as principais relações utilizadas na construção do dataset aparecem de forma coerente nos dados.

## Relações analisadas

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

## Interpretação consolidada

As relações analisadas indicam coerência geral no dataset:

- `active_power_w` e `energy_consumption_kwh` preservam coerência energética;
- `inlet_temperature_c`, `exhaust_temperature_c` e `delta_t_c` preservam coerência térmica;
- `gpu_power_w` e `gpu_utilization_percent` ajudam a identificar consumo sem aproveitamento proporcional;
- `gpu_temperature_c` e `fan_speed_rpm` indicam esforço de refrigeração plausível;
- `job_duration_hours` e `job_status` ajudam a caracterizar desperdício operacional;
- `rack_power_density_kw` apresenta relação forte com a classe `alto`, mas deve ser monitorado;
- `gpu_utilization_percent` é central para diferenciar uso eficiente de desperdício.

## Síntese das relações semânticas

| Relação | Interpretação | Decisão |
|---|---|---|
| `active_power_w × energy_consumption_kwh` | Coerência energética preservada | Manter |
| `inlet_temperature_c × delta_t_c` | Relação plausível, dependente de múltiplos fatores | Manter |
| `exhaust_temperature_c × delta_t_c` | Coerência térmica mais clara | Manter |
| `gpu_power_w × gpu_utilization_percent` | Útil para identificar desperdício ambiental | Manter |
| `gpu_temperature_c × fan_speed_rpm` | Esforço de refrigeração plausível | Manter |
| `job_duration_hours × job_status` | Possível desperdício operacional | Manter |
| `active_power_w × gpu_utilization_percent` | Relação central para desperdício | Manter |
| `rack_power_density_kw × environmental_waste_risk_level` | Coerente, mas possivelmente dominante | Manter e monitorar |

## Decisão da etapa

O dataset v2 preserva relações semânticas coerentes com o domínio. A classe-alvo parece depender da combinação entre consumo energético, utilização computacional, temperatura, esforço de refrigeração, duração dos jobs e densidade de potência.

Assim, o dataset é adequado para seguir ao pré-processamento, desde que as transformações preservem essas relações principais.

---

# Síntese geral do teste piloto e encaminhamento para o pré-processamento

## Síntese do teste piloto

O teste piloto mostrou que o dataset v2 está estruturalmente válido, contém imperfeições controladas e mantém coerência semântica com o problema de classificação do risco de desperdício ambiental.

Foram confirmados:

- valores faltantes planejados;
- ruído plausível;
- outliers interpretáveis;
- atributos irrelevantes;
- desbalanceamento moderado da classe-alvo;
- relações energéticas, térmicas, computacionais e operacionais coerentes.

A análise também mostrou que pré-processar não significa remover todos os ruídos e outliers. Alguns registros extremos representam situações críticas importantes para a classe `alto`, como alta potência, baixa utilização relativa, temperatura elevada, jobs longos e alta densidade energética.

## Decisões de pré-processamento

| Problema identificado | Decisão | Filtro/ação no Weka |
|---|---|---|
| Valores faltantes | Tratar antes do treinamento | `ReplaceMissingValues` |
| Atributos irrelevantes | Remover | `Remove` |
| Escalas muito diferentes | Ajustar escala | `Normalize` ou `Standardize` |
| `num_gpus` com comportamento discreto | Converter para atributo nominal | `NumericToNominal` |
| Outliers interpretáveis | Manter inicialmente | Monitorar nos modelos |
| Ruído leve e plausível | Manter | Documentar |
| `rack_power_density_kw` possivelmente dominante | Manter e monitorar | Avaliar impacto nos resultados |
| Possível redundância entre `active_power_w` e `energy_consumption_kwh` | Manter inicialmente | Avaliar na modelagem |

## Filtros do Weka previstos

| Ordem | Filtro | Finalidade |
|---:|---|---|
| 1 | `ReplaceMissingValues` | Substituir valores ausentes em atributos numéricos e nominais. |
| 2 | `Remove` | Remover atributos irrelevantes confirmados. |
| 3 | `NumericToNominal` | Converter `num_gpus` para nominal, por representar uma quantidade discreta de GPUs. |
| 4 | `Normalize` ou `Standardize` | Ajustar escalas numéricas, principalmente para KNN e SVM. |
| 5 | `AttributeSelection` | Apoiar a análise de relevância dos atributos e atender ao requisito de filtro adicional. |

## Justificativa para manter ruídos e outliers interpretáveis

Ruídos e outliers não serão removidos automaticamente porque fazem parte da proposta do dataset sintético e foram considerados plausíveis na análise exploratória.

A decisão correta é distinguir:

| Tipo de caso | Decisão |
|---|---|
| Valor faltante | Tratar com filtro |
| Atributo irrelevante | Remover |
| Ruído leve e plausível | Manter |
| Outlier interpretável | Manter |
| Valor impossível ou erro estrutural | Corrigir/remover, se identificado |
| Atributo dominante | Manter inicialmente e monitorar |

## Arquivos resultantes

O dataset original será preservado como referência:

```bash
dataset/dataset_original.arff
```

A versão após o pré-processamento será salva como:

```bash
dataset/dataset_preprocessado.arff
```

## Conclusão

O dataset v2 está apto para seguir para a fase de pré-processamento. A próxima etapa não será uma limpeza automática da base, mas a aplicação justificada de filtros do Weka.

As ações principais serão: tratar valores faltantes com `ReplaceMissingValues`, remover atributos irrelevantes com `Remove`, converter `num_gpus` para nominal com `NumericToNominal`, ajustar escalas com `Normalize` ou `Standardize` e testar `AttributeSelection` como filtro adicional.

Com isso, o dataset preprocessado deverá preservar os padrões relevantes do problema, reduzir interferências artificiais e ficar mais adequado para a comparação entre os algoritmos de aprendizado de máquina.
