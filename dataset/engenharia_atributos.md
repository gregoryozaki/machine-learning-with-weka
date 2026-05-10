# Engenharia de Atributos

## 1. Busca de Atributos

Os atributos foram selecionados a partir dos resultados do MSL — Eixo 2: Atributos e Métricas, que mapeia métricas, variáveis e indicadores utilizados na literatura para representar consumo energético, refrigeração, temperatura, utilização computacional, emissão de carbono, uso de água, eficiência operacional e desperdício ambiental em datacenters, servidores e racks.

Após a definição do recorte do trabalho para **datacenters voltados a cargas de IA**, foi realizada uma busca complementar para identificar atributos específicos relacionados a GPUs, aceleradores, treinamento de modelos, inferência, consumo energético, dissipação térmica, refrigeração e impacto ambiental.

A seleção dos atributos considerou:

* relevância para o problema de desperdício ambiental;
* possibilidade de interpretação em nível de rack;
* relação com consumo energético;
* relação com refrigeração e temperatura;
* relação com utilização computacional;
* relação com impacto ambiental;
* compatibilidade com a tarefa de classificação;
* possibilidade de representação em formato tabular;
* aderência ao contexto de datacenters de IA, treinamento, inferência e uso intensivo de GPUs.

---

## 2. Atributos sustentados pela literatura

| ID | Atributo | Tipo | Grupo | Descrição | Unidade | Nível |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Energy Waste Ratio — EWR | Numérico | Energético | Razão entre energia desperdiçada e energia total consumida | % | Rack |
| 2 | Potência Real / Active Power | Numérico | Energético | Consumo elétrico instantâneo do servidor ou rack | W / kW | Servidor / Rack |
| 3 | Carbon Waste Ratio — CWR | Numérico | Ambiental | Razão de emissões de CO₂ associadas ao desperdício energético | % | Rack |
| 4 | Water Usage Effectiveness — WUE | Numérico | Ambiental | Eficiência do uso de água associado à operação ou refrigeração | L/kWh | Rack / Datacenter |
| 5 | Consumo Estimado de Água | Numérico | Ambiental / Derivado | Estimativa do volume de água associado ao consumo energético e à eficiência hídrica da refrigeração. | Litros (L) | Rack / Datacenter |
| 6 | Temperatura de Entrada do Ar / Inlet Temperature | Numérico | Térmico | Temperatura do ar na entrada/frente do rack, usada em conjunto com a temperatura de exaustão para analisar o ΔT. | °C | Rack / Servidor |
| 7 | Temperatura de Exaustão / Exhaust Temperature | Numérico | Térmico | Temperatura do ar na saída traseira do rack ou servidor | °C | Servidor / Rack |
| 8 | Delta T — ΔT | Numérico | Térmico | Diferença entre temperatura de saída e temperatura de entrada | °C | Rack |
| 9 | Utilização de CPU | Numérico | Computacional | Percentual de uso dos núcleos de processamento | % | Servidor |
| 10 | Fan Speed | Numérico | Operacional / Térmico | Rotação dos ventiladores internos | RPM | Componente / Servidor |
| 11 | Job Status / Exit Code | Categórico | Operacional | Resultado da execução de um job, como sucesso, erro ou falha | N/A | Servidor / Workload |
| 12 | P-states / C-states | Categórico | Operacional | Estados de voltagem, frequência e economia de energia da CPU | N/A | Componente |
| 13 | Fator de Potência | Numérico | Energético | Razão entre potência real e potência aparente | Ratio | Rack |
| 14 | ID de Fabricante / SKU | Categórico | Operacional | Metadados estáticos do hardware | N/A | Servidor |
| 15 | PUE Global — Power Usage Effectiveness | Numérico | Energético | Eficiência energética total da instalação | Ratio | Datacenter |
| 16 | Timestamp | Numérico | Operacional | Registro temporal da coleta dos dados | ms / s | Todos |
| 17 | Throughput Bruto — FLOPS | Numérico | Computacional | Capacidade teórica de processamento | Ops/s | Componente |
| 18 | CapEx / OpEx Financeiro | Numérico | Operacional | Custos monetários de aquisição ou operação | Moeda | Rack / Datacenter |
| 19 | Intensidade de Carbono — α | Numérico | Ambiental | Pegada de carbono associada a cada kWh consumido | gCO₂/kWh | Rack / Datacenter / Região |
| 20 | Consumo de Energia | Numérico | Energético | Energia acumulada consumida ao longo do tempo de operação | kWh | Rack / Servidor / Workload |
| 21 | Potência da GPU | Numérico | IA / Energético | Potência consumida pelas GPUs durante cargas de IA | W | GPU / Servidor |
| 22 | Utilização de GPU | Numérico | IA / Computacional | Percentual de uso da GPU durante treinamento, inferência ou fine-tuning | % | GPU |
| 23 | Temperatura da GPU | Numérico | IA / Térmico | Temperatura operacional da GPU | °C | GPU |
| 24 | Frequência / Clock da GPU | Numérico | IA / Operacional | Frequência de operação da GPU, associada a DVFS, desempenho e consumo | MHz | GPU |
| 25 | Número de GPUs | Numérico | Infraestrutura IA | Quantidade de GPUs alocadas ou presentes no servidor/rack | Número absoluto | Servidor / Rack / Workload |
| 26 | Utilização de Memória | Numérico | Computacional | Percentual de uso de memória durante a execução do workload | % | Servidor / GPU |
| 27 | Tipo de Workload de IA | Categórico | Workload IA | Tipo de carga executada, como treinamento, inferência, fine-tuning ou ocioso  | N/A | Workload |
| 28 | Batch Size | Numérico | Workload IA | Número de amostras processadas por iteração de treinamento | Número absoluto | Workload                   |
| 29 | Número de Épocas | Numérico | Workload IA | Número de passagens completas pelo conjunto de treinamento | Número absoluto | Workload |
| 30 | Tamanho do Modelo | Numérico | Workload IA | Quantidade aproximada de parâmetros do modelo | Milhões de parâmetros | Workload |
| 31 | Quantidade de Amostras de Treinamento | Numérico | Workload IA | Número de amostras usadas no treinamento | Número absoluto | Workload |
| 32 | Duração do Job | Numérico | Operacional | Tempo de execução do job de treinamento, inferência ou fine-tuning | Horas / segundos | Workload |
| 33 | Throughput de Treinamento | Numérico | Desempenho / IA | Taxa de processamento do job, como iterações por segundo | iter/s | Workload |
| 34 | Energia por Iteração | Numérico | Energético / Derivado | Energia consumida por iteração de treinamento | J | Workload |
| 35 | Método de Refrigeração | Categórico | Térmico | Tipo de refrigeração utilizado, como ar, líquida, imersão ou híbrida | N/A | Rack / Datacenter |
| 36 | Emissão Estimada de Carbono | Numérico | Ambiental / Derivado | Emissão estimada a partir do consumo energético e da intensidade de carbono | kgCO₂ | Rack / Workload |
| 37 | Power Cap / Limite de Potência | Numérico | Operacional / Energético | Limite de potência aplicado a CPU, GPU ou servidor | W | GPU / Servidor |
| 38 | Power Budget / Orçamento de Potência | Numérico | Operacional / Energético | Orçamento de potência disponível para execução de jobs ou operação do cluster | W | Rack / Cluster |
| 39 | Taxa de Requisições de Inferência | Numérico | Workload IA | Volume de requisições processadas em workloads de inferência | req/s | Workload |
| 40 | Quantidade de Tokens Processados | Numérico | Workload IA | Número de tokens processados em tarefas de inferência de LLMs | tokens | Workload |
| 41 | Cor da Etiqueta do Rack | Categórico | Controle | Atributo artificial sem relação semântica com desperdício ambiental | N/A | Rack |
| 42 | Nível de Risco de Desperdício Ambiental | Categórico | Classe | Classe-alvo do dataset | ```baixo / moderado / alto``` | Rack |

---

## 3. Justificativa dos Atributos

| ID | Atributo | Relevância | Justificativa | Autores que sustentam | Contribuição para o Dataset | Incluir |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Energy Waste Ratio — EWR | Relevante, mas com risco de vazamento | Mede diretamente a proporção de energia desperdiçada. Pode ser útil para rotulagem, mas pode tornar a classe óbvia se usado como feature final | Grishina | Métrica auxiliar interna / possível base de rotulagem | Apenas internamente |
| 2 | Potência Real / Active Power | Relevante | Identifica consumo instantâneo e pode revelar consumo elevado em cenários de baixa utilização | Bartolini; Meisner; Sunkara; Narukulla | Feature preditora | Sim |
| 3 | Carbon Waste Ratio — CWR | Relevante, mas com risco de vazamento | Relaciona emissões de carbono ao desperdício energético. Pode ser útil para rotulagem, mas tem risco de vazamento se usado como entrada do classificador | Grishina; Sommerhalter | Métrica auxiliar interna / possível base de rotulagem | Apenas internamente |
| 4 | Water Usage Effectiveness — WUE | Relevante | Representa impacto hídrico associado à refrigeração e operação do datacenter | Lei; Li et al.; Cruzes | Feature preditora | Sim |
| 5 | Consumo Estimado de Água | Derivado / Opcional | Pode ser estimado a partir do consumo energético e do WUE, permitindo representar impacto hídrico associado à operação e refrigeração. Como é derivado, deve ser usado com cautela para não tornar a classe-alvo óbvia. | Lei; Li et al.; Ferraz; Cruzes | Métrica derivada / Feature com cautela | Opcional |
| 6 | Temperatura de Entrada do Ar / Inlet Temperature | Relevante por adaptação | É necessária para interpretar o Delta T, pois o ΔT depende da diferença entre a temperatura de saída e a temperatura de entrada. Ajuda a avaliar eficiência térmica e fluxo de ar no rack. | Grishina; Hamann; Sunkara; Narukulla; Cruzes | Feature preditora | Sim |
| 7 | Temperatura de Exaustão / Exhaust Temperature | Relevante | Temperaturas elevadas podem indicar hotspots, ineficiência térmica ou refrigeração inadequada | Hamann; Grishina; Sunkara; Narukulla | Feature preditora | Sim |
| 8 | Delta T — ΔT | Relevante | Ajuda a avaliar eficiência térmica e diferença entre entrada e saída de ar | Grishina; Hamann | Feature preditora | Sim |
| 9 | Utilização de CPU | Relevante | Baixa utilização de CPU associada a alta potência pode indicar ociosidade ou desperdício | Shedd; Sommerhalter; Sunkara; Narukulla; Cho et al. | Feature preditora | Sim |
| 10 | Fan Speed | Relevante | Rotações elevadas aumentam consumo e indicam esforço de refrigeração | Grishina; Yao et al.; Sunkara; Narukulla; Chung et al. | Feature preditora | Sim |
| 11 | Job Status / Exit Code | Relevante | Jobs com erro ou falha podem desperdiçar energia e carbono acumulados durante a execução | Grishina; Bartolini | Feature preditora / base para regra semântica | Sim |
| 12 | P-states / C-states | Relevante, mas substituível | Representa estados de economia de energia da CPU. No recorte de IA, pode ser substituído por frequência/clock da GPU | Guitart; Meisner | Feature preditora ou atributo a substituir | Opcional |
| 13 | Fator de Potência | Relevante | Baixo fator de potência indica uso menos eficiente da energia elétrica e perdas na distribuição | Shedd; Bartolini | Feature preditora | Sim |
| 14 | ID de Fabricante / SKU | Irrelevante | Informação estática do hardware. Pode ter relação indireta com eficiência, por isso não é o melhor atributo irrelevante | Grishina; Curtis | Atributo a descartar / substituir | Não |
| 15 | PUE Global — Power Usage Effectiveness | Irrelevante para nível de rack | Métrica de granularidade de datacenter, podendo ocultar ineficiências específicas de rack | Grishina; Sommerhalter; Ferraz; Cruzes | Atributo a descartar | Não |
| 16 | Timestamp | Irrelevante como preditor | Serve para indexação temporal, mas não representa diretamente desperdício ambiental | Bartolini; Sommerhalter | Feature de indexação | Não |
| 17 | Throughput Bruto — FLOPS | Irrelevante ou substituível | Mede capacidade teórica de processamento. Sem vínculo com energia, pode não representar desperdício ambiental | Fanara; Sommerhalter | Atributo a descartar / substituir | Não |
| 18 | CapEx / OpEx Financeiro | Irrelevante | Custos financeiros não refletem diretamente impacto ambiental operacional imediato | Sommerhalter; Li | Atributo a descartar | Não |
| 19 | Intensidade de Carbono — α | Relevante | Permite estimar emissões associadas ao consumo energético | Grishina; Li et al.; Ferraz | Feature preditora | Sim |
| 20 | Consumo de Energia | Relevante | Energia acumulada se relaciona com custo, emissão de carbono e demanda de refrigeração | Sunkara; Narukulla; Ferraz | Feature preditora | Sim |
| 21 | Potência da GPU | Relevante | GPUs são centrais em datacenters de IA e podem dominar o consumo do servidor | Sunkara; Narukulla; Liao et al.; Gu et al. | Feature preditora | Sim |
| 22 | Utilização de GPU | Relevante | Baixa utilização de GPU com alta potência indica subutilização e potencial desperdício | Gu et al.; Lai et al.; Jacquet et al. | Feature preditora | Sim |
| 23 | Temperatura da GPU | Relevante | Representa condição térmica do acelerador e necessidade de refrigeração | Sunkara; Narukulla; Chung et al.; Jacquet et al. | Feature preditora | Sim |
| 24 | Frequência / Clock da GPU | Relevante | Afeta consumo, desempenho e eficiência energética; associada a DVFS e power management | Gu et al.; Cho et al.; Chung et al. | Feature preditora | Sim |
| 25 | Número de GPUs | Relevante | Quantidade de GPUs influencia consumo, paralelismo, densidade computacional e energia do job | Liao et al.; Gu et al.; Cruzes | Feature preditora | Sim |
| 26 | Utilização de Memória | Relevante | Workloads de LLM possuem demandas multidimensionais de memória, computação e potência | Lai et al.; Liao et al. | Feature preditora | Sim |
| 27 | Tipo de Workload de IA | Relevante | Treinamento, inferência, fine-tuning e ociosidade possuem perfis energéticos e operacionais diferentes | Kang et al.; Cho et al.; Cruzes | Feature preditora | Sim |
| 28 | Batch Size | Relevante | Afeta throughput, consumo e comportamento de treinamento | Liao et al.; Gu et al. | Feature preditora | Sim |
| 29 | Número de Épocas | Relevante | Influencia duração do treinamento e consumo acumulado | Liao et al.; Kang et al. | Feature preditora | Sim |
| 30 | Tamanho do Modelo | Relevante | Modelos maiores tendem a exigir mais computação, memória e energia | Liao et al.; Gu et al.; Cruzes | Feature preditora | Sim |
| 31 | Quantidade de Amostras de Treinamento | Relevante | Volume de dados de treinamento influencia duração, iterações e consumo | Liao et al.; Gu et al. | Feature preditora | Sim |
| 32 | Duração do Job | Relevante | Jobs mais longos acumulam maior consumo energético e potencial impacto ambiental | Kang et al.; Gu et al.; Liao et al. | Feature preditora | Sim |
| 33 | Throughput de Treinamento | Relevante | Relaciona desempenho com consumo e pode apoiar análise de eficiência energética | Gu et al. | Feature preditora ou métrica auxiliar | Opcional |
| 34 | Energia por Iteração | Derivado / risco de vazamento | Resume diretamente energia por unidade de processamento. Pode aproximar demais a regra de rotulagem | Gu et al. | Métrica auxiliar interna | Apenas internamente |
| 35 | Método de Refrigeração | Relevante | Métodos como ar, refrigeração líquida e imersão alteram eficiência térmica, energia e uso de água | Sunkara; Narukulla; Cruzes | Feature preditora | Sim |
| 36 | Emissão Estimada de Carbono | Derivado | Estimada a partir de energia e intensidade de carbono. Útil para análise, mas pode causar vazamento se a classe depender diretamente dela | Ferraz; Cruzes | Métrica auxiliar ou feature com cautela | Opcional |
| 37 | Power Cap / Limite de Potência | Relevante | Limites de potência influenciam desempenho, subutilização e desperdício planejado | Cho et al.; Chung et al. | Feature preditora | Opcional |
| 38 | Power Budget / Orçamento de Potência | Relevante | Orçamento energético influencia alocação de jobs e decisões de scheduling | Gu et al. | Feature preditora | Opcional |
| 39 | Taxa de Requisições de Inferência | Relevante para inferência | Volume de requisições altera consumo, carga e demanda de recursos | Lai et al.; Cho et al. | Feature preditora | Opcional |
| 40 | Quantidade de Tokens Processados | Relevante para inferência | Tokens processados influenciam demanda computacional, memória e potência em LLMs | Lai et al.; Cruzes | Feature preditora | Opcional |
| 41 | Cor da Etiqueta do Rack | Irrelevante | Atributo artificial sem relação semântica com desperdício ambiental, incluído para atender ao requisito de atributo irrelevante | Autores do projeto | Atributo irrelevante | Sim |
| 42 | Nível de Risco de Desperdício Ambiental | Classe-alvo | Representa a variável a ser prevista pelo modelo de classificação | Autores do projeto | Classe-alvo | Sim |

---


## Versão Final

| ID | Atributo | Nome p/ Dataset - En | Nome p/ Dataset - Pt | Unidade | Tipo | Grupo | Faixa plausível / Categorias válidas |
|---:|---|---|---|---|---|---|---|
| 1 | Potência Real / Active Power | `active_power_w` | `potencia_ativa_w` | W | Numérico | Energético | 500 a 12000 |
| 2 | Consumo de Energia | `energy_consumption_kwh` | `consumo_energia_kwh` | kWh | Numérico | Energético | 0.5 a 12.0 por hora |
| 3 | Water Usage Effectiveness — WUE | `water_usage_effectiveness` | `efetividade_uso_agua` | L/kWh | Numérico | Ambiental | 0.1 a 5.0 |
| 4 | Intensidade de Carbono | `carbon_intensity_gco2_kwh` | `intensidade_carbono_gco2_kwh` | gCO₂/kWh | Numérico | Ambiental | 20 a 900 |
| 5 | Temperatura de Entrada do Ar / Inlet Temperature | `inlet_temperature_c` | `temperatura_entrada_c` | °C | Numérico | Térmico | 15 a 35 |
| 6 | Temperatura de Exaustão / Exhaust Temperature | `exhaust_temperature_c` | `temperatura_exaustao_c` | °C | Numérico | Térmico | 25 a 75 |
| 7 | Delta T — ΔT | `delta_t_c` | `delta_t_c` | °C | Numérico | Térmico | 2 a 45 |
| 8 | Fan Speed | `fan_speed_rpm` | `velocidade_fans_rpm` | RPM | Numérico | Operacional / Térmico | 1000 a 22000 |
| 9 | Método de Refrigeração | `cooling_method` | `metodo_refrigeracao` | N/A | Categórico | Térmico | `air`, `liquid`, `immersion`, `hybrid` |
| 10 | Utilização de CPU | `cpu_utilization_percent` | `utilizacao_cpu_percentual` | % | Numérico | Computacional | 0 a 100 |
| 11 | Utilização de Memória | `memory_utilization_percent` | `utilizacao_memoria_percentual` | % | Numérico | Computacional | 0 a 100 |
| 12 | Potência da GPU | `gpu_power_w` | `potencia_gpu_w` | W | Numérico | IA / Energético | 50 a 700 |
| 13 | Utilização de GPU | `gpu_utilization_percent` | `utilizacao_gpu_percentual` | % | Numérico | IA / Computacional | 0 a 100 |
| 14 | Temperatura da GPU | `gpu_temperature_c` | `temperatura_gpu_c` | °C | Numérico | IA / Térmico | 30 a 95 |
| 15 | Frequência / Clock da GPU | `gpu_core_frequency_mhz` | `frequencia_gpu_mhz` | MHz | Numérico | IA / Operacional | 300 a 2500 |
| 16 | Número de GPUs | `num_gpus` | `numero_gpus` | Número absoluto | Numérico | Infraestrutura IA | 1, 2, 4, 8 ou 16 |
| 17 | Tipo de Workload de IA | `ai_workload_type` | `tipo_workload_ia` | N/A | Categórico | Workload IA | `training`, `inference`, `fine_tuning`, `idle` |
| 18 | Batch Size | `batch_size` | `tamanho_batch` | Número absoluto | Numérico | Workload IA | 1 a 4096 |
| 19 | Número de Épocas | `num_epochs` | `numero_epocas` | Número absoluto | Numérico | Workload IA | 1 a 300 |
| 20 | Tamanho do Modelo | `model_parameter_size_million` | `tamanho_modelo_milhoes_parametros` | Milhões de parâmetros | Numérico | Workload IA | 1 a 70000 |
| 21 | Quantidade de Amostras de Treinamento | `training_samples` | `amostras_treinamento` | Número absoluto | Numérico | Workload IA | 1000 a 100000000 |
| 22 | Duração do Job | `job_duration_hours` | `duracao_job_horas` | Horas | Numérico | Operacional | 0.05 a 240 |
| 23 | Job Status / Exit Code | `job_status` | `status_job` | N/A | Categórico | Operacional | `success`, `failed`, `aborted`, `running` |
| 24 | Cor da Etiqueta do Rack | `rack_label_color` | `cor_etiqueta_rack` | N/A | Categórico | Controle / Irrelevante | `blue`, `green`, `yellow`, `red`, `white` |
| 25 | Nível de Risco de Desperdício Ambiental | `environmental_waste_risk_level` | `nivel_risco_desperdicio_ambiental` | N/A | Categórico | Classe | `baixo`, `moderado`, `alto` |