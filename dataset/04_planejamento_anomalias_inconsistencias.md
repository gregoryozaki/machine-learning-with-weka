# Planejamento da Inserção de Valores Faltantes, Ruídos e Outliers

## Estratégia para Valores Faltantes

Valores faltantes serão inseridos intencionalmente para simular falhas de sensores, ausência de telemetria ou indisponibilidade parcial de dados operacionais.

A proporção planejada de valores faltantes será baixa, aproximadamente entre **3% e 7% do dataset**, para não comprometer a estrutura geral dos dados nem inviabilizar a etapa posterior de pré-processamento.

| Atributo | Estratégia | Justificativa |
|---|---|---|
| `gpu_temperature_c` | Inserir `N/A` em parte dos registros | Sensores térmicos podem falhar ou deixar de registrar temperatura em determinados períodos |
| `fan_speed_rpm` | Inserir `N/A` em parte dos registros | A telemetria dos ventiladores pode estar indisponível em alguns registros |
| `water_usage_effectiveness` | Inserir `N/A` em parte dos registros | Métricas ambientais podem não estar disponíveis para todos os racks ou períodos |
| `carbon_intensity_gco2_kwh` | Inserir `N/A` em parte dos registros | A intensidade de carbono pode depender da região ou da fonte energética e pode não estar disponível em todos os casos |
| `job_status` | Inserir `N/A` em poucos registros | O estado final do job pode estar ausente em registros incompletos ou ainda em execução |

Os valores faltantes serão representados por:

```bash
N/A
```

Os valores faltantes não serão removidos nesta etapa, pois fazem parte do dataset original. O tratamento desses valores será realizado posteriormente na etapa de pré-processamento.

---

## Estratégia para Ruído

O ruído será inserido de forma controlada para simular variações de medição, imprecisão de sensores ou pequenas oscilações operacionais.

A proporção planejada de registros com ruído será moderada, aproximadamente entre **5% e 10% do dataset**. O ruído não deve alterar a classe de forma arbitrária nem destruir a coerência semântica do registro.

| Tipo de ruído | Atributo afetado | Estratégia | Justificativa |
| --- | --- | --- | --- |
| Ruído energético leve | `active_power_w`, `energy_consumption_kwh`, `gpu_power_w` | Aplicar pequenas variações numéricas nos valores gerados | Consumo energético pode oscilar durante a operação do rack |
| Ruído de utilização | `cpu_utilization_percent`, `memory_utilization_percent`, `gpu_utilization_percent` | Aplicar pequenas variações percentuais | Medidas de uso computacional variam naturalmente ao longo do tempo |
| Ruído térmico | `inlet_temperature_c`, `exhaust_temperature_c`, `gpu_temperature_c`, `delta_t_c` | Aplicar pequenas variações térmicas | Sensores térmicos podem apresentar pequenas oscilações de leitura |
| Ruído operacional | `fan_speed_rpm`, `gpu_core_frequency_mhz` | Aplicar pequenas variações em rotação e frequência | Frequência da GPU e rotação dos fans podem oscilar conforme carga e controle térmico |

O ruído deverá permanecer dentro de uma margem plausível, sem gerar valores fisicamente impossíveis ou contraditórios com as regras semânticas do dataset.

O ruído inserido será mantido no dataset original para posterior análise e tratamento durante a etapa de pré-processamento.

---

## Estratégia para Outliers

Os outliers serão inseridos intencionalmente para representar instâncias distantes da distribuição principal ou combinações incomuns entre atributos.

A proporção planejada de outliers será baixa, aproximadamente entre **3% e 5% do dataset**. Os outliers devem ser interpretáveis no contexto de racks de datacenters voltados a cargas de IA.

| Tipo de outlier | Descrição | Justificativa |
| --- | --- | --- |
| Outlier energético | Alta potência com baixa utilização de CPU/GPU | Representa desperdício por subutilização de recursos energéticos |
| Outlier térmico | Alta temperatura de GPU ou exaustão mesmo com fan speed elevado | Representa possível hotspot ou refrigeração ineficiente |
| Outlier operacional | `job_status = failed` ou `aborted` com longa duração do job | Representa energia consumida sem resultado útil |
| Outlier ambiental | Alto consumo energético combinado com alta intensidade de carbono ou alto WUE | Representa maior impacto ambiental associado à operação |
| Outlier de refrigeração | Fan speed alto com baixa carga computacional | Representa refrigeração excessiva ou mal ajustada |
| Outlier de alocação de GPU | GPU inteira alocada com baixa utilização | Representa má alocação de aceleradores em cargas pequenas |
| Outlier de densidade de rack | Alta densidade de potência com refrigeração a ar e temperaturas elevadas | Representa risco térmico em rack de IA de alta densidade |

Exemplos possíveis de outliers planejados:

* consumo muito alto com baixa utilização de GPU;
* temperatura muito alta mesmo com refrigeração intensa;
* fan speed elevado com workload leve;
* job falho após longa duração;
* alta intensidade de carbono combinada com alto consumo energético;
* uso de GPU inteira para workload pequeno ou ocioso;
* alta densidade de potência com método de refrigeração inadequado.

Os outliers não serão removidos nesta etapa. Eles devem permanecer no dataset original para posterior identificação, análise e tratamento durante a etapa de pré-processamento.
