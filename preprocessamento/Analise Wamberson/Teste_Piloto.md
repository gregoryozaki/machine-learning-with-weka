# 5. Etapas do Teste Piloto

## 5.1. Etapa 1 — Verificação de Integridade Estrutural

## Objetivo
Verificar se o conjunto de dados está corretamente estruturado e pode ser utilizado no Weka sem erros técnicos.

## Resultado da verificação

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

## Comprovacão

### 1. Carregamento no Weka
A base foi aberta corretamente no Weka, sem erros de importação.
![Captura de tela do Weka](<https://github.com/user-attachments/assets/05435f68-a568-44b8-bec1-89b01fcd0fc1>
)

### 2. Número de instâncias
O Weka exibiu **674 instâncias**, confirmando a quantidade total de registros.  
![Captura de tela do Weka](<https://github.com/user-attachments/assets/a5b5e35f-135d-4026-bc6d-9d0b908413d4>
)

### 3. Número de atributos
O Weka exibiu **30 atributos**, **sendo 29 atributaos + 1 classe alvo** . Confirmando a estrutura esperada da base.

![Captura de tela do Weka](<https://github.com/user-attachments/assets/a5b5e35f-135d-4026-bc6d-9d0b908413d4>
)

### 4. Classe-alvo
O atributo `environmental_waste_risk_level` foi reconhecido como **nominal**.
![](<https://github.com/user-attachments/assets/31e8fb8a-24d6-4d4f-88a5-f4715bb9f9c8>
).

### 5. Tipos dos atributos
Os atributos numéricos foram reconhecidos como **numeric** e os atributos categóricos como **nominal**.
![](<https://github.com/user-attachments/assets/2c6a5492-dbc8-46a3-b3d3-69fb9a3d845f>
).

### 6. Valores faltantes
Os valores faltantes foram observados em alguns atributos planejados:

- `water_usage_effectiveness` → `Missing: 7 (1%)`.  
![](<https://github.com/user-attachments/assets/dfb3385a-9ab8-40b6-8129-076df3147c81>).  

- `fan_speed_rpm` → `Missing: 7 (1%)`.  
![](<https://github.com/user-attachments/assets/8d3217a3-762a-4771-b285-fbd22d666a8c>).

- `gpu_temperature_c` → `Missing: 7 (1%)`.  
![](<https://github.com/user-attachments/assets/7a1562aa-69b0-46c8-a4af-2cbde9323226>).

- `carbon_intensity_gco2_kwh` → `Missing: 7 (1%)`.  
![](<https://github.com/user-attachments/assets/8a4d80bb-f5ce-4f2d-9480-f81a0e807c51>
).
- `job_status` → `Missing: 6 (1%)`.  
![](<https://github.com/user-attachments/assets/c5c8f158-6d0e-41a8-adfa-e0b5fcd442fa>
).

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
![](<https://github.com/user-attachments/assets/a060cd6c-319a-4cb6-b7ea-5c24dfc527d2>
).

- `manufacturer_sku_id` → `sku_a`, `sku_b`, `sku_c`, `sku_d`, `sku_e` 
![](<https://github.com/user-attachments/assets/0a75da64-4c14-4012-9ded-8ff2788bbd6c>
).

- `gpu_sharing_mode` → `full_gpu`, `temporal_sharing`, `mig`, `none` 
![](<https://github.com/user-attachments/assets/cf824cfd-eac0-47ff-91f2-5660ccb935c7>
).

- `rack_label_color` → `blue`, `green`, `yellow`, `red`, `white` 
![](<https://github.com/user-attachments/assets/ffc051d0-22d5-41a8-a463-2776e671ce5f>
).

- `environmental_waste_risk_level` → `baixo`, `moderado`, `alto`
![](<https://github.com/user-attachments/assets/53d4f181-7e01-41ba-a7a1-dcb9307e7ffd>
).

- `rack_inventory_zone` → `zone_a`, `zone_b`, `zone_c`, `zone_d`
![](<https://github.com/user-attachments/assets/9fc8a910-7b1f-4f5c-bf62-428e597dccb5>
).

### 8. Linhas quebradas
Não foram encontrados indícios de registros mal formatados. A base abriu corretamente, os tipos foram reconhecidos e os atributos numéricos e nominais aparecem coerentes, sem sinais visíveis de desalinhamento.

### 9. Duplicatas
Não foram identificados registros 100% idênticos em excesso.

---

## Perguntas vocacionais

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

## Evidências esperadas
As evidências podem incluir:

- Print da tela de carregamento do Weka.
  ![Captura de tela do Weka](<https://github.com/user-attachments/assets/05435f68-a568-44b8-bec1-89b01fcd0fc1>
)    
  
- Contagem de instâncias e Contagem de atributos.
![Captura de tela do Weka](<https://github.com/user-attachments/assets/a5b5e35f-135d-4026-bc6d-9d0b908413d4>
)  

- Tabela com atributos que possuem valores faltantes.  
Foram identificados valores ausentes em alguns atributos como ja foi identificado acima. 
  
- Observações sobre problemas encontrados.  
Esses atributos ausentes aparecem no Weka como Missing, com pequenas proporções em relação ao total, e devem ser considerados no pré-processamento antes da modelagem. Com base na análise feita no Weka, a base está estruturalmente consistente para uso em mineração de dados e classificação. Os registros foram carregados sem erro, a classe-alvo foi reconhecida como nominal, os atributos numéricos e categóricos foram interpretados corretamente, e os únicos problemas identificados foram valores ausentes pontuais em alguns campos específicos.

## 5.2. Etapa 2 — Análise Estatística Descritiva

### Objetivo

Investigar o comportamento geral dos atributos numéricos e categóricos, observando médias, dispersões, frequências e distribuições.

## Atributos numéricos prioritários
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

## Atributos categóricos prioritários
- `cooling_method`
- `ai_workload_type`
- `job_status`
- `gpu_sharing_mode`
- `manufacturer_sku_id`
- `rack_label_color`
- `rack_inventory_zone`
- `environmental_waste_risk_level`

## O que será analisado

| Tipo de análise | Descrição |
|---|---|
| Mínimo e máximo | Verificar se os valores estão dentro das faixas planejadas. |
| Média | Observar a tendência central dos atributos numéricos. |
| Mediana | Verificar o possível efeito de valores extremos. |
| Desvio-padrão | Avaliar o grau de dispersão dos atributos. |
| Distribuição | Observar o formato dos histogramas no Weka. |
| Frequência de categorias | Verificar a distribuição dos atributos nominais. |
| Distribuição da classe | Verificar a quantidade de registros por classe. |

## Análise dos atributos numéricos
Os atributos numéricos da base apresentam comportamento compatível com um cenário de monitoramento técnico e operacional de data center, com diferentes escalas e amplitudes entre si. No Weka, esses atributos foram reconhecidos como `Numeric` e exibiram estatísticas como mínimo, máximo, média, desvio-padrão, quantidade de valores distintos e valores ausentes, o que permite uma leitura descritiva inicial consistente.

Nos exemplos verificados, `water_usage_effectiveness` apresentou mínimo de 0.28, máximo de 4.96, média de 1.246 e desvio-padrão de 0.635. Isso sugere valores concentrados em uma faixa relativamente controlada, com dispersão moderada e sem indícios imediatos de valores extremos incompatíveis com o contexto da variável.

O atributo `fan_speed_rpm` apresentou mínimo de 1948, máximo de 22000, média de 11612.787 e desvio-padrão de 4099.732. Essa combinação revela grande amplitude e dispersão elevada, indicando forte variabilidade operacional entre os registros e tornando esse atributo um dos mais sensíveis para análise posterior de padronização e outliers.

Já `gpu_temperature_c` apresentou mínimo de 32, máximo de 95, média de 74.798 e desvio-padrão de 13.343, sugerindo dispersão moderada e comportamento plausível. O atributo `carbon_intensity_gco2_kwh` apresentou mínimo de 62, máximo de 891, média de 377.663 e desvio-padrão de 159.486, o que indica maior heterogeneidade e possibilidade de diferentes perfis de consumo e impacto ambiental entre os registros.

De modo geral, os valores mínimos e máximos observados não sugerem inconsistências evidentes nas variáveis verificadas. Ainda assim, atributos como `fan_speed_rpm` e `carbon_intensity_gco2_kwh` merecem atenção por apresentarem maior dispersão, o que pode influenciar diretamente algoritmos sensíveis à escala dos dados.

## Análise dos atributos categóricos
Os atributos categóricos prioritários foram reconhecidos como `Nominal` no Weka e apresentaram apenas categorias coerentes com o domínio esperado da base. Isso indica que os campos foram estruturados corretamente e que não há sinais de categorias inesperadas ou ruído nominal evidente.

No atributo `cooling_method`, foram observadas as categorias `air`, `liquid`, `immersion` e `hybrid`. Em `ai_workload_type`, as categorias identificadas foram `training`, `inference`, `fine_tuning` e `idle`. Já `job_status` apresentou as categorias `success`, `failed`, `aborted` e `running`, mostrando uma distribuição coerente com estados operacionais típicos da execução de tarefas computacionais.

Também foram confirmadas categorias válidas em `gpu_sharing_mode` (`full_gpu`, `temporal_sharing`, `mig`, `none`), `manufacturer_sku_id` (`sku_a`, `sku_b`, `sku_c`, `sku_d`, `sku_e`), `rack_label_color` (`blue`, `green`, `yellow`, `red`, `white`) e `rack_inventory_zone` (`zone_a`, `zone_b`, `zone_c`, `zone_d`). Em todos esses casos, a frequência das categorias pode ser analisada diretamente pelo painel `Selected attribute` do Weka.

A variável-alvo `environmental_waste_risk_level` foi reconhecida como nominal, com as classes `baixo`, `moderado` e `alto`. Essa distribuição é importante para a etapa de classificação, pois permite avaliar se há balanceamento suficiente entre as classes ou se existe predominância de alguma delas na base.

## Valores faltantes e comportamento suspeito
A base apresenta valores ausentes pontuais em alguns atributos, como `water_usage_effectiveness`, `fan_speed_rpm`, `gpu_temperature_c`, `carbon_intensity_gco2_kwh` e `job_status`. Nos exemplos já verificados, a proporção de ausências foi baixa, em torno de 1% da base, o que não compromete a estrutura geral do conjunto de dados, mas exige tratamento adequado no pré-processamento.

Não há evidência forte de que algum atributo seja artificial demais, embora certas variáveis discretas, como `num_gpus`, `batch_size`, `num_epochs` e `power_cap_w`, possam apresentar distribuição mais controlada devido à própria natureza operacional dos dados. Isso deve ser interpretado com cautela: uma distribuição regular pode ser parte natural do processo de coleta e não necessariamente um erro.

## Respostas às perguntas vocacionais

| Pergunta | Resposta | Comentário |
|---|---|---|
| Os valores mínimos e máximos estão dentro das faixas planejadas? | Sim, de forma geral. | As estatísticas observadas no Weka não indicam valores visivelmente incompatíveis nos atributos analisados. |
| Algum atributo possui dispersão muito alta? | Sim. | `fan_speed_rpm` e `carbon_intensity_gco2_kwh` se destacam por maior amplitude e maior desvio-padrão. |
| Algum atributo parece artificial demais? | Não há evidência forte disso. | Algumas variáveis discretas podem parecer controladas, mas isso é compatível com a natureza parametrizada da base. |
| As classes estão distribuídas de forma adequada? | Sim, com possível leve desequilíbrio. | A classe-alvo possui três categorias e deve ser observada quanto ao balanceamento na modelagem. |
| A distribuição dos atributos relevantes parece desejada? | Sim. | Os atributos categóricos têm categorias válidas e os atributos numéricos mostram variação plausível. |
| Existem atributos com distribuição muito separados por classe? | Não foi possível afirmar de forma conclusiva apenas com as estatísticas básicas. | Essa resposta depende de histogramas separados por classe ou comparação visual adicional no Weka. |

## Evidências esperadas
As evidências desta etapa devem incluir:

- Prints dos histogramas do Weka.
- Tabelas com estatísticas básicas dos atributos numéricos.
- Comentários sobre distribuições relevantes.
- Identificação dos atributos com maior dispersão.
- Identificação de atributos com comportamento suspeito.

## Síntese final
A análise exploratória mostra que a base possui atributos numéricos e categóricos estruturalmente consistentes para uso no Weka. Os atributos numéricos apresentam escalas variadas e, em alguns casos, dispersão mais elevada, enquanto os atributos categóricos possuem categorias válidas e coerentes com o domínio. Os valores faltantes existem, mas em baixa proporção, e não há indícios claros de atributos artificiais ou inconsistências estruturais severas.
