# Regras Semânticas do Dataset

As regras abaixo são usadas para orientar a geração sintética e validar a plausibilidade dos registros. O objetivo é evitar combinações incoerentes entre consumo energético, carga computacional, uso de GPU, refrigeração, temperatura e impacto ambiental.

Estas regras se aplicam à geração normal do dataset. Valores faltantes, ruído e outliers serão tratados em etapas posteriores.

---

## Regras Gerais de Validação

| Código | Regra | Justificativa |
|---|---|---|
| R1 | Todos os atributos numéricos devem respeitar as faixas plausíveis definidas na tabela de atributos. | Evita valores fisicamente ou operacionalmente incoerentes. |
| R2 | Atributos percentuais devem permanecer entre 0 e 100. | CPU, memória e GPU são medidas percentuais. |
| R3 | `energy_consumption_kwh` deve ser aproximadamente compatível com `active_power_w`, considerando uma janela de 1 hora. | Cada instância representa um rack em uma hora de operação. |
| R4 | `delta_t_c` deve ser compatível com a diferença entre `exhaust_temperature_c` e `inlet_temperature_c`. | O Delta T representa a diferença térmica entre saída e entrada do ar. |
| R5 | `environmental_waste_risk_level` deve assumir apenas os valores `baixo`, `moderado` ou `alto`. | Garante consistência da classe-alvo. |
| R6 | `rack_label_color` não deve influenciar a classe-alvo. | Esse atributo é propositalmente irrelevante. |
| R7 | Atributos categóricos devem usar apenas as categorias definidas na tabela de atributos. | Evita categorias inventadas pelo LLM. |
| R8 | `rack_power_density_kw` deve ser compatível com `active_power_w`, `num_gpus` e o perfil do rack de IA. | A densidade de potência representa a escala energética do rack. |
| R9 | `power_cap_w` deve ser coerente com a potência operacional esperada do rack e das GPUs. | O limite de potência representa uma restrição operacional e não deve contradizer o consumo gerado. |

---

## Regras de Relação entre Energia, GPU e Carga Computacional

| Código | Regra | Justificativa |
|---|---|---|
| R10 | Alta utilização de GPU geralmente deve estar associada a maior `gpu_power_w` e maior `active_power_w`. | Workloads de IA usam GPUs intensivamente, elevando potência e consumo. |
| R11 | Baixa utilização de GPU com alta `gpu_power_w` pode indicar subutilização energética. | GPU consumindo muito sem uso proporcional caracteriza possível desperdício. |
| R12 | Alta utilização de CPU e memória pode justificar aumento de `active_power_w`, especialmente em workloads de inferência ou suporte ao treinamento. | O consumo não depende apenas da GPU. |
| R13 | Maior `num_gpus` tende a estar associado a maior `rack_power_density_kw`, maior `active_power_w` e maior `energy_consumption_kwh`. | Mais GPUs aumentam a densidade computacional e energética do rack. |
| R14 | `gpu_core_frequency_mhz` mais alta tende a aumentar desempenho e consumo, especialmente quando combinada com alta utilização de GPU. | Frequência da GPU influencia consumo e desempenho. |
| R15 | `power_cap_w` muito alto combinado com baixa utilização de GPU/CPU pode indicar limite de potência mal ajustado. | Um limite de potência excessivo pode permitir consumo desnecessário. |
| R16 | `power_cap_w` baixo demais para workloads `training` ou `fine_tuning` com muitas GPUs pode indicar limitação operacional. | Treinamento intensivo exige maior orçamento energético. |
| R17 | `gpu_sharing_mode = full_gpu` com baixa `gpu_utilization_percent` pode indicar maior desperdício do que `gpu_sharing_mode = mig` ou `temporal_sharing`. | Alocar uma GPU inteira para carga pequena pode gerar subutilização energética. |
| R18 | `gpu_sharing_mode = mig` ou `temporal_sharing` tende a ser mais plausível em workloads pequenos, leves ou de inferência. | Compartilhamento reduz desperdício em cargas que não exigem a GPU inteira. |
| R19 | Workloads `training` e `fine_tuning` tendem a ter maior duração, maior uso de GPU e maior consumo do que `inference` e `idle`. | Treinamento e ajuste fino são cargas mais intensivas. |
| R20 | Workload `idle` deve ter baixa utilização de GPU, baixa utilização de CPU e menor consumo de energia, exceto em casos de desperdício. | Um rack ocioso com alto consumo é sinal de ineficiência. |

---

## Regras Térmicas e de Refrigeração

| Código | Regra | Justificativa |
|---|---|---|
| R21 | `exhaust_temperature_c` deve ser maior ou igual a `inlet_temperature_c` na maioria dos registros. | O ar de saída tende a ser mais quente após passar pelo rack. |
| R22 | Alta `gpu_power_w` e alta `active_power_w` tendem a elevar `gpu_temperature_c` e `exhaust_temperature_c`. | Maior potência gera maior dissipação térmica. |
| R23 | Alta temperatura da GPU deve estar associada a maior `fan_speed_rpm` ou a métodos de refrigeração mais robustos. | O sistema de refrigeração responde ao aumento térmico. |
| R24 | Maior `rack_power_density_kw` tende a exigir `cooling_method` mais robusto, como `liquid`, `immersion` ou `hybrid`. | Racks de IA de alta densidade são mais difíceis de resfriar com ar tradicional. |
| R25 | `cooling_method = air` deve ser usado com cautela quando `rack_power_density_kw` for muito alto. | Refrigeração a ar pode ser insuficiente para racks de IA de alta densidade. |
| R26 | Alta `rack_power_density_kw` tende a estar associada a maior `fan_speed_rpm`, maior `exhaust_temperature_c` ou necessidade de refrigeração avançada. | A densidade energética aumenta a carga térmica. |
| R27 | `fan_speed_rpm` alta com baixa utilização de CPU/GPU pode indicar desperdício de refrigeração. | Refrigeração intensa sem carga proporcional representa ineficiência. |
| R28 | `delta_t_c` muito baixo com `fan_speed_rpm` alto pode indicar fluxo de ar ineficiente ou ar de bypass. | Alto esforço de ventilação sem troca térmica adequada sugere desperdício. |

---

## Regras Ambientais

| Código | Regra | Justificativa |
|---|---|---|
| R29 | Maior `energy_consumption_kwh` tende a aumentar o impacto ambiental, especialmente quando `carbon_intensity_gco2_kwh` é alta. | A emissão associada depende do consumo e da intensidade de carbono da energia. |
| R30 | Alto `water_usage_effectiveness` combinado com alto consumo energético indica maior impacto hídrico. | WUE relaciona uso de água com energia consumida. |
| R31 | Consumo alto em região com baixa intensidade de carbono pode representar menor risco climático do que o mesmo consumo com alta intensidade de carbono. | A matriz energética altera o impacto ambiental do consumo. |
| R32 | Alto consumo energético só deve ser considerado desperdício alto quando estiver desproporcional à carga útil do rack. | Nem todo alto consumo representa desperdício. |
| R33 | Workloads com alto consumo, longa duração e alta intensidade de carbono devem tender a maior risco ambiental. | O impacto ambiental aumenta quando consumo, tempo e intensidade de carbono se combinam. |

---

## Condições Aproximadas para as Classes

As condições abaixo não são regras rígidas únicas. Elas orientam a geração da classe-alvo e ajudam a manter coerência entre atributos.

A classificação `baixo`, `moderado` ou `alto` deve ser definida por combinações de atributos, e não por um único atributo isolado.

---

#### Classe `baixo`

Um registro tende a ser classificado como `baixo` quando apresenta operação proporcional entre carga computacional, consumo energético, refrigeração e impacto ambiental.

| Condição esperada | Interpretação |
|---|---|
| Alta utilização de GPU com consumo alto, mas compatível | Consumo justificado pela carga de IA. |
| Baixa ou média temperatura com refrigeração proporcional | Controle térmico adequado. |
| `job_status = success` | Job executado com aproveitamento útil. |
| `ai_workload_type = training`, `inference` ou `fine_tuning` com uso coerente de GPU | Carga computacional produtiva. |
| `gpu_sharing_mode = mig` ou `temporal_sharing` em workloads leves | Alocação mais eficiente da GPU. |
| `power_cap_w` compatível com a carga executada | Limite de potência ajustado ao workload. |
| Baixo WUE ou baixa intensidade de carbono | Menor impacto ambiental relativo. |

Exemplo:

```bash
gpu_utilization_percent alto
gpu_power_w alto
active_power_w alto
job_status = success
temperaturas dentro da faixa
power_cap_w compatível com a carga
environmental_waste_risk_level = baixo
```

---

#### Classe `moderado`

Um registro tende a ser classificado como `moderado` quando apresenta sinais parciais de ineficiência, mas sem comportamento extremo.

| Condição esperada                                          | Interpretação                      |
| ---------------------------------------------------------- | ---------------------------------- |
| Consumo moderado ou alto com utilização média              | Alguma ineficiência possível.      |
| Fan speed elevado com temperatura moderada                 | Possível excesso de refrigeração.  |
| GPU ou CPU parcialmente subutilizadas                      | Uso não ideal dos recursos.        |
| Workload `inference` com consumo acima do esperado         | Possível ineficiência operacional. |
| `power_cap_w` acima do necessário, mas sem consumo extremo | Configuração pouco eficiente.      |
| `gpu_sharing_mode = full_gpu` com utilização intermediária | Alocação aceitável, mas não ideal. |
| WUE ou intensidade de carbono intermediários               | Impacto ambiental moderado.        |

Exemplo:

```bash
gpu_utilization_percent médio
active_power_w médio/alto
fan_speed_rpm elevado
temperatura controlada
power_cap_w um pouco acima da carga
environmental_waste_risk_level = moderado
```

---

#### Classe `alto`

Um registro tende a ser classificado como `alto` quando há consumo, refrigeração ou impacto ambiental desproporcional à carga útil do rack.

| Condição esperada                                                               | Interpretação                                         |
| ------------------------------------------------------------------------------- | ----------------------------------------------------- |
| Baixa utilização de GPU/CPU com alta potência                                   | Desperdício energético por subutilização.             |
| `ai_workload_type = idle` com alto consumo                                      | Rack consumindo sem carga útil.                       |
| `job_status = failed` ou `aborted` com longa duração                            | Energia consumida sem resultado útil.                 |
| Alta fan speed com baixa carga computacional                                    | Desperdício de refrigeração.                          |
| Alto consumo com alta intensidade de carbono                                    | Alto impacto climático.                               |
| Alto WUE combinado com alto consumo                                             | Alto impacto hídrico.                                 |
| Temperatura alta mesmo com fan speed alto                                       | Ineficiência térmica ou refrigeração insuficiente.    |
| `gpu_sharing_mode = full_gpu` com baixa utilização de GPU                       | GPU alocada de forma ineficiente.                     |
| `power_cap_w` alto com baixa carga computacional                                | Limite de potência mal ajustado ou permissivo demais. |
| `rack_power_density_kw` alto com `cooling_method = air` e temperaturas elevadas | Risco térmico e energético elevado.                   |

Exemplo:

```bash
gpu_utilization_percent baixo
cpu_utilization_percent baixo
active_power_w alto
fan_speed_rpm alto
job_status = failed
job_duration_hours alto
gpu_sharing_mode = full_gpu
environmental_waste_risk_level = alto
```

---

## Regras para Evitar Registros Incoerentes

| Código | Regra | Justificativa |
| --- | --- | --- |
| R34 | `delta_t_c` não deve contradizer `inlet_temperature_c` e `exhaust_temperature_c` | Mantém consistência térmica |
| R35 | `ai_workload_type = idle` não deve aparecer com `batch_size`, `num_epochs` e `training_samples` extremamente altos, salvo se o registro representar inconsistência planejada em etapa posterior | Evita combinação operacional absurda |
| R36 | `job_status = success` com `job_duration_hours` muito baixo e workload de treinamento muito grande deve ser evitado | Treinamentos grandes não terminam instantaneamente |
| R37 | `num_gpus = 1` não deve ser combinado com consumo típico de racks multi-GPU extremos, exceto em outliers planejados | Garante coerência entre infraestrutura e consumo |
| R38 | `cooling_method = air` deve ser usado com cautela em registros de altíssima potência e muitas GPUs | Racks de IA de alta densidade tendem a exigir refrigeração mais robusta |
| R39 | `rack_label_color` deve variar aleatoriamente e não seguir padrão por classe | Garante que o atributo irrelevante não vire preditor artificial |
| R40 | `gpu_sharing_mode = none` deve estar associado a workload `idle` ou a registros sem uso relevante de GPU | Evita combinação incoerente entre ausência de compartilhamento/uso e workload ativo |
| R41 | `gpu_sharing_mode = mig` deve ser compatível com workloads menores ou compartilhados, principalmente inferência | MIG é mais coerente com particionamento de GPU |
| R42 | `rack_power_density_kw` muito alto deve estar associado a múltiplas GPUs, alto consumo ou método de refrigeração mais robusto | Alta densidade de potência precisa ter causa operacional ou estrutural |
| R43 | `power_cap_w` não deve ser muito inferior à potência necessária para workloads intensivos com muitas GPUs, exceto se representar limitação planejada | Evita gerar configurações inviáveis para treinamento pesado |

---

## Observação Metodológica

As regras semânticas não têm a função de criar uma simulação física perfeita de um datacenter. Elas servem para orientar a geração sintética, reduzir incoerências e garantir que os registros sejam plausíveis para uma tarefa de classificação.

A classe `environmental_waste_risk_level` deve ser definida por combinações de atributos relacionados a:

* consumo energético;
* utilização de GPU, CPU e memória;
* tipo de workload;
* duração e status do job;
* potência e limite de potência;
* densidade energética do rack;
* método de refrigeração;
* temperatura;
* intensidade de carbono;
* uso de água;
* eficiência da alocação da GPU.

Essa estratégia reduz o risco de vazamento de informação e evita que o modelo aprenda uma regra trivial baseada em um único atributo.

