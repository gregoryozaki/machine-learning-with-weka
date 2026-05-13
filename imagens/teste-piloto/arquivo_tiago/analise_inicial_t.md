

## Etapa 1 — Verificação de Integridade Estrutural

### Objetivo

Verificar se o dataset está corretamente estruturado e pode ser utilizado no Weka sem erros técnicos.


-  Carregamento no Weka: Verificar se o arquivo `.arff` abre corretamente  
- Número de instâncias: Confirmar a quantidade total de registros  
- Número de atributos: Confirmar se existem 30 atributos  
- Classe-alvo: Verificar se `environmental_waste_risk_level` foi reconhecida como nominal  
- Tipos dos atributos: Verificar se os atributos numéricos e categóricos foram reconhecidos corretamente  
- Valores faltantes: Confirmar a presença de `?` nos atributos planejados  
- Categorias válidas: Verificar se atributos nominais possuem apenas categorias previstas  
- Linhas quebradas: Verificar se existem registros mal formatados  
- Duplicatas: Verificar se há registros 100% idênticos em excesso  


| Atributo                     | Valores Faltantes |
|------------------------------|-------------------|
| water_usage_effectiveness    | 7                 |
| carbon_intensity_gco2_kwh    | 7                 |
| fan_speed_rpm                | 7                 |
| gpu_temperature_c            | 7                 |
| job_status                   | 6                 |



### Observações

- Atributo `num_gpus` apesar de ser expresso com números, é um atributo categórico, possuindo 5 valores possíveis
- Não foram encontradas instâncias idênticas

---

##  Etapa 2 — Análise Estatística Descritiva

### Objetivo

Investigar o comportamento geral dos atributos numéricos e categóricos, observando médias, dispersões, frequências e distribuições.

- Mínimo e máximo: Verificar se os valores estão dentro das faixas planejadas  
- Média: Observar tendência central  
- Mediana: Verificar efeito de valores extremos  
- Desvio padrão: Avaliar dispersão  
- Distribuição: Observar formato dos histogramas  
- Frequência de categorias: Verificar distribuição dos atributos nominais  
- Distribuição da classe: Verificar quantidade de registros por classe  

---

##  Etapa 3 — Análise de Valores Faltantes

### Objetivo

Verificar se os valores faltantes foram inseridos conforme o planejamento e levantar hipóteses sobre o tratamento posterior.


- Quantidade de valores faltantes: Contar quantos `?` existem no dataset  
- Atributos afetados: Verificar em quais colunas aparecem valores faltantes  
- Proporção de faltantes: Verificar se está próxima da proporção planejada  
- Distribuição por classe: Observar se faltantes aparecem concentrados em alguma classe  
- Impacto potencial: Avaliar se os faltantes afetam atributos críticos  

---

##  Etapa 4 — Análise de Ruído

### Objetivo

Verificar se o ruído inserido no dataset é leve, plausível e compatível com o domínio.

- Pequenas oscilações: Observar variações plausíveis em atributos numéricos  
- Coerência potência-energia: Verificar relação entre `active_power_w` e `energy_consumption_kwh`  
- Coerência térmica: Verificar relação entre `inlet_temperature_c`, `exhaust_temperature_c` e `delta_t_c`  
- Valores fora de faixa: Verificar se o ruído gerou valores inválidos  
- Impacto na classe: Observar se o ruído tornou algum registro incoerente  

---

##  Etapa 5 — Análise de Outliers

### Objetivo

Identificar outliers e avaliar se eles são interpretáveis, planejados e úteis para a tarefa de classificação.

- Outliers numéricos: Valores extremos em atributos como potência, temperatura e duração  
- Outliers relacionais: Combinações incomuns entre atributos  
- Outliers por classe: Verificar se estão concentrados apenas em uma classe  
- Plausibilidade: Avaliar se os outliers têm interpretação no domínio  
- Decisão futura: Manter, tratar ou remover no pré-processamento  

---

##  Etapa 6 — Análise da Classe-Alvo

### Objetivo

Verificar como a classe `environmental_waste_risk_level` está distribuída e se há separação excessiva entre as classes.

- Distribuição das classes: Quantidade de registros `baixo`, `moderado` e `alto`  
- Sobreposição entre classes: Verificar se atributos aparecem em faixas compartilhadas  
- Atributos dominantes: Identificar se uma variável separa a classe sozinha  
- Casos de fronteira: Observar registros próximos entre classes  
- Coerência semântica: Avaliar se a classe é justificável  

---

##  Etapa 7 — Análise dos Atributos Irrelevantes

### Objetivo

Verificar se os atributos irrelevantes planejados realmente não apresentam relação clara com a classe-alvo.

- Frequência geral: Distribuição dos valores de cada atributo  
- Frequência por classe: Verificar se algum valor aparece concentrado em uma classe  
- Relação com a classe: Observar se há padrão artificial  
- Decisão futura: Confirmar remoção ou manutenção temporária  

---

##  Etapa 8 — Análise de Relações Semânticas

### Objetivo

Verificar se as principais regras semânticas usadas na geração do dataset aparecem de forma coerente nos dados.

- `active_power_w` × `energy_consumption_kwh`: Energia compatível com potência  
- `inlet_temperature_c` × `exhaust_temperature_c` × `delta_t_c`: Delta T coerente  
- `gpu_utilization_percent` × `gpu_power_w`: Uso alto → maior potência  
- `gpu_utilization_percent` × `environmental_waste_risk_level`: Baixa util. com alta potência pode indicar desperdício  
- `fan_speed_rpm` × temperaturas: Ventilação acompanha calor  
- `job_status` × `job_duration_hours`: Falhas longas podem indicar desperdício  
- `rack_power_density_kw` × classe: Verificar dominância  
- `gpu_sharing_mode` × `gpu_utilization_percent`: GPU ociosa pode indicar desperdício  