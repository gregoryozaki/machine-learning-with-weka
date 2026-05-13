# Análise Inicial — dataset v2

## 1. Objetivo

Esta análise tem como objetivo realizar o teste piloto da segunda versão do dataset sintético, denominada **dataset v2**, antes da aplicação de qualquer técnica de pré-processamento.

O teste piloto busca investigar a estrutura do dataset, suas distribuições, valores faltantes, ruídos, outliers, inconsistências e relações entre atributos, de modo que as decisões posteriores de pré-processamento sejam guiadas por evidências observadas nos dados.

O dataset analisado corresponde ao arquivo:

```bash
dataset/dataset_original_v2.arff
````

Esse arquivo representa a segunda versão do dataset sintético, criada após a análise preliminar do dataset v1. A v2 preserva a estrutura original do dataset, mas possui maior grau de imperfeições controladas, com o objetivo de tornar mais evidente a etapa de teste piloto e pré-processamento.

O dataset v2 contém:

* registros sintéticos gerados com apoio de LLM;
* valores faltantes inseridos de forma controlada;
* ruído controlado;
* outliers interpretáveis;
* algumas inconsistências controladas;
* atributos irrelevantes planejados;
* classe-alvo `environmental_waste_risk_level`.

---

## 2. Caracterização do Dataset

O dataset foi construído para a tarefa de **classificação do nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de IA**.

Cada instância representa:

> Um rack de datacenter em uma hora de operação.

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

O dataset v2 possui:

| Item                     | Descrição                        |
| ------------------------ | -------------------------------- |
| Quantidade de instâncias | 674                              |
| Quantidade de atributos  | 30                               |
| Tipo da tarefa           | Classificação                    |
| Ferramenta utilizada     | Weka                             |
| Formato do dataset       | ARFF                             |
| Classe-alvo              | `environmental_waste_risk_level` |
| Classes                  | `baixo`, `moderado`, `alto`      |

---

## 3. Justificativa da Análise do dataset v2

A primeira versão do dataset, denominada dataset v1, foi analisada previamente por três integrantes da equipe. Essa análise indicou que a v1 estava estruturalmente correta, era compatível com o Weka e continha valores faltantes, ruído e outliers conforme planejado.

Entretanto, os achados também indicaram que essas imperfeições estavam presentes em baixa proporção e com baixa severidade. Por esse motivo, foi criada a segunda versão do dataset, denominada dataset v2.

A v2 foi criada como uma iteração metodológica, e não como uma substituição arbitrária da v1. O objetivo foi aumentar, de forma controlada, a presença de:

* valores faltantes;
* ruído;
* outliers relacionais;
* inconsistências interpretáveis.

A v2 preserva:

* a mesma quantidade de instâncias;
* a mesma quantidade de atributos;
* a mesma classe-alvo;
* as mesmas categorias válidas;
* a compatibilidade com Weka;
* a mesma unidade de análise.

Assim, esta análise inicial da v2 será utilizada para verificar se o dataset está adequado para seguir para a etapa de pré-processamento.

---

# 4. Etapas do Teste Piloto

## 4.1. Etapa 1 — Verificação de Integridade Estrutural

### Objetivo

Verificar se o dataset v2 está corretamente estruturado e pode ser utilizado no Weka sem erros técnicos.

### Resultado da verificação

| Nº | Verificação          | Resultado esperado                                        | Resultado observado | Status    |
| -: | -------------------- | --------------------------------------------------------- | ------------------- | --------- |
|  1 | Carregamento no Weka | O arquivo `.arff` deve abrir corretamente                 | preencher           | preencher |
|  2 | Número de instâncias | 674 instâncias                                            | preencher           | preencher |
|  3 | Número de atributos  | 30 atributos                                              | preencher           | preencher |
|  4 | Classe-alvo          | `environmental_waste_risk_level` reconhecida como nominal | preencher           | preencher |
|  5 | Tipos dos atributos  | Numéricos como `numeric` e categóricos como `nominal`     | preencher           | preencher |
|  6 | Valores faltantes    | Presença de `?` nos atributos planejados                  | preencher           | preencher |
|  7 | Categorias válidas   | Apenas categorias previstas                               | preencher           | preencher |
|  8 | Linhas quebradas     | Ausência de registros mal formatados                      | preencher           | preencher |
|  9 | Duplicatas           | Ausência de duplicatas em excesso                         | preencher           | preencher |

### Observações iniciais

A validação prévia do dataset v2 indicou que o arquivo possui 674 instâncias e exatamente 30 atributos. A classe-alvo `environmental_waste_risk_level` permanece como último atributo e contém apenas as categorias `baixo`, `moderado` e `alto`.

As categorias nominais também foram preservadas conforme o esquema original do dataset, sem indicação de categorias inválidas. Os valores faltantes permanecem representados por `?`, formato compatível com ARFF/Weka.

### Evidência

Inserir aqui o print do Weka mostrando:

* carregamento do arquivo;
* quantidade de instâncias;
* quantidade de atributos;
* classe-alvo.

```markdown
![Carregamento do dataset v2 no Weka](CAMINHO_DA_IMAGEM)
```

---

## 4.2. Etapa 2 — Análise Estatística Descritiva

### Objetivo

Investigar o comportamento geral dos atributos numéricos e categóricos do dataset v2, observando mínimos, máximos, médias, dispersões, frequências e distribuições.

### Atributos numéricos prioritários

```bash
active_power_w
energy_consumption_kwh
water_usage_effectiveness
carbon_intensity_gco2_kwh
inlet_temperature_c
exhaust_temperature_c
delta_t_c
fan_speed_rpm
cpu_utilization_percent
memory_utilization_percent
gpu_power_w
gpu_utilization_percent
gpu_temperature_c
gpu_core_frequency_mhz
num_gpus
batch_size
num_epochs
model_parameter_size_million
training_samples
job_duration_hours
rack_power_density_kw
power_cap_w
```

### Atributos categóricos prioritários

```bash
cooling_method
ai_workload_type
job_status
gpu_sharing_mode
manufacturer_sku_id
rack_label_color
rack_inventory_zone
environmental_waste_risk_level
```

### O que será observado no Weka

| Tipo de análise          | O que verificar                                  |
| ------------------------ | ------------------------------------------------ |
| Mínimo e máximo          | Se os valores estão dentro das faixas planejadas |
| Média                    | Tendência central dos atributos numéricos        |
| Desvio-padrão            | Dispersão dos atributos                          |
| Histograma               | Formato da distribuição                          |
| Frequência de categorias | Distribuição dos atributos nominais              |
| Distribuição da classe   | Quantidade de registros por classe               |

### Observações iniciais da v2

A validação prévia da v2 indicou que os atributos numéricos permanecem dentro das faixas plausíveis definidas no esquema do dataset. Não foram identificados valores negativos, percentuais acima de 100, temperaturas impossíveis ou categorias inválidas.

A distribuição da classe-alvo na v2 foi observada como:

| Classe     | Quantidade | Percentual aproximado |
| ---------- | ---------: | --------------------: |
| `baixo`    |        268 |                 39,8% |
| `moderado` |        248 |                 36,8% |
| `alto`     |        158 |                 23,5% |

A classe `alto` aparece em menor quantidade, o que caracteriza um leve desbalanceamento. Esse ponto não invalida o dataset, mas deverá ser considerado posteriormente na divisão treino/teste e na avaliação dos algoritmos.

### Evidências a inserir

Inserir prints do Weka para atributos como:

* `active_power_w`;
* `energy_consumption_kwh`;
* `fan_speed_rpm`;
* `gpu_temperature_c`;
* `rack_power_density_kw`;
* `environmental_waste_risk_level`.

```markdown
![Distribuição de active_power_w](CAMINHO_DA_IMAGEM)

![Distribuição de fan_speed_rpm](CAMINHO_DA_IMAGEM)

![Distribuição da classe-alvo](CAMINHO_DA_IMAGEM)
```

---

## 4.3. Etapa 3 — Análise de Valores Faltantes

### Objetivo

Verificar se os valores faltantes do dataset v2 foram inseridos conforme o planejamento e levantar hipóteses sobre o tratamento posterior.

### Atributos candidatos a valores faltantes

```bash
gpu_temperature_c
fan_speed_rpm
water_usage_effectiveness
carbon_intensity_gco2_kwh
job_status
```

### Valores faltantes observados na v2

A validação prévia indicou que o dataset v2 contém **154 valores faltantes**, distribuídos apenas nos atributos planejados.

| Atributo                    | Quantidade de faltantes | Observação                                                      |
| --------------------------- | ----------------------: | --------------------------------------------------------------- |
| `carbon_intensity_gco2_kwh` |                      32 | Atributo ambiental planejado para conter faltantes              |
| `gpu_temperature_c`         |                      32 | Atributo térmico planejado para conter faltantes                |
| `water_usage_effectiveness` |                      32 | Atributo ambiental planejado para conter faltantes              |
| `fan_speed_rpm`             |                      32 | Atributo operacional/térmico planejado para conter faltantes    |
| `job_status`                |                      26 | Atributo categórico operacional planejado para conter faltantes |
| **Total**                   |                 **154** | Faltantes controlados e restritos aos atributos previstos       |

Considerando que o dataset possui:

```bash
674 instâncias × 30 atributos = 20220 células
```

os 154 valores faltantes representam aproximadamente:

```bash
154 / 20220 ≈ 0,76% das células do dataset
```

A quantidade é maior que na v1, mas permanece controlada. Os faltantes não aparecem na classe-alvo e não aparecem nos atributos irrelevantes.

### Interpretação

A quantidade de valores faltantes pode ser classificada como **moderada** para o objetivo didático do trabalho. Ela é suficiente para justificar a aplicação de técnicas de tratamento de valores ausentes, sem comprometer a estrutura geral do dataset.

Os faltantes são plausíveis no contexto do problema, pois aparecem em atributos sujeitos a falhas de telemetria, indisponibilidade parcial de sensores ou ausência de informação operacional.

### Hipóteses para o pré-processamento

| Tipo de atributo | Atributos                                                                                      | Técnica candidata                                                               |
| ---------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Numéricos        | `gpu_temperature_c`, `fan_speed_rpm`, `water_usage_effectiveness`, `carbon_intensity_gco2_kwh` | Imputação por mediana ou filtro `ReplaceMissingValues` do Weka                  |
| Categórico       | `job_status`                                                                                   | Imputação pela moda ou criação de categoria `unknown`, se a estratégia permitir |

### Evidência

Inserir prints do Weka mostrando `Missing` nos atributos:

```markdown
![Missing em gpu_temperature_c](CAMINHO_DA_IMAGEM)

![Missing em fan_speed_rpm](CAMINHO_DA_IMAGEM)

![Missing em water_usage_effectiveness](CAMINHO_DA_IMAGEM)

![Missing em carbon_intensity_gco2_kwh](CAMINHO_DA_IMAGEM)

![Missing em job_status](CAMINHO_DA_IMAGEM)
```

---

## 4.4. Etapa 4 — Análise de Ruído

### Objetivo

Verificar se o ruído inserido no dataset v2 é visível, plausível e compatível com o domínio.

### Atributos candidatos a ruído

```bash
active_power_w
energy_consumption_kwh
gpu_power_w
cpu_utilization_percent
memory_utilization_percent
gpu_utilization_percent
inlet_temperature_c
exhaust_temperature_c
gpu_temperature_c
delta_t_c
fan_speed_rpm
gpu_core_frequency_mhz
```

### Observações iniciais sobre o ruído da v2

A validação prévia classificou o ruído da v2 como **moderado**.

Foram identificadas variações mais visíveis na relação entre:

```bash
energy_consumption_kwh
active_power_w
```

e também na relação entre:

```bash
delta_t_c
exhaust_temperature_c
inlet_temperature_c
```

A maioria dos registros preserva coerência geral, mas existem casos localizados de maior inconsistência. Esses casos são importantes para o teste piloto porque representam possíveis erros de medição, falhas de telemetria ou inconsistências de registro.

### Pontos observados previamente

| Relação analisada                                             | Achado                                                                                           | Interpretação                                     |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------- |
| `active_power_w` × `energy_consumption_kwh`                   | Alguns registros apresentam desvio mais visível em relação à aproximação `active_power_w / 1000` | Pode indicar ruído energético ou erro de registro |
| `inlet_temperature_c` × `exhaust_temperature_c` × `delta_t_c` | Alguns registros apresentam diferença térmica menos coerente                                     | Pode indicar ruído térmico ou falha de sensor     |
| Percentuais de utilização                                     | Devem permanecer entre 0 e 100                                                                   | Verificar se não houve valores inválidos          |
| `fan_speed_rpm` e `gpu_core_frequency_mhz`                    | Podem apresentar oscilações operacionais                                                         | Verificar dispersão e possíveis extremos          |

### Interpretação

O ruído da v2 é mais evidente que o da v1. Isso favorece a análise exploratória, pois permite identificar inconsistências e levantar decisões de pré-processamento de forma mais clara.

Entretanto, nem toda inconsistência deve ser removida automaticamente. Alguns registros podem representar erros de medição ou de telemetria que devem ser documentados e tratados com critério.

### Evidência

Inserir prints ou observações do Weka para atributos como:

```markdown
![Distribuição de energy_consumption_kwh](CAMINHO_DA_IMAGEM)

![Distribuição de delta_t_c](CAMINHO_DA_IMAGEM)

![Distribuição de fan_speed_rpm](CAMINHO_DA_IMAGEM)
```

---

## 4.5. Etapa 5 — Análise de Outliers

### Objetivo

Identificar outliers no dataset v2 e avaliar se eles são interpretáveis, planejados e úteis para a tarefa de classificação.

### Tipos de outliers esperados

| Tipo de outlier   | Exemplo                                                         |
| ----------------- | --------------------------------------------------------------- |
| Energético        | Alta potência com baixa utilização de CPU/GPU                   |
| Térmico           | Temperatura elevada mesmo com fan speed alto ou fan speed baixo |
| Operacional       | `job_status = failed` ou `aborted` com longa duração            |
| Ambiental         | Alto consumo com alta intensidade de carbono ou alto WUE        |
| Refrigeração      | Fan speed alto com baixa carga computacional                    |
| Alocação de GPU   | `full_gpu` com baixa utilização de GPU                          |
| Densidade de rack | Alta densidade com refrigeração inadequada                      |

### Observações iniciais da v2

A validação prévia classificou os outliers da v2 como **moderados**.

Foram observados outliers principalmente em:

| Atributo ou relação                          | Observação                                                 |
| -------------------------------------------- | ---------------------------------------------------------- |
| `rack_power_density_kw`                      | Valores altos, inclusive entre 60 e 120 kW                 |
| `water_usage_effectiveness`                  | Alguns valores acima de 3,9                                |
| `gpu_temperature_c`                          | Casos de temperatura elevada                               |
| `fan_speed_rpm`                              | Casos de fan speed elevado ou incompatível com temperatura |
| `job_status` + `job_duration_hours`          | Jobs falhos ou abortados com longa duração                 |
| `active_power_w` + `gpu_utilization_percent` | Alta potência com baixa utilização de GPU                  |

### Interpretação

Os outliers da v2 são, em geral, interpretáveis no contexto de datacenters de IA. Eles podem representar situações como:

* desperdício energético por baixa utilização de GPU;
* falha ou atraso na refrigeração;
* jobs longos com falha ou aborto;
* maior impacto ambiental por alto consumo e alta intensidade de carbono;
* alta densidade energética com método de refrigeração inadequado.

Esses casos devem ser analisados antes de qualquer decisão de remoção. Como o objetivo do dataset é classificar risco de desperdício ambiental, alguns outliers podem carregar informação importante para a classe-alvo.

### Evidência

Inserir prints do Weka ou exemplos de registros suspeitos:

```markdown
![Outliers em rack_power_density_kw](CAMINHO_DA_IMAGEM)

![Outliers em gpu_temperature_c](CAMINHO_DA_IMAGEM)

![Outliers em fan_speed_rpm](CAMINHO_DA_IMAGEM)
```

---

## 4.6. Etapa 6 — Análise da Classe-Alvo

### Objetivo

Verificar como a classe `environmental_waste_risk_level` está distribuída e se há separação excessiva entre as classes.

### Distribuição da classe

A distribuição da classe-alvo no dataset v2 é:

| Classe     | Quantidade | Percentual aproximado |
| ---------- | ---------: | --------------------: |
| `baixo`    |        268 |                 39,8% |
| `moderado` |        248 |                 36,8% |
| `alto`     |        158 |                 23,5% |

### Interpretação

As três classes estão presentes. Há leve desbalanceamento, pois a classe `alto` possui menor quantidade de registros. Esse desbalanceamento não impede o uso do dataset, mas deve ser considerado na avaliação dos algoritmos de classificação.

O risco de separação trivial foi classificado previamente como **moderado**. O atributo `rack_power_density_kw` apresenta forte relação com a classe `alto`, mas há sobreposição entre `moderado` e `alto`, além de registros da classe `alto` com densidade mais baixa.

Isso indica que a classe-alvo não parece ser determinada perfeitamente por um único atributo, embora alguns atributos possam ser fortes preditores.

### Atributos que merecem atenção

```bash
rack_power_density_kw
active_power_w
power_cap_w
gpu_utilization_percent
fan_speed_rpm
job_status
job_duration_hours
```

### Evidência

Inserir prints do Weka da classe-alvo e dos principais atributos relacionados:

```markdown
![Distribuição da classe environmental_waste_risk_level](CAMINHO_DA_IMAGEM)

![Distribuição de rack_power_density_kw por classe](CAMINHO_DA_IMAGEM)

![Distribuição de gpu_utilization_percent por classe](CAMINHO_DA_IMAGEM)
```

---

## 4.7. Etapa 7 — Análise dos Atributos Irrelevantes

### Objetivo

Verificar se os atributos irrelevantes planejados realmente não apresentam relação clara com a classe-alvo.

### Atributos analisados

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

### Observações iniciais

A validação prévia da v2 indicou que os atributos irrelevantes apresentam distribuição equilibrada entre suas categorias e não possuem padrão dominante em relação à classe-alvo.

Esses atributos foram incluídos propositalmente para atender ao requisito de presença de atributos irrelevantes no dataset e devem ser avaliados como candidatos à remoção durante o pré-processamento.

### Interpretação

Os atributos `manufacturer_sku_id`, `rack_label_color` e `rack_inventory_zone` não possuem relação semântica direta com consumo energético, carga computacional, refrigeração ou impacto ambiental.

Portanto, a tendência é removê-los no pré-processamento, caso a análise confirme que não contribuem para a classificação.

### Evidência

Inserir prints ou tabelas de frequência no Weka:

```markdown
![Distribuição de manufacturer_sku_id](CAMINHO_DA_IMAGEM)

![Distribuição de rack_label_color](CAMINHO_DA_IMAGEM)

![Distribuição de rack_inventory_zone](CAMINHO_DA_IMAGEM)
```

---

## 4.8. Etapa 8 — Análise de Relações Semânticas

### Objetivo

Verificar se as principais relações semânticas usadas na geração do dataset v2 aparecem de forma coerente nos dados.

### Relações prioritárias

| Relação                                                       | O que verificar                                                      |
| ------------------------------------------------------------- | -------------------------------------------------------------------- |
| `active_power_w` × `energy_consumption_kwh`                   | Energia deve ser aproximadamente compatível com potência em uma hora |
| `inlet_temperature_c` × `exhaust_temperature_c` × `delta_t_c` | Delta T deve representar a diferença térmica                         |
| `gpu_utilization_percent` × `gpu_power_w`                     | Alta utilização tende a maior potência                               |
| `gpu_utilization_percent` × `environmental_waste_risk_level`  | Baixa utilização com alta potência pode indicar desperdício          |
| `fan_speed_rpm` × temperaturas                                | Fan speed deve acompanhar esforço térmico                            |
| `job_status` × `job_duration_hours`                           | Jobs falhos longos podem indicar desperdício                         |
| `rack_power_density_kw` × classe                              | Verificar risco de dominância                                        |
| `gpu_sharing_mode` × `gpu_utilization_percent`                | GPU inteira com baixa utilização pode indicar desperdício            |

### Observações iniciais

A v2 foi construída para manter coerência geral, mas também incluir imperfeições mais visíveis. Portanto, espera-se que a maior parte dos registros siga as relações semânticas planejadas, enquanto alguns registros apresentem inconsistências ou outliers interpretáveis.

Essas inconsistências não devem ser interpretadas automaticamente como erro de geração. Algumas representam falhas simuladas de telemetria, erro de registro, ruído de medição ou eventos anômalos plausíveis.

### Interpretação

As relações semânticas devem ser analisadas para orientar decisões como:

* manter ou tratar outliers;
* imputar valores faltantes;
* remover atributos irrelevantes;
* normalizar atributos numéricos;
* avaliar atributos redundantes;
* testar modelos com e sem atributos dominantes.

### Evidência

Inserir prints, gráficos ou registros suspeitos:

```markdown
![Relação potência e energia](CAMINHO_DA_IMAGEM)

![Relação fan speed e temperatura](CAMINHO_DA_IMAGEM)

![Relação job status e duração](CAMINHO_DA_IMAGEM)
```

---

# 5. Síntese Parcial do Teste Piloto da v2

Com base na validação inicial da v2, o dataset está apto para ser analisado no Weka e seguir para o pré-processamento, desde que as imperfeições sejam documentadas e tratadas de forma criteriosa.

| Dimensão                   | Avaliação inicial                               |
| -------------------------- | ----------------------------------------------- |
| Integridade estrutural     | Adequada                                        |
| Valores faltantes          | Moderados                                       |
| Ruído                      | Moderado                                        |
| Outliers                   | Moderados                                       |
| Atributos irrelevantes     | Presentes e aparentemente sem padrão por classe |
| Risco de separação trivial | Moderado                                        |
| Compatibilidade com Weka   | Adequada                                        |

As próximas etapas consistem em registrar evidências visuais no Weka, observar distribuições dos principais atributos e consolidar as decisões que orientarão o pré-processamento.

```

## Agora, no Weka, comece por esta ordem

1. Abra `dataset_original_v2.arff`.
2. Tire print da tela geral com `Instances: 674` e `Attributes: 30`.
3. Clique em `environmental_waste_risk_level` e tire print da distribuição da classe.
4. Clique nos atributos com missing:
   - `gpu_temperature_c`
   - `fan_speed_rpm`
   - `water_usage_effectiveness`
   - `carbon_intensity_gco2_kwh`
   - `job_status`
5. Depois clique nos atributos críticos:
   - `active_power_w`
   - `energy_consumption_kwh`
   - `rack_power_density_kw`
   - `gpu_utilization_percent`
   - `gpu_temperature_c`
   - `job_duration_hours`

A metodologia anexada pede justamente que o teste piloto registre integridade, distribuições, valores faltantes, ruído, outliers, classe-alvo, atributos irrelevantes e relações semânticas antes do pré-processamento. :contentReference[oaicite:1]{index=1}
```
