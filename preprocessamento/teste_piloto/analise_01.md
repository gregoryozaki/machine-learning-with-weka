# Análise 01 do Teste Piloto

**Responsável:** `Wamberson Pacheco`

---

## Etapa 1 — Verificação de Integridade Estrutural

### Objetivo
Verificar se o conjunto de dados está corretamente estruturado e pode ser utilizado no Weka sem erros técnicos.

### Resultado da verificação

| Nº | Verificação | Descrição | Status | Relato |
|---|---|---|---|---|
| 1 | Carregamento no Weka | Verifique se o arquivo `.arff` abre corretamente. | ✅ | Arquivo `.arff` abriu corretamente durante a execução do teste piloto. |
| 2 | Número de registros | Confirmar a quantidade total de registros. | ✅ | 674 registros encontrados. |
| 3 | Número de atributos | Confirmar se existem 30 atributos. | ✅ | 29 atributos encontrados + o atributo-alvo `environmental_waste_risk_level`, totalizando 30 atributos. |
| 4 | Classe-alvo | Verificar se `environmental_waste_risk_level` foi reconhecida como nominal. | ✅ | Classe-alvo do tipo nominal. |
| 5 | Tipos dos atributos | Verificar se os atributos numéricos e categóricos foram reconhecidos corretamente. | ✅ | Sim, atributos numéricos e nominais foram reconhecidos corretamente. |
| 6 | Valores faltantes | Confirmar a presença de `?` nos atributos planejados. | ✅ | Sim, há valores faltantes em alguns atributos. |
| 7 | Categorias válidas | Verificar se os atributos nominais possuem apenas categorias previstas. | ✅ | Sim, os valores nominais possuem apenas categorias previstas. |
| 8 | Linhas quebradas | Verificar se existem registros mal formatados. | ✅ | Não foram identificados registros mal formatados. Caso houvesse, a base não seria lida normalmente e poderiam ocorrer erros nos atributos. |
| 9 | Duplicatas | Verificar se há registros 100% idênticos em excesso. | ✅ | Não foram identificados dados duplicados em excesso. |

---

### Comprovacão

### 1. Carregamento no Weka

A base foi aberta corretamente no Weka, sem erros de importação.

![Captura de tela do Weka](<https://github.com/user-attachments/assets/05435f68-a568-44b8-bec1-89b01fcd0fc1>)

### 2. Número de instâncias

O Weka exibiu **674 instâncias**, confirmando a quantidade total de registros.

![Captura de tela do Weka](<https://github.com/user-attachments/assets/a5b5e35f-135d-4026-bc6d-9d0b908413d4>)

### 3. Número de atributos

O Weka exibiu **30 atributos**, **sendo 29 atributaos + 1 classe alvo** . Confirmando a estrutura esperada da base.

![Captura de tela do Weka](<https://github.com/user-attachments/assets/a5b5e35f-135d-4026-bc6d-9d0b908413d4>)

### 4. Classe-alvo

O atributo `environmental_waste_risk_level` foi reconhecido como **nominal**.

![](<https://github.com/user-attachments/assets/31e8fb8a-24d6-4d4f-88a5-f4715bb9f9c8>).

### 5. Tipos dos atributos

Os atributos numéricos foram reconhecidos como **numeric** e os atributos categóricos como **nominal**.

![](<https://github.com/user-attachments/assets/2c6a5492-dbc8-46a3-b3d3-69fb9a3d845f>).

### 6. Valores faltantes

Os valores faltantes foram observados em alguns atributos planejados:

- `water_usage_effectiveness` → `Missing: 7 (1%)`.

![](<https://github.com/user-attachments/assets/dfb3385a-9ab8-40b6-8129-076df3147c81>).

- `fan_speed_rpm` → `Missing: 7 (1%)`.

![](<https://github.com/user-attachments/assets/8d3217a3-762a-4771-b285-fbd22d666a8c>).

- `gpu_temperature_c` → `Missing: 7 (1%)`.

![](<https://github.com/user-attachments/assets/7a1562aa-69b0-46c8-a4af-2cbde9323226>).

- `carbon_intensity_gco2_kwh` → `Missing: 7 (1%)`.

![](<https://github.com/user-attachments/assets/8a4d80bb-f5ce-4f2d-9480-f81a0e807c51>).

- `job_status` → `Missing: 6 (1%)`.

![](<https://github.com/user-attachments/assets/c5c8f158-6d0e-41a8-adfa-e0b5fcd442fa>).

Esses valores indicam ausência pontual de dados e devem ser tratados no pré-processamento.

### 7. Categorias válidas

Os atributos nominais apresentaram apenas categorias previstas:

| Atributo | Tipo no Weka | Categorias previstas/encontradas | Interpretação |
|---|---|---|---|
| `cooling_method` | Nominal | `air`, `liquid`, `immersion`, `hybrid` | As categorias exibidas estão de acordo com o esperado, sem valores fora do domínio. |
| `ai_workload_type` | Nominal | `training`, `inference`, `fine_tuning`, `idle` | O Weka reconheceu corretamente as 4 categorias previstas. |
| `job_status` | Nominal | `success`, `failed`, `aborted`, `running` | As categorias mostradas são exatamente as previstas para esse atributo. |
| `manufacturer_sku_id` | Nominal | `sku_a`, `sku_b`, `sku_c`, `sku_d`, `sku_e` | O atributo foi lido corretamente como nominal com 5 categorias válidas. |
| `gpu_sharing_mode` | Nominal | `full_gpu`, `temporal_sharing`, `mig`, `none` | As categorias listadas estão coerentes com o domínio definido. |
| `rack_label_color` | Nominal | `blue`, `green`, `yellow`, `red`, `white` | O Weka exibiu apenas categorias esperadas para esse campo. |
| `environmental_waste_risk_level` | Nominal | `baixo`, `moderado`, `alto` | A classe-alvo foi reconhecida corretamente com 3 categorias nominais. |
| `rack_inventory_zone` | Nominal | `zone_a`, `zone_b`, `zone_c`, `zone_d` | O atributo contém apenas as categorias previstas e nenhuma extra.|

- `cooling_method` →   `air`, `liquid`, `immersion`, `hybrid`

![`cooling_method`](<https://github.com/user-attachments/assets/9c4bb10e-66dc-497f-8d6a-69b43d58dee3>).

- `ai_workload_type` →  `training`, `inference`, `fine_tuning`, `idle`

![](<https://github.com/user-attachments/assets/abeeeb25-07c0-4a86-9830-f13c72be01ef>).

- `job_status` →  `success`, `failed`, `aborted`, `running`

![](<https://github.com/user-attachments/assets/a060cd6c-319a-4cb6-b7ea-5c24dfc527d2>).

- `manufacturer_sku_id` → `sku_a`, `sku_b`, `sku_c`, `sku_d`, `sku_e`

![](<https://github.com/user-attachments/assets/0a75da64-4c14-4012-9ded-8ff2788bbd6c>).

- `gpu_sharing_mode` → `full_gpu`, `temporal_sharing`, `mig`, `none`

![](<https://github.com/user-attachments/assets/cf824cfd-eac0-47ff-91f2-5660ccb935c7>).

- `rack_label_color` → `blue`, `green`, `yellow`, `red`, `white`

![](<https://github.com/user-attachments/assets/ffc051d0-22d5-41a8-a463-2776e671ce5f>).

- `environmental_waste_risk_level` → `baixo`, `moderado`, `alto`

![](<https://github.com/user-attachments/assets/53d4f181-7e01-41ba-a7a1-dcb9307e7ffd>).

- `rack_inventory_zone` → `zone_a`, `zone_b`, `zone_c`, `zone_d`

![](<https://github.com/user-attachments/assets/9fc8a910-7b1f-4f5c-bf62-428e597dccb5>).

### 8. Linhas quebradas

Não foram encontrados indícios de registros mal formatados. A base abriu corretamente, os tipos foram reconhecidos e os atributos numéricos e nominais aparecem coerentes, sem sinais visíveis de desalinhamento.

### 9. Duplicatas

Não foram identificados registros 100% idênticos em excesso.

---

### Perguntas vocacionais

| Nº | Pergunta | Resposta | Evidência nos prints |
|---|---|---|---|
| 1 | O conjunto de dados abriu corretamente no Weka? | Sim. | O print do Weka mostra a base carregada, com `Instances: 674` e `Attributes: 30`, sem erro de abertura. |
| 2 | A classe-alvo foi reconhecida como nominal? | Sim. | No print da classe `environmental_waste_risk_level`, aparece `Type: Nominal` e `Distinct: 3`. |
| 3 | Os atributos numéricos foram reconhecidos como numeric? | Sim. | Os prints de `water_usage_effectiveness`, `fan_speed_rpm` e `gpu_temperature_c` mostram `Type: Numeric`. |
| 4 | Os atributos categóricos foram reconhecidos como nominal? | Sim. | Os prints de `cooling_method`, `ai_workload_type`, `job_status`, `manufacturer_sku_id`, `gpu_sharing_mode`, `rack_label_color`, `environmental_waste_risk_level` e `rack_inventory_zone` mostram `Type: Nominal`. |
| 5 | Existem valores que faltam fora dos atributos planejados? | Não. | Os valores faltantes aparecem apenas em atributos esperados como `water_usage_effectiveness`, `fan_speed_rpm`, `gpu_temperature_c`, `carbon_intensity_gco2_kwh` e `job_status`. |
| 6 | Existem categorias inesperadas? | Não. | Os atributos nominais exibem somente as categorias previstas, como `air/liquid/immersion/hybrid`, `training/inference/fine_tuning/idle` e `baixo/moderado/alto`. |
| 7 | Existem linhas quebradas ou colunas deslocadas? | Não. | A base abriu corretamente, os tipos foram reconhecidos e os atributos numéricos/nominais aparecem coerentes, sem sinais visíveis de desalinhamento. |

---

### Evidências esperadas

As evidências podem incluir:

- Print da tela de carregamento do Weka.

![Captura de tela do Weka](<https://github.com/user-attachments/assets/05435f68-a568-44b8-bec1-89b01fcd0fc1>)

- Contagem de instâncias e Contagem de atributos.

![Captura de tela do Weka](<https://github.com/user-attachments/assets/a5b5e35f-135d-4026-bc6d-9d0b908413d4>)

- Tabela com atributos que possuem valores faltantes.

Foram identificados valores ausentes em alguns atributos como ja foi identificado acima.

- Observações sobre problemas encontrados.

Esses atributos ausentes aparecem no Weka como Missing, com pequenas proporções em relação ao total, e devem ser considerados no pré-processamento antes da modelagem. Com base na análise feita no Weka, a base está estruturalmente consistente para uso em mineração de dados e classificação. Os registros foram carregados sem erro, a classe-alvo foi reconhecida como nominal, os atributos numéricos e categóricos foram interpretados corretamente, e os únicos problemas identificados foram valores ausentes pontuais em alguns campos específicos.

## 5.2. Etapa 2 — Análise Estatística Descritiva

### Objetivo

Investigar o comportamento geral dos atributos numéricos e categóricos, observando médias, dispersões, frequências e distribuições.

### Análise exploratória dos atributos no Weka

### Atributos numéricos prioritários
- `active_power_w`
- `energy_consumption_kwh`
- `water_usage_effectiveness`
- `carbon_intensity_gco2_kwh`
- `inlet_temperature_c`
- `exhaust_temperature_c`
- `delta_t_c`
- `fan_speed_rpm`
- `cpu_utilization_percent`
- `memory_utilization_percent`
- `gpu_power_w`
- `gpu_utilization_percent`
- `gpu_temperature_c`
- `gpu_core_frequency_mhz`
- `num_gpus`
- `batch_size`
- `num_epochs`
- `model_parameter_size_million`
- `training_samples`
- `job_duration_hours`
- `rack_power_density_kw`
- `power_cap_w`

### Atributos categóricos prioritários
- `cooling_method`
- `ai_workload_type`
- `job_status`
- `gpu_sharing_mode`
- `manufacturer_sku_id`
- `rack_label_color`
- `rack_inventory_zone`
- `environmental_waste_risk_level`

### O que será analisado

| Tipo de análise | Descrição |
|---|---|
| Mínimo e máximo | Verificar se os valores estão dentro das faixas planejadas. |
| Média | Observar a tendência central dos atributos numéricos. |
| Mediana | Verificar o efeito dos valores extremos. |
| Desvio-padrão | Avaliar o grau de dispersão dos atributos. |
| Distribuição | Observar o formato dos histogramas no Weka. |
| Frequência de categorias | Verificar a distribuição dos atributos nominais. |
| Distribuição da classe | Verificar a quantidade de registros por classe. |

### Análise dos atributos numéricos

Os atributos numéricos da base apresentam comportamento compatível com um cenário de monitoramento técnico e operacional de data center, com diferentes escalas e amplitudes entre si. No Weka, esses atributos foram reconhecidos como `Numeric` e exibiram estatísticas como mínimo, máximo, média, desvio-padrão, quantidade de valores distintos e valores ausentes, o que permite uma leitura descritiva inicial consistente.

E na base de dados verificada, `water_usage_effectiveness` apresentou mínimo de 0.28, máximo de 4.96, média de 1.246 e desvio-padrão de 0.635. Isso sugere valores concentrados em uma faixa relativamente controlada, com dispersão moderada e sem indícios imediatos de valores extremos incompatíveis com o contexto da variável.

![](<https://github.com/user-attachments/assets/94960243-1462-40bf-99a0-bb983a9c783f>)

O atributo `fan_speed_rpm` apresentou mínimo de 1948, máximo de 22000, média de 11612.787 e desvio-padrão de 4099.732. Essa combinação revela grande amplitude e dispersão elevada, indicando forte variabilidade operacional entre os registros e tornando esse atributo um dos mais sensíveis para análise posterior de padronização e outliers.

![](<https://github.com/user-attachments/assets/54017baa-f9f3-4885-b056-600b828ac2b4>)

Já `gpu_temperature_c` apresentou mínimo de 32, máximo de 95, média de 74.798 e desvio-padrão de 13.343, sugerindo dispersão moderada e comportamento plausível. O atributo `carbon_intensity_gco2_kwh` apresentou mínimo de 62, máximo de 891, média de 377.663 e desvio-padrão de 159.486, o que indica maior heterogeneidade e possibilidade de diferentes perfis de consumo e impacto ambiental entre os registros.

![](<https://github.com/user-attachments/assets/7444888b-119c-4e72-ac00-45f95d0fc38d>)

## 5.3. Etapa 3 — Análise de Valores Faltantes

### Objetivo
Verificar se os valores faltantes foram inseridos conforme o planejamento e levantar hipóteses sobre o tratamento posterior.

### Atributos candidatos a valores faltantes
- `gpu_temperature_c`
- `fan_speed_rpm`
- `water_usage_effectiveness`
- `carbon_intensity_gco2_kwh`
- `job_status`

### Verificação dos faltantes

Os valores faltantes aparecem apenas nos atributos planejados. Nos prints verificados no Weka, os campos `water_usage_effectiveness`, `fan_speed_rpm`, `gpu_temperature_c` e `carbon_intensity_gco2_kwh` apresentam `Missing: 7 (1%)`, enquanto `job_status` apresenta `Missing: 6 (1%)`.

### Proporção de faltantes

A proporção de faltantes é baixa e compatível com uma inserção planejada para simular ausência realista de dados. Em termos práticos, os faltantes representam cerca de 1% da base em cada um desses atributos, o que é aceitável para análise exploratória e modelagem, desde que o tratamento seja feito de forma consistente.

### Distribuição por classe

Pelos prints enviados, não há evidência visual suficiente de concentração forte dos faltantes em uma classe específica. Essa análise pode ser refinada com filtros de visualização por classe no Weka ou com uma tabela cruzada entre faltantes e `environmental_waste_risk_level`.

### Impacto potencial

Os faltantes ocorrem em atributos importantes para o modelo, principalmente `gpu_temperature_c`, `fan_speed_rpm` e `carbon_intensity_gco2_kwh`, que podem influenciar tanto o comportamento térmico quanto o ambiental da base. `job_status` também é relevante porque ajuda a explicar estados operacionais do experimento.

### Técnica de tratamento sugerida

A técnica mais adequada depende do tipo de atributo:
- Para variáveis numéricas com distribuição contínua, a **mediana** é mais robusta que a média quando há assimetria ou valores extremos.
- Para `job_status`, que é nominal, a **moda** ou o filtro `ReplaceMissingValues` do Weka são opções mais apropriadas.
- Se você quiser padronização automática, o filtro `ReplaceMissingValues` do Weka é uma alternativa prática para a etapa inicial.

### Respostas vocacionais

- Os valores faltantes aparecem apenas nos atributos planejados? **Sim.**
- A proporção de valores faltantes é aceitável? **Sim.**
- Há concentração de faltantes em alguma classe? **Não foi evidenciado nos prints.**
- As faltantes ocorrem em atributos importantes para o modelo? **Sim.**
- Qual técnica parece mais adequada? **Mediana para numéricos e moda/ReplaceMissingValues para nominal.**

### Evidências esperadas
| Atributo | Quantidade de faltantes | Percentual | Observação |
|---|---:|---:|---|
| `gpu_temperature_c` | 7 | 1% | Faltantes pontuais, dentro do planejado.  |
| `fan_speed_rpm` | 7 | 1% | Faltantes pontuais, dentro do planejado.  |
| `water_usage_effectiveness` | 7 | 1% | Faltantes pontuais, dentro do planejado.  |
| `carbon_intensity_gco2_kwh` | 7 | 1% | Faltantes pontuais, dentro do planejado. |
| `job_status` | 6 | 1% | Faltantes pontuais, dentro do planejado.  |

---

## Etapa 4 — Análise de Ruído

### Objetivo

Verificar se o ruído introduzido no conjunto de dados é leve, plausível e compatível com o domínio.

### Atributos candidatos a ruído
- `active_power_w`
- `energy_consumption_kwh`
- `gpu_power_w`
- `cpu_utilization_percent`
- `memory_utilization_percent`
- `gpu_utilization_percent`
- `inlet_temperature_c`
- `exhaust_temperature_c`
- `gpu_temperature_c`
- `delta_t_c`
- `fan_speed_rpm`
- `gpu_core_frequency_mhz`

### Pequenas oscilações

Os histogramas e estatísticas observados indicam que há variações plausíveis entre os registros, sem comportamento perfeitamente uniforme. Por exemplo, `cpu_utilization_percent`, `gpu_utilization_percent`, `gpu_power_w`, `inlet_temperature_c` e `exhaust_temperature_c` mostram dispersão coerente com flutuações operacionais normais em data centers de IA.

### Coerência potência-energia

A relação entre `active_power_w` e `energy_consumption_kwh` parece plausível, com ambos os atributos mostrando amplitude e distribuição compatíveis com consumo real. O atributo `active_power_w` apresenta mínimo de 600, máximo de 11980, média de 5737.113 e desvio-padrão de 2881.924, enquanto `energy_consumption_kwh` apresenta mínimo de 0.6, máximo de 12, média de 5.738 e desvio-padrão de 2.881.

### Coerência térmica

Os atributos térmicos também mantêm relação lógica entre si. `inlet_temperature_c` vai de 18.2 a 31, `exhaust_temperature_c` vai de 25.8 a 74 e `delta_t_c` vai de 6.6 a 43, o que sugere uma coerência interna entre entrada, saída e diferença térmica.

### Valores fora de faixa

Não há evidência de valores inválidos nos atributos de ruído analisados. Os intervalos observados permanecem dentro de faixas plausíveis para o domínio.

### Impacto na classe

O ruído parece simular variações reais e não ruído aleatório puro. Isso é desejável, porque preserva relações semânticas importantes entre atributos e classe-alvo.

### Respostas vocacionais

- O ruído permanece dentro das faixas plausíveis? **Sim.**
- Há incoerência entre `energy_consumption_kwh` e `active_power_w`? **Não foi evidenciada.**
- Há incoerência entre `delta_t_c` e temperaturas? **Não foi evidenciada.**
- O ruído parece simular variações reais? **Sim.**
- Deve ser tratado no pré-processamento ou mantido? **Mantido, salvo casos extremos específicos.**

### Evidências esperadas

| Relação | Explicação | Critério | Achado | Ação sugerida |
|---|---|---|---|---|
| `active_power_w × energy_consumption_kwh` | Aproximação em 1 hora | Coerência energética | Valores compatíveis com consumo operacional.  | Manter. |
| `inlet_temperature_c × exhaust_temperature_c × delta_t_c` | Relação térmica | Lógica física | Faixas compatíveis entre entrada, saída e diferença.  | Manter. |
| `cpu_utilization_percent`, `gpu_utilization_percent` e potência | Variação operacional | Plausibilidade | Distribuições coerentes com carga computacional.  | Manter. |

---

## 5.5. Etapa 5 — Análise de Outliers

### Objetivo

Identificar outliers e avaliar se eles são interpretáveis, planejados e úteis para a tarefa de classificação.

### Valores atípicos numéricos

Alguns atributos apresentam amplitude elevada e caudas longas, como `fan_speed_rpm`, `gpu_core_frequency_mhz`, `job_duration_hours`, `batch_size`, `active_power_w` e `energy_consumption_kwh`. Esses casos não significam erro por si só, mas indicam possíveis extremos operacionais que podem ser interpretados como eventos relevantes.

### Combinações peculiares

Há padrões que podem caracterizar outliers semânticos, como alta potência com baixa utilização de GPU, temperaturas elevadas com velocidade alta do ventilador ou `job_status = failed` junto com duração longa. Esses casos são úteis para análise de desperdício, falha ou comportamento anômalo do sistema.

### Outliers por classe

Os outliers podem aparecer em todas as classes, não apenas em `alto`. A distribuição da classe-alvo mostra que há registros em `baixo`, `moderado` e `alto`, então os extremos podem estar associados tanto a situações normais quanto a situações de maior risco ambiental.

### Verossimilhança

Os outliers parecem interpretáveis no contexto de datacenters de IA, especialmente quando refletem eventos críticos plausíveis, como maior carga térmica, maior potência ou execução longa com falha.

### Decisão futura

A recomendação inicial é manter os outliers interpretáveis e tratar apenas casos que claramente pareçam erro de medição ou inconsistência estrutural. Em datasets de monitoramento, extremos podem carregar informação relevante para a tarefa de classificação.

### Respostas vocacionais

- Os outliers são interpretáveis no contexto? **Sim.**
- Parecem erros ou eventos críticos plausíveis? **Mais plausíveis do que errados, na maior parte dos casos.**
- Há outliers em todas as classes ou só em `alto`? **Não foi comprovado que estejam restritos a uma única classe.**
- Algum outlier compromete a coerência? **Não foi evidenciado de forma clara.**
- Devem ser mantidos ou tratados? **Mantidos, com revisão caso a caso.**

### Evidências esperadas

| Atributo ou relação | Valor atípico | Interpretação | Ação sugerida |
|---|---|---|---|
| `gpu_temperature_c` | 95 | Temperatura alta, mas plausível em carga elevada.  | Manter e monitorar. |
| `fan_speed_rpm` | 22000 | Velocidade muito alta do ventilador em cenário de estresse térmico.  | Manter se coerente com contexto. |
| `job_duration_hours + job_status` | Duração longa com falha | Pode representar tarefa problemática ou desperdício operacional.  | Avaliar caso a caso. |
| `rack_power_density_kw` | Valores altos | Pode indicar alta concentração de carga. | Manter e comparar com classe. |

---

## 5.6. Etapa 6 — Análise da Classe-Alvo

### Objetivo

Verificar como a classe `environmental_waste_risk_level` está distribuída e se há separação excessiva entre as classes.

### Distribuição das classes

A classe-alvo está distribuída em três categorias: `baixo` com 268 registros, `moderado` com 248 e `alto` com 158. Isso mostra presença de todas as classes e um leve desequilíbrio, com menor frequência em `alto`.

### Sobreposição entre classes

Os histogramas dos atributos numéricos indicam sobreposição considerável entre as classes em várias variáveis, o que é desejável para um problema realista de classificação. Em atributos como `gpu_temperature_c`, `exhaust_temperature_c`, `inlet_temperature_c`, `gpu_power_w` e `energy_consumption_kwh`, as classes não parecem totalmente separadas.

### Atributos dominantes

Não há indício claro de que uma única variável separe perfeitamente a classe sozinha. A classe `alto` parece resultar de combinação de fatores, e não de um critério trivial isolado.

### Casos de fronteira

Os casos de fronteira parecem plausíveis, especialmente entre `baixo` e `moderado`. Isso é positivo, pois evita uma classificação artificialmente fácil e ajuda a avaliar melhor o desempenho dos algoritmos.

### Coerência semântica

A classe `alto` é coerente com combinações de maior potência, maior temperatura, maior consumo e menor eficiência. Isso sugere que a variável-alvo foi construída com base em uma lógica semântica consistente.

### Respostas vocacionais

- As três classes estão presentes? **Sim.**
- Há equilíbrio suficiente? **Sim, com leve desequilíbrio.**
- Existe atributo que separa quase perfeitamente uma classe? **Não foi evidenciado.**
- A classe `alto` é explicada por um único atributo? **Não parece ser o caso.**
- Existem casos de fronteira plausíveis? **Sim.**
- O conjunto é adequado para classificação? **Sim.**

### Evidências esperadas

| Classe | Quantidade | Características observadas | Possíveis problemas |
|---|---:|---|---|
| `baixo` | 268 | Maior frequência, associado a menor risco. | Pode gerar leve viés se não balanceado. |
| `moderado` | 248 | Próximo de `baixo`, com características intermediárias.  | Sobreposição natural com as demais. |
| `alto` | 158 | Menor frequência, associado a maior risco. | Classe minoritária. |

---

## 5.7. Etapa 7 — Análise dos Atributos Irrelevantes

### Objetivo

Verificar se os atributos irrelevantes planejados realmente não apresentam relação clara com a classe-alvo.

### Atributos analisados
- `manufacturer_sku_id`
- `rack_label_color`
- `rack_inventory_zone`

### Frequência geral

Esses atributos apresentam categorias válidas e frequências relativamente distribuídas, sem concentração evidente em uma única categoria. `manufacturer_sku_id` possui `sku_a` a `sku_e`, `rack_label_color` possui cinco cores e `rack_inventory_zone` possui quatro zonas.

### Relação com a classe

Pelos prints disponíveis, não há indício claro de que SKU, cor ou zona estejam determinando a classe `environmental_waste_risk_level` de forma artificial. Esses atributos podem até apresentar alguma associação residual, mas não há evidência de dependência forte apenas com base nas estatísticas e frequências observadas.

### Decisão futura

A tendência é manter esses atributos em análise exploratória, mas tratá-los como candidatos a remoção se a modelagem mostrar que induzem padrões espúrios. Em outras palavras, podem ser úteis como variáveis de contexto, mas não parecem ser determinantes semânticos diretos da classe.

### Respostas vocacionais

- Os atributos irrelevantes estão distribuídos de forma equilibrada? **Sim, em geral.**
- Algum valor aparece associado demais a uma classe? **Não foi evidenciado.**
- Podem induzir padrão falso? **Possivelmente, mas sem prova forte até aqui.**
- Devem ser removidos sem pré-processamento? **Não necessariamente; melhor avaliar na modelagem.**

### Evidências esperadas

| Atributo | Observado | Relação com a classe? | Ação sugerida |
|---|---|---|---|
| `manufacturer_sku_id` | `sku_a` a `sku_e` com frequências distribuídas.  | Sem evidência forte de relação direta. | Manter temporariamente. |
| `rack_label_color` | Cinco cores válidas e variadas.  | Sem padrão artificial evidente. | Manter temporariamente. |
| `rack_inventory_zone` | Quatro zonas com distribuição equilibrada. | Sem evidência forte de domínio da classe. | Manter temporariamente. |

---

## 5.8. Etapa 8 — Análise de Relações Semânticas

### Objetivo

Verificar se as principais regras semânticas usadas na geração do conjunto de dados aparecem de forma consistente nos dados.

### Relações prioritárias

- `active_power_w × energy_consumption_kwh`
- `inlet_temperature_c × exhaust_temperature_c × delta_t_c`
- `gpu_utilization_percent × gpu_power_w`
- `gpu_utilization_percent × environmental_waste_risk_level`
- `fan_speed_rpm × temperaturas`
- `job_status × job_duration_hours`
- `rack_power_density_kw × classe`
- `gpu_sharing_mode × gpu_utilization_percent`

### Potência × energia

A relação entre `active_power_w` e `energy_consumption_kwh` é compatível com a ideia de consumo aproximado em uma hora. Os valores observados indicam coerência geral entre potência ativa e energia consumida, sem sinais evidentes de inversão ou incompatibilidade.

### Temperaturas × delta T

Os atributos térmicos também mantêm coerência semântica. `inlet_temperature_c`, `exhaust_temperature_c` e `delta_t_c` apresentam faixas plausíveis e relacionamento lógico entre entrada, saída e diferença térmica.

### Utilização da GPU × potência da GPU

A combinação entre `gpu_utilization_percent` e `gpu_power_w` sugere comportamento esperado: maior utilização tende a vir acompanhada de maior consumo de potência, embora a relação não seja perfeita, o que é normal em dados reais.

### Velocidade do ventilador × temperatura

Os atributos `fan_speed_rpm`, `gpu_temperature_c` e as temperaturas do sistema sugerem relação física coerente: maior esforço térmico tende a exigir maior velocidade do ventilador.

### Situação profissional × duração

A relação entre `job_status` e `job_duration_hours` mostra potencial semântico claro. Registros com `failed` ou `aborted` após longas durações podem representar desperdício operacional ou falhas tardias, o que é interpretável no domínio.

### Coerência semântica geral

As relações físicas e operacionais fazem sentido e os dados parecem obedecer às regras esperadas do domínio. Não foram observadas incoerências graves que inviabilizem a análise, embora existam combinações extremas que devem ser interpretadas como possíveis eventos críticos.

### Respostas vocacionais

- As relações físicas e operacionais fazem sentido? **Sim.**
- Há registros incoerentes com as regras semânticas? **Não foi evidenciado de forma clara.**
- Há casos de desperdício justificáveis? **Sim, especialmente em combinações de alta potência, baixa eficiência e longa duração.**
- Há atributos redundantes ou muito correlacionados? **Possivelmente, mas isso exige análise adicional.**
- Existem relações que indicam necessidade de transformação? **Sim, principalmente normalização para atributos numéricos em escalas diferentes.**

### Evidências esperadas

| Relação | Achado | Interpretação | Ação sugerida |
|---|---|---|---|
| Potência × energia | Coerência geral entre os valores. | Relação fisicamente plausível. | Manter. |
| Temperaturas × Delta T | Faixas coerentes.  | Relação térmica lógica. | Manter. |
| Utilização da GPU × Potência da GPU | Tendência compatível com a carga.  | Relação operacional plausível. | Manter. |
| Velocidade do ventilador × temperatura | Resposta coerente ao estresse térmico.  | Relação física esperada. | Manter. |
| Situação profissional × duração | Falhas longas podem indicar desperdício.  | Relação interpretável. | Avaliar caso a caso. |

![imagem](<https://github.com/user-attachments/assets/90a4cb3c-7d5d-4b0b-8ea7-6f7316477356>)

De modo geral, os valores mínimos e máximos observados não sugerem inconsistências evidentes nas variáveis verificadas. Ainda assim, atributos como `fan_speed_rpm` e `carbon_intensity_gco2_kwh` merecem atenção por apresentarem maior dispersão, o que pode influenciar diretamente algoritmos sensíveis à escala dos dados.

![](<https://github.com/user-attachments/assets/9e2e2e94-4753-440d-9cc9-d9cdd5606bcd>)

![](<https://github.com/user-attachments/assets/9d15771f-767f-41fe-a965-15ea9a8aa5b5>)

### Análise dos atributos categóricos
Os atributos categóricos prioritários foram reconhecidos como `Nominal` no Weka e apresentaram apenas categorias coerentes com o domínio esperado da base. Isso indica que os campos foram estruturados corretamente e que não há sinais de categorias inesperadas ou ruído nominal evidente.

No atributo `cooling_method`, foram observadas as categorias `air`, `liquid`, `immersion` e `hybrid`. Em `ai_workload_type`, as categorias identificadas foram `training`, `inference`, `fine_tuning` e `idle`. Já `job_status` apresentou as categorias `success`, `failed`, `aborted` e `running`, mostrando uma distribuição coerente com estados operacionais típicos da execução de tarefas computacionais.

![](<https://github.com/user-attachments/assets/542fe540-1265-48b5-9e6e-117cc18a1263>)

![](<https://github.com/user-attachments/assets/73463b1f-ebf0-4bdc-b626-0b942b6ad75d>)

![](<https://github.com/user-attachments/assets/ffecd974-ca04-4891-8040-db62d5a47ef4>)

Também foram confirmadas categorias válidas em `gpu_sharing_mode` (`full_gpu`, `temporal_sharing`, `mig`, `none`), `manufacturer_sku_id` (`sku_a`, `sku_b`, `sku_c`, `sku_d`, `sku_e`), `rack_label_color` (`blue`, `green`, `yellow`, `red`, `white`) e `rack_inventory_zone` (`zone_a`, `zone_b`, `zone_c`, `zone_d`). Em todos esses casos, a frequência das categorias pode ser analisada diretamente pelo painel `Selected attribute` do Weka.

A variável-alvo `environmental_waste_risk_level` foi reconhecida como nominal, com as classes `baixo`, `moderado` e `alto`. Essa distribuição é importante para a etapa de classificação, pois permite avaliar se há balanceamento suficiente entre as classes ou se existe predominância de alguma delas na base.

![](<https://github.com/user-attachments/assets/db60a650-6c7c-4632-b43b-f1f8e3d5d250>)

### Valores faltantes e comportamento suspeito
A base apresenta valores ausentes pontuais em alguns atributos, como `water_usage_effectiveness`, `fan_speed_rpm`, `gpu_temperature_c`, `carbon_intensity_gco2_kwh` e `job_status`. Nos exemplos já verificados, a proporção de ausências foi baixa, em torno de 1% da base, o que não compromete a estrutura geral do conjunto de dados, mas exige tratamento adequado no pré-processamento.

Não há evidência forte de que algum atributo seja artificial demais, embora certas variáveis discretas, como `num_gpus`, `batch_size`, `num_epochs` e `power_cap_w`, possam apresentar distribuição mais controlada devido à própria natureza operacional dos dados. Isso deve ser interpretado com cautela: uma distribuição regular pode ser parte natural do processo de coleta e não necessariamente um erro.

### Respostas às perguntas vocacionais

| Pergunta | Resposta | Comentário |
|---|---|---|
| Os valores mínimos e máximos estão dentro das faixas planejadas? | Sim, de forma geral. | As estatísticas observadas no Weka não indicam valores visivelmente incompatíveis nos atributos analisados. |
| Algum atributo possui dispersão muito alta? | Sim. | `fan_speed_rpm` e `carbon_intensity_gco2_kwh` se destacam por maior amplitude e maior desvio-padrão.|
| Algum atributo parece artificial demais? | Não há evidência forte disso. | Algumas variáveis discretas podem parecer controladas, mas isso é compatível com a natureza parametrizada da base.|
| As classes estão distribuídas de forma adequada? | Sim, com possível leve desequilíbrio. | A classe-alvo possui três categorias e deve ser observada quanto ao balanceamento na modelagem.  |
| A distribuição dos atributos relevantes parece desejada? | Sim. | Os atributos categóricos têm categorias válidas e os atributos numéricos mostram variação plausível. |
| Existem atributos com distribuição muito separados por classe? | Não foi possível afirmar de forma conclusiva apenas com as estatísticas básicas. | Essa resposta depende de histogramas separados por classe ou comparação visual adicional no Weka.  |

### Evidências esperadas

As evidências desta etapa devem incluir:

- Prints dos histogramas do Weka.
- Tabelas com estatísticas básicas dos atributos numéricos.
- Comentários sobre distribuições relevantes.
- Identificação dos atributos com maior dispersão.
- Identificação de atributos com comportamento suspeito.

### Síntese final
A análise exploratória mostra que a base possui atributos numéricos e categóricos estruturalmente consistentes para uso no Weka. Os atributos numéricos apresentam escalas variadas e, em alguns casos, dispersão mais elevada, enquanto os atributos categóricos possuem categorias válidas e coerentes com o domínio. Os valores faltantes existem, mas em baixa proporção, e não há indícios claros de atributos artificiais ou inconsistências estruturais severas.

## Etapa 3 — Análise de Valores Faltantes

### Objetivo

Verificar se os valores faltantes foram inseridos conforme o planejamento e levantar hipóteses sobre o tratamento posterior.

### Atributos candidatos a valores faltantes
- `gpu_temperature_c`
- `fan_speed_rpm`
- `water_usage_effectiveness`
- `carbon_intensity_gco2_kwh`
- `job_status`

### Verificação dos faltantes
Os valores faltantes aparecem apenas nos atributos planejados. Nos prints verificados no Weka, os campos `water_usage_effectiveness`, `fan_speed_rpm`, `gpu_temperature_c` e `carbon_intensity_gco2_kwh` apresentam `Missing: 7 (1%)`, enquanto `job_status` apresenta `Missing: 6 (1%)`.

### Proporção de faltantes
A proporção de faltantes é baixa e compatível com uma inserção planejada para simular ausência realista de dados. Em termos práticos, os faltantes representam cerca de 1% da base em cada um desses atributos, o que é aceitável para análise exploratória e modelagem, desde que o tratamento seja feito de forma consistente.

### Distribuição por classe
Pelos prints enviados, não há evidência visual suficiente de concentração forte dos faltantes em uma classe específica. Essa análise pode ser refinada com filtros de visualização por classe no Weka ou com uma tabela cruzada entre faltantes e `environmental_waste_risk_level`.

### Impacto potencial
Os faltantes ocorrem em atributos importantes para o modelo, principalmente `gpu_temperature_c`, `fan_speed_rpm` e `carbon_intensity_gco2_kwh`, que podem influenciar tanto o comportamento térmico quanto o ambiental da base. `job_status` também é relevante porque ajuda a explicar estados operacionais do experimento.

### Técnica de tratamento sugerida
A técnica mais adequada depende do tipo de atributo:
- Para variáveis numéricas com distribuição contínua, a **mediana** é mais robusta que a média quando há assimetria ou valores extremos.
- Para `job_status`, que é nominal, a **moda** ou o filtro `ReplaceMissingValues` do Weka são opções mais apropriadas.
- Se você quiser padronização automática, o filtro `ReplaceMissingValues` do Weka é uma alternativa prática para a etapa inicial.

### Respostas vocacionais
- Os valores faltantes aparecem apenas nos atributos planejados? **Sim.**
- A proporção de valores faltantes é aceitável? **Sim.**
- Há concentração de faltantes em alguma classe? **Não foi evidenciado nos prints.**
- As faltantes ocorrem em atributos importantes para o modelo? **Sim.**
- Qual técnica parece mais adequada? **Mediana para numéricos e moda/ReplaceMissingValues para nominal.**

### Evidências esperadas
| Atributo | Quantidade de faltantes | Percentual | Observação |
|---|---:|---:|---|
| `gpu_temperature_c` | 7 | 1% | Faltantes pontuais, dentro do planejado.  |
| `fan_speed_rpm` | 7 | 1% | Faltantes pontuais, dentro do planejado.  |
| `water_usage_effectiveness` | 7 | 1% | Faltantes pontuais, dentro do planejado.  |
| `carbon_intensity_gco2_kwh` | 7 | 1% | Faltantes pontuais, dentro do planejado. |
| `job_status` | 6 | 1% | Faltantes pontuais, dentro do planejado. |

---

## Etapa 4 — Análise de Ruído

### Objetivo
Verificar se o ruído introduzido no conjunto de dados é leve, plausível e compatível com o domínio.

### Atributos candidatos a ruído
- `active_power_w`
- `energy_consumption_kwh`
- `gpu_power_w`
- `cpu_utilization_percent`
- `memory_utilization_percent`
- `gpu_utilization_percent`
- `inlet_temperature_c`
- `exhaust_temperature_c`
- `gpu_temperature_c`
- `delta_t_c`
- `fan_speed_rpm`
- `gpu_core_frequency_mhz`

### Pequenas oscilações
Os histogramas e estatísticas observados indicam que há variações plausíveis entre os registros, sem comportamento perfeitamente uniforme. Por exemplo, `cpu_utilization_percent`, `gpu_utilization_percent`, `gpu_power_w`, `inlet_temperature_c` e `exhaust_temperature_c` mostram dispersão coerente com flutuações operacionais normais em data centers de IA.

### Coerência potência-energia
A relação entre `active_power_w` e `energy_consumption_kwh` parece plausível, com ambos os atributos mostrando amplitude e distribuição compatíveis com consumo real. O atributo `active_power_w` apresenta mínimo de 600, máximo de 11980, média de 5737.113 e desvio-padrão de 2881.924, enquanto `energy_consumption_kwh` apresenta mínimo de 0.6, máximo de 12, média de 5.738 e desvio-padrão de 2.881.

### Coerência térmica
Os atributos térmicos também mantêm relação lógica entre si. `inlet_temperature_c` vai de 18.2 a 31, `exhaust_temperature_c` vai de 25.8 a 74 e `delta_t_c` vai de 6.6 a 43, o que sugere uma coerência interna entre entrada, saída e diferença térmica.

### Valores fora de faixa
Não há evidência de valores inválidos nos atributos de ruído analisados. Os intervalos observados permanecem dentro de faixas plausíveis para o domínio.

### Impacto na classe
O ruído parece simular variações reais e não ruído aleatório puro. Isso é desejável, porque preserva relações semânticas importantes entre atributos e classe-alvo.

### Respostas vocacionais
- O ruído permanece dentro das faixas plausíveis? **Sim.**
- Há incoerência entre `energy_consumption_kwh` e `active_power_w`? **Não foi evidenciada.**
- Há incoerência entre `delta_t_c` e temperaturas? **Não foi evidenciada.**
- O ruído parece simular variações reais? **Sim.**
- Deve ser tratado no pré-processamento ou mantido? **Mantido, salvo casos extremos específicos.**

### Evidências esperadas
| Relação | Explicação | Critério | Achado | Ação sugerida |
|---|---|---|---|---|
| `active_power_w × energy_consumption_kwh` | Aproximação em 1 hora | Coerência energética | Valores compatíveis com consumo operacional.  | Manter. |
| `inlet_temperature_c × exhaust_temperature_c × delta_t_c` | Relação térmica | Lógica física | Faixas compatíveis entre entrada, saída e diferença.  | Manter. |
| `cpu_utilization_percent`, `gpu_utilization_percent` e potência | Variação operacional | Plausibilidade | Distribuições coerentes com carga computacional.  | Manter. |

---

## 5.5. Etapa 5 — Análise de Outliers

### Objetivo
Identificar outliers e avaliar se eles são interpretáveis, planejados e úteis para a tarefa de classificação.

### Valores atípicos numéricos
Alguns atributos apresentam amplitude elevada e caudas longas, como `fan_speed_rpm`, `gpu_core_frequency_mhz`, `job_duration_hours`, `batch_size`, `active_power_w` e `energy_consumption_kwh`. Esses casos não significam erro por si só, mas indicam possíveis extremos operacionais que podem ser interpretados como eventos relevantes.

### Combinações peculiares
Há padrões que podem caracterizar outliers semânticos, como alta potência com baixa utilização de GPU, temperaturas elevadas com velocidade alta do ventilador ou `job_status = failed` junto com duração longa. Esses casos são úteis para análise de desperdício, falha ou comportamento anômalo do sistema.

### Outliers por classe
Os outliers podem aparecer em todas as classes, não apenas em `alto`. A distribuição da classe-alvo mostra que há registros em `baixo`, `moderado` e `alto`, então os extremos podem estar associados tanto a situações normais quanto a situações de maior risco ambiental.

### Verossimilhança
Os outliers parecem interpretáveis no contexto de datacenters de IA, especialmente quando refletem eventos críticos plausíveis, como maior carga térmica, maior potência ou execução longa com falha. [web:218][web:220]

### Decisão futura
A recomendação inicial é manter os outliers interpretáveis e tratar apenas casos que claramente pareçam erro de medição ou inconsistência estrutural. Em datasets de monitoramento, extremos podem carregar informação relevante para a tarefa de classificação.

### Respostas vocacionais
- Os outliers são interpretáveis no contexto? **Sim.**
- Parecem erros ou eventos críticos plausíveis? **Mais plausíveis do que errados, na maior parte dos casos.**
- Há outliers em todas as classes ou só em `alto`? **Não foi comprovado que estejam restritos a uma única classe.**
- Algum outlier compromete a coerência? **Não foi evidenciado de forma clara.**
- Devem ser mantidos ou tratados? **Mantidos, com revisão caso a caso.**

### Evidências esperadas
| Atributo ou relação | Valor atípico | Interpretação | Ação sugerida |
|---|---|---|---|
| `gpu_temperature_c` | 95 | Temperatura alta, mas plausível em carga elevada.  | Manter e monitorar. |
| `fan_speed_rpm` | 22000 | Velocidade muito alta do ventilador em cenário de estresse térmico. | Manter se coerente com contexto. |
| `job_duration_hours + job_status` | Duração longa com falha | Pode representar tarefa problemática ou desperdício operacional.  | Avaliar caso a caso. |
| `rack_power_density_kw` | Valores altos | Pode indicar alta concentração de carga. | Manter e comparar com classe. |

---

## 5.6. Etapa 6 — Análise da Classe-Alvo

### Objetivo
Verificar como a classe `environmental_waste_risk_level` está distribuída e se há separação excessiva entre as classes.

### Distribuição das classes
A classe-alvo está distribuída em três categorias: `baixo` com 268 registros, `moderado` com 248 e `alto` com 158. Isso mostra presença de todas as classes e um leve desequilíbrio, com menor frequência em `alto`.

### Sobreposição entre classes
Os histogramas dos atributos numéricos indicam sobreposição considerável entre as classes em várias variáveis, o que é desejável para um problema realista de classificação. Em atributos como `gpu_temperature_c`, `exhaust_temperature_c`, `inlet_temperature_c`, `gpu_power_w` e `energy_consumption_kwh`, as classes não parecem totalmente separadas.

### Atributos dominantes
Não há indício claro de que uma única variável separe perfeitamente a classe sozinha. A classe `alto` parece resultar de combinação de fatores, e não de um critério trivial isolado.

### Casos de fronteira
Os casos de fronteira parecem plausíveis, especialmente entre `baixo` e `moderado`. Isso é positivo, pois evita uma classificação artificialmente fácil e ajuda a avaliar melhor o desempenho dos algoritmos.

### Coerência semântica
A classe `alto` é coerente com combinações de maior potência, maior temperatura, maior consumo e menor eficiência. Isso sugere que a variável-alvo foi construída com base em uma lógica semântica consistente.

### Respostas vocacionais
- As três classes estão presentes? **Sim.**
- Há equilíbrio suficiente? **Sim, com leve desequilíbrio.**
- Existe atributo que separa quase perfeitamente uma classe? **Não foi evidenciado.**
- A classe `alto` é explicada por um único atributo? **Não parece ser o caso.**
- Existem casos de fronteira plausíveis? **Sim.**
- O conjunto é adequado para classificação? **Sim.**

### Evidências esperadas
| Classe | Quantidade | Características observadas | Possíveis problemas |
|---|---:|---|---|
| `baixo` | 268 | Maior frequência, associado a menor risco. | Pode gerar leve viés se não balanceado. |
| `moderado` | 248 | Próximo de `baixo`, com características intermediárias.  | Sobreposição natural com as demais. |
| `alto` | 158 | Menor frequência, associado a maior risco.  | Classe minoritária. |

---

## 5.7. Etapa 7 — Análise dos Atributos Irrelevantes

### Objetivo
Verificar se os atributos irrelevantes planejados realmente não apresentam relação clara com a classe-alvo.

### Atributos analisados
- `manufacturer_sku_id`
- `rack_label_color`
- `rack_inventory_zone`

### Frequência geral
Esses atributos apresentam categorias válidas e frequências relativamente distribuídas, sem concentração evidente em uma única categoria. `manufacturer_sku_id` possui `sku_a` a `sku_e`, `rack_label_color` possui cinco cores e `rack_inventory_zone` possui quatro zonas.

### Relação com a classe
Pelos prints disponíveis, não há indício claro de que SKU, cor ou zona estejam determinando a classe `environmental_waste_risk_level` de forma artificial. Esses atributos podem até apresentar alguma associação residual, mas não há evidência de dependência forte apenas com base nas estatísticas e frequências observadas.

### Decisão futura
A tendência é manter esses atributos em análise exploratória, mas tratá-los como candidatos a remoção se a modelagem mostrar que induzem padrões espúrios. Em outras palavras, podem ser úteis como variáveis de contexto, mas não parecem ser determinantes semânticos diretos da classe.

### Respostas vocacionais
- Os atributos irrelevantes estão distribuídos de forma equilibrada? **Sim, em geral.**
- Algum valor aparece associado demais a uma classe? **Não foi evidenciado.**
- Podem induzir padrão falso? **Possivelmente, mas sem prova forte até aqui.**
- Devem ser removidos sem pré-processamento? **Não necessariamente; melhor avaliar na modelagem.**

### Evidências esperadas
| Atributo | Observado | Relação com a classe? | Ação sugerida |
|---|---|---|---|
| `manufacturer_sku_id` | `sku_a` a `sku_e` com frequências distribuídas. [file:206] | Sem evidência forte de relação direta. | Manter temporariamente. |
| `rack_label_color` | Cinco cores válidas e variadas. [file:114] | Sem padrão artificial evidente. | Manter temporariamente. |
| `rack_inventory_zone` | Quatro zonas com distribuição equilibrada. [file:116] | Sem evidência forte de domínio da classe. | Manter temporariamente. |

---

## 5.8. Etapa 8 — Análise de Relações Semânticas

### Objetivo
Verificar se as principais regras semânticas usadas na geração do conjunto de dados aparecem de forma consistente nos dados.

### Relações prioritárias
- `active_power_w × energy_consumption_kwh`
- `inlet_temperature_c × exhaust_temperature_c × delta_t_c`
- `gpu_utilization_percent × gpu_power_w`
- `gpu_utilization_percent × environmental_waste_risk_level`
- `fan_speed_rpm × temperaturas`
- `job_status × job_duration_hours`
- `rack_power_density_kw × classe`
- `gpu_sharing_mode × gpu_utilization_percent`

### Potência × energia
A relação entre `active_power_w` e `energy_consumption_kwh` é compatível com a ideia de consumo aproximado em uma hora. Os valores observados indicam coerência geral entre potência ativa e energia consumida, sem sinais evidentes de inversão ou incompatibilidade.

### Temperaturas × delta T
Os atributos térmicos também mantêm coerência semântica. `inlet_temperature_c`, `exhaust_temperature_c` e `delta_t_c` apresentam faixas plausíveis e relacionamento lógico entre entrada, saída e diferença térmica.

### Utilização da GPU × potência da GPU
A combinação entre `gpu_utilization_percent` e `gpu_power_w` sugere comportamento esperado: maior utilização tende a vir acompanhada de maior consumo de potência, embora a relação não seja perfeita, o que é normal em dados reais.

### Velocidade do ventilador × temperatura
Os atributos `fan_speed_rpm`, `gpu_temperature_c` e as temperaturas do sistema sugerem relação física coerente: maior esforço térmico tende a exigir maior velocidade do ventilador.

### Situação profissional × duração
A relação entre `job_status` e `job_duration_hours` mostra potencial semântico claro. Registros com `failed` ou `aborted` após longas durações podem representar desperdício operacional ou falhas tardias, o que é interpretável no domínio.

### Coerência semântica geral
As relações físicas e operacionais fazem sentido e os dados parecem obedecer às regras esperadas do domínio. Não foram observadas incoerências graves que inviabilizem a análise, embora existam combinações extremas que devem ser interpretadas como possíveis eventos críticos.

### Respostas vocacionais
- As relações físicas e operacionais fazem sentido? **Sim.**
- Há registros incoerentes com as regras semânticas? **Não foi evidenciado de forma clara.**
- Há casos de desperdício justificáveis? **Sim, especialmente em combinações de alta potência, baixa eficiência e longa duração.**
- Há atributos redundantes ou muito correlacionados? **Possivelmente, mas isso exige análise adicional.**
- Existem relações que indicam necessidade de transformação? **Sim, principalmente normalização para atributos numéricos em escalas diferentes.**

### Evidências esperadas
| Relação | Achado | Interpretação | Ação sugerida |
|---|---|---|---|
| Potência × energia | Coerência geral entre os valores. | Relação fisicamente plausível. | Manter. |
| Temperaturas × Delta T | Faixas coerentes.  | Relação térmica lógica. | Manter. |
| Utilização da GPU × Potência da GPU | Tendência compatível com a carga.  | Relação operacional plausível. | Manter. |
| Velocidade do ventilador × temperatura | Resposta coerente ao estresse térmico.  | Relação física esperada. | Manter. |
| Situação profissional × duração | Falhas longas podem indicar desperdício. | Relação interpretável. | Avaliar caso a caso. |

