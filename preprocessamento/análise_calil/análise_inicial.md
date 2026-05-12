# Teste Piloto - Dataset Classificação do Nível de Risco de Desperdício Ambiental em Racks de Datacenters Voltados a Cargas de IA.

### Objetivo:
Realizar uma análise exploratória inicial do dataset original antes da aplicação de qualquer técnica de pré-processamento.

### Equipe de Testes: Calil Lima, Tiago Santos, Wamberson Pacheco

----

## Etapa 1 - Verificação de Integridade Estrutural
### Objetivo da etapa:
Verificar se o dataset está tecnicamente correto, se abre no Weka sem erro e se sua estrutura corresponde ao que foi planejado.

### O que será analisado

| Verificação          | Descrição                                                                         |
| -------------------- | --------------------------------------------------------------------------------- |
| Carregamento no Weka | Verificar se o arquivo `.arff` abre corretamente                                  |
| Número de instancias | Confirmar a quantidade total de registros                                         |
| Número de atributos  | Confirmar se existem 30 atributos                                                 |
| Classe-alvo          | Verificar se `environmental_waste_risk_level` foi reconhecida como nominal        |
| Tipos dos atributos  | Verificar se os atributos numéricos e categóricos foram reconhecidos corretamente |
| Valores faltantes    | Confirmar a presença de `?` nos atributos planejados                              |
| Categorias válidas   | Verificar se atributos nominais possuem apenas categorias previstas               |
| Linhas quebradas     | Verificar se existem registros mal formatados                                     |
| Duplicatas           | Verificar se há registros 100% idênticos em excesso                               |

----

### Carregamento no Weka e verificação de instâncias e atributos:

![conteudoWeka](<../imagens/Captura de tela 2026-05-12 001241.png>)

<p align="justify">
O dataset dataset_original.arff foi carregado corretamente no Weka, sem apresentar erros de leitura ou formatação. 
Foram identificadas 674 instâncias e 30 atributos, conforme especificado na metodologia do teste piloto. 
A classe-alvo environmental_waste_risk_level foi reconhecida como nominal, contendo as categorias baixo, moderado e alto. Presença de valores faltantes planejados confirmada.
Os atributos numéricos e categóricos também foram reconhecidos adequadamente, indicando que o arquivo ARFF está estruturalmente válido para a etapa de análise exploratória.
</p>

### Registro de Achados:

| ID | Eixo        | Atributo(s) analisado(s) | Achado observado                       | Evidência                     | Hipótese              | Impacto no pré-processamento | Ação sugerida           |
| -- | ----------- | ------------------------ | -------------------------------------- | ----------------------------- | --------------------- | ---------------------------- | ----------------------- |
| A1 | Integridade | Dataset completo         | Arquivo carregado corretamente no Weka | 674 instâncias e 30 atributos | Estrutura ARFF válida | Dataset apto para análise    | Manter arquivo original |

----

## Etapa 2 - Análise Estatística Descritiva
### Objetivo da etapa:
Investigar o comportamento geral dos atributos numéricos e categóricos, observando médias, dispersões, frequências e distribuições.

### O que será analisado

| Tipo de análise          | Descrição                                                  |
| ------------------------ | ---------------------------------------------------------- |
| Mínimo e máximo          | Verificar se os valores estão dentro das faixas planejadas |
| Média                    | Observar tendência central                                 |
| Mediana                  | Verificar efeito de valores extremos                       |
| Desvio padrão            | Avaliar dispersão                                          |
| Distribuição             | Observar formato dos histogramas                           |
| Frequência de categorias | Verificar distribuição dos atributos nominais              |
| Distribuição da classe   | Verificar quantidade de registros por classe               |

----

### Histogramas dos principais atributos:

#### Atributo active_power_w:

<div align="center">
  <img src="../imagens/activatePowerHist.png" alt="conteudoWeka">
</div>

----

#### Atributo energy_consumption_kwh:

<div align="center">
  <img src="../imagens/energyConsulHist.png" alt="conteudoWeka">
</div>

----

#### Atributo water_usage_effectiveness:

<div align="center">
  <img src="../imagens/waterUsageHist.png" alt="conteudoWeka">
</div>

----

#### Atributo carbon_intensity_gco2_kwh:

<div align="center">
  <img src="../imagens/carbonIntensityHist.png" alt="conteudoWeka">
</div>

----

#### Atributo inlet_temperature_c:

<div align="center">
  <img src="../imagens/inletTemperatureHist.png" alt="conteudoWeka">
</div>

----

#### Atributo exhaust_temperature_c:

<div align="center">
  <img src="../imagens/exhaustTemperatureHist.png" alt="conteudoWeka">
</div>

----

#### Atributo delta_t_c:

<div align="center">
  <img src="../imagens/deltaTHist.png" alt="conteudoWeka">
</div>

----

#### Atributo fan_speed_rpm:

<div align="center">
  <img src="../imagens/fanSpeedHist.png" alt="conteudoWeka">
</div>

----

#### Atributo gpu_power_w:

<div align="center">
  <img src="../imagens/gpuPowerHist.png" alt="conteudoWeka">
</div>

----

#### Atributo gpu_utilization_percent:

<div align="center">
  <img src="../imagens/gpuUtilizationHist.png" alt="conteudoWeka">
</div>

----

#### Atributo gpu_temperature_c:

<div align="center">
  <img src="../imagens/GpuTemperatureHist.png" alt="conteudoWeka">
</div>

----

#### Atributo job_duration_hours:

<div align="center">
  <img src="../imagens/jobDurationHist.png" alt="conteudoWeka">
</div>

----

#### Atributo rack_power_density_kw:

<div align="center">
  <img src="../imagens/rackPowerHist.png" alt="conteudoWeka">
</div>

----

#### Atributo power_cap_w:

<div align="center">
  <img src="../imagens/powerCapHist.png" alt="conteudoWeka">
</div>

----


### Estatísticas principais observadas:

| Atributo                    | Mínimo | Máximo |    Média | Interpretação                                    |
| --------------------------- | -----: | -----: | -------: | ------------------------------------------------ |
| `active_power_w`            |    600 |  11980 |  5737,11 | Grande variação de potência                      |
| `energy_consumption_kwh`    |   0,60 |  12,00 |     5,74 | Compatível com operação de 1 hora                |
| `water_usage_effectiveness` |   0,28 |   4,96 |     1,25 | Variação ambiental relevante                     |
| `carbon_intensity_gco2_kwh` |     62 |    891 |   377,66 | Variação significativa de intensidade de carbono |
| `inlet_temperature_c`       |  18,20 |  31,00 |    24,00 | Faixa plausível de entrada                       |
| `exhaust_temperature_c`     |  25,80 |  74,00 |    51,74 | Há temperaturas de exaustão elevadas             |
| `delta_t_c`                 |   6,60 |  43,00 |    27,74 | Grande diferença térmica em alguns racks         |
| `fan_speed_rpm`             |   1948 |  22000 | 11612,79 | Alta dispersão no esforço de refrigeração        |
| `gpu_power_w`               |     55 |    670 |   401,26 | Variação relevante de consumo da GPU             |
| `gpu_utilization_percent`   |      2 |     94 |    55,66 | Existem casos de baixa e alta utilização         |
| `gpu_temperature_c`         |     32 |     95 |    74,80 | Existem temperaturas críticas                    |
| `job_duration_hours`        |   0,06 |    170 |    32,43 | Existem jobs muito longos                        |
| `rack_power_density_kw`     |      5 |    120 |    26,96 | Existem valores extremos de densidade            |
| `power_cap_w`               |    660 |  12000 |  7027,83 | Grande variação de limite de potência            |

### Observações:

Os atributos com maior dispersão e maior chance de comportamento crítico são:

```bash
active_power_w
fan_speed_rpm
gpu_temperature_c
job_duration_hours
rack_power_density_kw
power_cap_w
```

Esses atributos devem receber mais atenção porque podem influenciar diretamente a classificação do risco ambiental.

----

<p align="justify">
  A análise estatística descritiva revelou grande variação em atributos energéticos, térmicos e operacionais. 
  A potência ativa variou de 600 W a 11980 W, enquanto o consumo energético variou de 0,60 kWh a 12,00 kWh. 
  Também foram observados valores elevados em gpu_temperature_c, fan_speed_rpm, job_duration_hours e rack_power_density_kw, 
  indicando a presença de registros extremos que podem representar situações críticas de operação. 
  Esses valores não devem ser removidos automaticamente, 
  pois podem estar associados ao próprio fenômeno investigado: o desperdício ambiental em racks de datacenters voltados a cargas de IA.
</p>

### Registro de Achados:

| ID | Eixo                   | Atributo(s) analisado(s)                                                         | Achado observado             | Evidência                          | Hipótese                                    | Impacto no pré-processamento | Ação sugerida                                         |
| -- | ---------------------- | -------------------------------------------------------------------------------- | ---------------------------- | ---------------------------------- | ------------------------------------------- | ---------------------------- | ----------------------------------------------------- |
| A2 | Estatística descritiva | `active_power_w`, `fan_speed_rpm`, `job_duration_hours`, `rack_power_density_kw` | Alta dispersão nos atributos | Histogramas e estatísticas do Weka | Existem cargas operacionais muito distintas | Pode exigir normalização     | Avaliar normalização em algoritmos sensíveis à escala |

----
