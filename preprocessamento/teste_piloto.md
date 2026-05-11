# Metodologia do Teste Piloto

## 1. Objetivo do Teste Piloto

O teste piloto tem como objetivo realizar uma análise exploratória inicial do dataset original antes da aplicação de qualquer técnica de pré-processamento.

Essa etapa busca investigar a estrutura do dataset, suas distribuições, valores faltantes, ruídos, outliers, inconsistências e relações entre atributos, de modo que as decisões posteriores de pré-processamento sejam guiadas por evidências observadas nos dados.

O dataset analisado corresponde ao arquivo:

```bash
dataset/dataset_original.arff
```

Esse arquivo representa a versão original do dataset sintético, contendo:

* registros sintéticos gerados por LLM;
* valores faltantes inseridos de forma controlada;
* ruído controlado;
* outliers interpretáveis;
* atributos irrelevantes planejados;
* classe-alvo `environmental_waste_risk_level`.

---

## 2. Caracterização do Dataset

O dataset foi construído para a tarefa de **classificação do nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de IA**.

Cada instância representa:

> Um rack de datacenter em uma hora de operação.

O dataset original possui:

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

## 3. Justificativa do Teste Piloto

O teste piloto é necessário porque o pré-processamento não deve ser aplicado de forma mecânica. Antes de remover atributos, substituir valores faltantes, tratar outliers ou aplicar filtros, é preciso compreender o comportamento real do dataset.

A análise exploratória inicial permite:

* verificar se o dataset foi carregado corretamente no Weka;
* identificar problemas estruturais;
* observar distribuições dos atributos;
* localizar valores faltantes;
* verificar se os outliers são interpretáveis;
* analisar se há atributos muito dominantes;
* avaliar se os atributos irrelevantes realmente não possuem relação com a classe;
* levantar hipóteses para orientar o pré-processamento.

Assim, as decisões posteriores serão baseadas em evidências observadas nos dados, e não apenas em regras automáticas.

---

## 4. Organização da Equipe

A análise será realizada por três integrantes, cada um responsável por um eixo de investigação. Essa divisão permite que o dataset seja analisado sob perspectivas diferentes.

| Responsável  | Eixo de Análise | Objetivo |
| --- | --- | --- |
| Calil Lima, Tiago Santos, Wamberson Pacheco | Integridade estrutural e qualidade dos dados | Verificar se o dataset está tecnicamente correto e compatível com o Weka |
| Calil Lima, Tiago Santos, Wamberson Pacheco | Distribuições, valores faltantes, ruído e outliers | Investigar o comportamento estatístico dos atributos |
| Calil Lima, Tiago Santos, Wamberson Pacheco | Relações semânticas e relação com a classe-alvo | Avaliar se as relações entre atributos e classes são coerentes |

Cada integrante deverá registrar seus achados, evidências e hipóteses. Ao final, os resultados serão consolidados em uma análise única para orientar as decisões de pré-processamento.

---

# 5. Etapas do Teste Piloto

## 5.1. Etapa 1 — Verificação de Integridade Estrutural

### Objetivo

Verificar se o dataset está corretamente estruturado e pode ser utilizado no Weka sem erros técnicos.

### O que será analisado

| Verificação          | Descrição                                                                         |
| -------------------- | --------------------------------------------------------------------------------- |
| Carregamento no Weka | Verificar se o arquivo `.arff` abre corretamente                                  |
| Número de instâncias | Confirmar a quantidade total de registros                                         |
| Número de atributos  | Confirmar se existem 30 atributos                                                 |
| Classe-alvo          | Verificar se `environmental_waste_risk_level` foi reconhecida como nominal        |
| Tipos dos atributos  | Verificar se os atributos numéricos e categóricos foram reconhecidos corretamente |
| Valores faltantes    | Confirmar a presença de `?` nos atributos planejados                              |
| Categorias válidas   | Verificar se atributos nominais possuem apenas categorias previstas               |
| Linhas quebradas     | Verificar se existem registros mal formatados                                     |
| Duplicatas           | Verificar se há registros 100% idênticos em excesso                               |

### Perguntas orientadoras

* O dataset abriu corretamente no Weka?
* A classe-alvo foi reconhecida como nominal?
* Os atributos numéricos foram reconhecidos como `numeric`?
* Os atributos categóricos foram reconhecidos como `nominal`?
* Existem valores faltantes fora dos atributos planejados?
* Existem categorias inesperadas?
* Existem linhas quebradas ou colunas deslocadas?

### Evidências esperadas

As evidências podem incluir:

* print da tela de carregamento do Weka;
* estatísticas exibidas pelo Weka;
* contagem de instâncias;
* contagem de atributos;
* tabela com atributos que possuem valores faltantes;
* observações sobre problemas encontrados.

---

## 5.2. Etapa 2 — Análise Estatística Descritiva

### Objetivo

Investigar o comportamento geral dos atributos numéricos e categóricos, observando médias, dispersões, frequências e distribuições.

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

### Perguntas orientadoras

* Os valores mínimos e máximos estão dentro das faixas planejadas?
* Algum atributo possui dispersão muito alta?
* Algum atributo parece artificial demais?
* As classes estão distribuídas de forma adequada?
* A distribuição dos atributos irrelevantes parece aleatória?
* Existem atributos com distribuição muito separada por classe?

### Evidências esperadas

* prints dos histogramas do Weka;
* tabelas com estatísticas básicas;
* comentários sobre distribuições relevantes;
* identificação de atributos com maior dispersão;
* identificação de atributos com comportamento suspeito.

---

## 5.3. Etapa 3 — Análise de Valores Faltantes

### Objetivo

Verificar se os valores faltantes foram inseridos conforme o planejamento e levantar hipóteses sobre o tratamento posterior.

### Atributos candidatos a valores faltantes

```bash
gpu_temperature_c
fan_speed_rpm
water_usage_effectiveness
carbon_intensity_gco2_kwh
job_status
```

### O que será analisado

| Verificação                     | Descrição                                                    |
| ------------------------------- | ------------------------------------------------------------ |
| Quantidade de valores faltantes | Contar quantos `?` existem no dataset                        |
| Atributos afetados              | Verificar em quais colunas aparecem valores faltantes        |
| Proporção de faltantes          | Verificar se está próxima da proporção planejada             |
| Distribuição por classe         | Observar se faltantes aparecem concentrados em alguma classe |
| Impacto potencial               | Avaliar se os faltantes afetam atributos críticos            |

### Perguntas orientadoras

* Os valores faltantes aparecem apenas nos atributos planejados?
* A proporção de valores faltantes é aceitável?
* Há concentração de faltantes em alguma classe?
* Os faltantes ocorrem em atributos importantes para o modelo?
* Qual técnica de tratamento parece mais adequada: média, mediana, moda ou filtro do Weka?

### Evidências esperadas

| Atributo                    | Quantidade de faltantes | Percentual | Observação |
| --------------------------- | ----------------------: | ---------: | ---------- |
| `gpu_temperature_c`         |               preencher |  preencher | preencher  |
| `fan_speed_rpm`             |               preencher |  preencher | preencher  |
| `water_usage_effectiveness` |               preencher |  preencher | preencher  |
| `carbon_intensity_gco2_kwh` |               preencher |  preencher | preencher  |
| `job_status`                |               preencher |  preencher | preencher  |

---

## 5.4. Etapa 4 — Análise de Ruído

### Objetivo

Verificar se o ruído inserido no dataset é leve, plausível e compatível com o domínio.

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

### O que será analisado

| Verificação                | Descrição                                                                            |
| -------------------------- | ------------------------------------------------------------------------------------ |
| Pequenas oscilações        | Observar variações plausíveis em atributos numéricos                                 |
| Coerência potência-energia | Verificar relação entre `active_power_w` e `energy_consumption_kwh`                  |
| Coerência térmica          | Verificar relação entre `inlet_temperature_c`, `exhaust_temperature_c` e `delta_t_c` |
| Valores fora de faixa      | Verificar se o ruído gerou valores inválidos                                         |
| Impacto na classe          | Observar se o ruído tornou algum registro incoerente                                 |

### Perguntas orientadoras

* O ruído permanece dentro de faixas plausíveis?
* Existem registros em que `energy_consumption_kwh` ficou incoerente com `active_power_w`?
* Existem registros em que `delta_t_c` ficou incoerente com as temperaturas?
* O ruído parece simular variações reais ou parece aleatório demais?
* O ruído deve ser tratado no pré-processamento ou mantido?

### Evidências esperadas

| Relação analisada                                             | Critério                   | Achado    | Ação sugerida |
| ------------------------------------------------------------- | -------------------------- | --------- | ------------- |
| `active_power_w` × `energy_consumption_kwh`                   | Aproximação em 1 hora      | preencher | preencher     |
| `inlet_temperature_c` × `exhaust_temperature_c` × `delta_t_c` | Diferença térmica coerente | preencher | preencher     |
| Percentuais de utilização                                     | Entre 0 e 100              | preencher | preencher     |

---

## 5.5. Etapa 5 — Análise de Outliers

### Objetivo

Identificar outliers e avaliar se eles são interpretáveis, planejados e úteis para a tarefa de classificação.

### Tipos de outliers esperados

| Tipo de outlier   | Exemplo                                                  |
| ----------------- | -------------------------------------------------------- |
| Energético        | Alta potência com baixa utilização de CPU/GPU            |
| Térmico           | Temperatura elevada mesmo com fan speed alto             |
| Operacional       | `job_status = failed` ou `aborted` com longa duração     |
| Ambiental         | Alto consumo com alta intensidade de carbono ou alto WUE |
| Refrigeração      | Fan speed alto com baixa carga computacional             |
| Alocação de GPU   | `full_gpu` com baixa utilização de GPU                   |
| Densidade de rack | Alta densidade com refrigeração inadequada               |

### O que será analisado

| Verificação          | Descrição                                                          |
| -------------------- | ------------------------------------------------------------------ |
| Outliers numéricos   | Valores extremos em atributos como potência, temperatura e duração |
| Outliers relacionais | Combinações incomuns entre atributos                               |
| Outliers por classe  | Verificar se estão concentrados apenas em uma classe               |
| Plausibilidade       | Avaliar se os outliers têm interpretação no domínio                |
| Decisão futura       | Manter, tratar ou remover no pré-processamento                     |

### Perguntas orientadoras

* Os outliers são interpretáveis no contexto de datacenters de IA?
* Os outliers parecem erros ou eventos críticos plausíveis?
* Há outliers em todas as classes ou apenas na classe `alto`?
* Algum outlier compromete a coerência do dataset?
* Os outliers devem ser mantidos para o treinamento ou tratados no pré-processamento?

### Evidências esperadas

| Atributo ou relação                 | Outlier observado | Interpretação | Ação sugerida |
| ----------------------------------- | ----------------- | ------------- | ------------- |
| `gpu_temperature_c`                 | preencher         | preencher     | preencher     |
| `fan_speed_rpm`                     | preencher         | preencher     | preencher     |
| `job_duration_hours` + `job_status` | preencher         | preencher     | preencher     |
| `rack_power_density_kw`             | preencher         | preencher     | preencher     |

---

## 5.6. Etapa 6 — Análise da Classe-Alvo

### Objetivo

Verificar como a classe `environmental_waste_risk_level` está distribuída e se há separação excessiva entre as classes.

### O que será analisado

| Verificação                | Descrição                                                                      |
| -------------------------- | ------------------------------------------------------------------------------ |
| Distribuição das classes   | Quantidade de registros `baixo`, `moderado` e `alto`                           |
| Sobreposição entre classes | Verificar se atributos aparecem em faixas compartilhadas                       |
| Atributos dominantes       | Identificar se uma variável separa a classe sozinha                            |
| Casos de fronteira         | Observar registros próximos entre `baixo` e `moderado`, ou `moderado` e `alto` |
| Coerência semântica        | Avaliar se a classe é justificável pela combinação de atributos                |

### Atributos importantes para observar por classe

```bash
active_power_w
energy_consumption_kwh
water_usage_effectiveness
carbon_intensity_gco2_kwh
fan_speed_rpm
gpu_utilization_percent
gpu_power_w
gpu_temperature_c
job_duration_hours
job_status
rack_power_density_kw
gpu_sharing_mode
power_cap_w
```

### Perguntas orientadoras

* As três classes estão presentes?
* Há equilíbrio suficiente entre as classes?
* Existe algum atributo que separa quase perfeitamente uma classe?
* A classe `alto` é explicada por combinações de fatores ou por um único atributo?
* Existem casos de fronteira plausíveis?
* O dataset está adequado para testar algoritmos de classificação?

### Evidências esperadas

| Classe     | Quantidade | Características observadas | Possíveis problemas |
| ---------- | ---------: | -------------------------- | ------------------- |
| `baixo`    |  preencher | preencher                  | preencher           |
| `moderado` |  preencher | preencher                  | preencher           |
| `alto`     |  preencher | preencher                  | preencher           |

---

## 5.7. Etapa 7 — Análise dos Atributos Irrelevantes

### Objetivo

Verificar se os atributos irrelevantes planejados realmente não apresentam relação clara com a classe-alvo.

### Atributos analisados

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

### O que será analisado

| Verificação           | Descrição                                                  |
| --------------------- | ---------------------------------------------------------- |
| Frequência geral      | Distribuição dos valores de cada atributo                  |
| Frequência por classe | Verificar se algum valor aparece concentrado em uma classe |
| Relação com a classe  | Observar se há padrão artificial                           |
| Decisão futura        | Confirmar remoção ou manutenção temporária                 |

### Perguntas orientadoras

* Os atributos irrelevantes estão distribuídos de forma aleatória?
* Algum SKU, cor ou zona aparece associado demais a uma classe?
* Esses atributos podem induzir o modelo a aprender um padrão falso?
* Devem ser removidos no pré-processamento?

### Evidências esperadas

| Atributo              | Padrão observado | Relação com a classe? | Ação sugerida |
| --------------------- | ---------------- | --------------------- | ------------- |
| `manufacturer_sku_id` | preencher        | preencher             | preencher     |
| `rack_label_color`    | preencher        | preencher             | preencher     |
| `rack_inventory_zone` | preencher        | preencher             | preencher     |

---

## 5.8. Etapa 8 — Análise de Relações Semânticas

### Objetivo

Verificar se as principais regras semânticas usadas na geração do dataset aparecem de forma coerente nos dados.

### Relações prioritárias

| Relação                                                       | O que verificar                                             |
| ------------------------------------------------------------- | ----------------------------------------------------------- |
| `active_power_w` × `energy_consumption_kwh`                   | Energia deve ser compatível com potência em uma hora        |
| `inlet_temperature_c` × `exhaust_temperature_c` × `delta_t_c` | Delta T deve representar diferença térmica                  |
| `gpu_utilization_percent` × `gpu_power_w`                     | Alta utilização tende a maior potência                      |
| `gpu_utilization_percent` × `environmental_waste_risk_level`  | Baixa utilização com alta potência pode indicar desperdício |
| `fan_speed_rpm` × temperaturas                                | Fan speed deve acompanhar esforço térmico                   |
| `job_status` × `job_duration_hours`                           | Jobs falhos longos podem indicar desperdício                |
| `rack_power_density_kw` × classe                              | Verificar risco de dominância                               |
| `gpu_sharing_mode` × `gpu_utilization_percent`                | GPU inteira com baixa utilização pode indicar desperdício   |

### Perguntas orientadoras

* As relações físicas e operacionais fazem sentido?
* Há registros incoerentes com as regras semânticas?
* Há casos de desperdício alto justificados por combinação de fatores?
* Há atributos redundantes ou altamente correlacionados?
* Existem relações que indicam necessidade de normalização, remoção ou transformação?

### Evidências esperadas

| Relação analisada       | Achado    | Interpretação | Ação sugerida |
| ----------------------- | --------- | ------------- | ------------- |
| Potência × energia      | preencher | preencher     | preencher     |
| Temperaturas × Delta T  | preencher | preencher     | preencher     |
| GPU util × GPU power    | preencher | preencher     | preencher     |
| Fan speed × temperatura | preencher | preencher     | preencher     |
| Job status × duração    | preencher | preencher     | preencher     |

---

# 6. Matriz de Registro dos Achados

Todos os achados relevantes do teste piloto deverão ser registrados em uma matriz comum.

| ID | Eixo         | Atributo(s) analisado(s) | Achado observado | Evidência | Hipótese  | Impacto no pré-processamento | Ação sugerida |
| -- | ------------ | ------------------------ | ---------------- | --------- | --------- | ---------------------------- | ------------- |
| A1 | Integridade  | preencher                | preencher        | preencher | preencher | preencher                    | preencher     |
| A2 | Faltantes    | preencher                | preencher        | preencher | preencher | preencher                    | preencher     |
| A3 | Outliers     | preencher                | preencher        | preencher | preencher | preencher                    | preencher     |
| A4 | Classe-alvo  | preencher                | preencher        | preencher | preencher | preencher                    | preencher     |
| A5 | Irrelevantes | preencher                | preencher        | preencher | preencher | preencher                    | preencher     |

Exemplo de preenchimento:

| ID | Eixo         | Atributo(s) analisado(s)            | Achado observado        | Evidência                           | Hipótese                       | Impacto no pré-processamento          | Ação sugerida                         |
| -- | ------------ | ----------------------------------- | ----------------------- | ----------------------------------- | ------------------------------ | ------------------------------------- | ------------------------------------- |
| A1 | Faltantes    | `gpu_temperature_c`                 | Presença de valores `?` | Contagem no Weka                    | Falha simulada de sensor       | Necessita tratamento de missing value | Testar substituição por média/mediana |
| A2 | Irrelevantes | `rack_label_color`                  | Sem padrão por classe   | Frequência semelhante entre classes | Atributo realmente irrelevante | Pode ser removido                     | Aplicar filtro Remove                 |
| A3 | Outliers     | `job_duration_hours` + `job_status` | Jobs falhos longos      | Registros `failed` com duração alta | Outlier operacional planejado  | Pode ser mantido ou analisado         | Manter inicialmente                   |

---

# 7. Critérios para Decisões de Pré-processamento

As decisões de pré-processamento deverão ser tomadas apenas após a análise exploratória inicial.

| Problema identificado                      | Possível decisão de pré-processamento            |
| ------------------------------------------ | ------------------------------------------------ |
| Valores faltantes em atributos numéricos   | Substituir por média, mediana ou técnica do Weka |
| Valores faltantes em atributos categóricos | Substituir pela moda ou categoria mais frequente |
| Atributos irrelevantes confirmados         | Remover antes do treinamento                     |
| Outliers interpretáveis                    | Manter ou tratar com cautela                     |
| Outliers absurdos                          | Corrigir ou remover                              |
| Ruído leve e plausível                     | Manter ou suavizar conforme impacto              |
| Atributo dominante demais                  | Testar modelos com e sem o atributo              |
| Escalas numéricas muito diferentes         | Considerar normalização                          |
| Algoritmos sensíveis à escala              | Aplicar normalização para KNN e SVM              |
| Muitos atributos categóricos               | Verificar tratamento adequado no Weka            |
| Colinearidade forte entre atributos        | Avaliar remoção ou manutenção conforme algoritmo |

---

# 8. Ferramentas Utilizadas

O teste piloto será realizado exclusivamente com o **Weka**, utilizando recursos de visualização, estatísticas e filtros.

| Ferramenta                  | Uso                                                                     |
| --------------------------- | ----------------------------------------------------------------------- |
| Weka                        | Carregamento do ARFF, estatísticas, histogramas, visualização e filtros |
| Editor de texto ou planilha | Inspeção manual de linhas específicas                                   |
| Markdown                    | Documentação dos achados                                                |
| GitHub                      | Registro dos arquivos e histórico do trabalho                           |

---

# 9. Evidências a Serem Coletadas

Durante o teste piloto, deverão ser coletadas evidências suficientes para justificar as decisões posteriores.

As evidências podem incluir:

* prints da tela de carregamento do Weka;
* prints de histogramas;
* estatísticas dos atributos;
* contagens de valores faltantes;
* exemplos de registros suspeitos;
* tabelas de frequência por classe;
* matriz de achados;
* justificativas para cada decisão de pré-processamento.

As evidências deverão ser registradas no arquivo:

```bash
preprocessamento/analise_inicial.md
```

---

# 10. Produtos Esperados

Ao final do teste piloto, deverão ser produzidos os seguintes artefatos:

| Artefato                               | Finalidade                                                      |
| -------------------------------------- | --------------------------------------------------------------- |
| `preprocessamento/analise_inicial.md`  | Documentar a análise exploratória inicial                       |
| `preprocessamento/descricao_etapas.md` | Descrever as decisões de pré-processamento a partir dos achados |
| Prints do Weka                         | Evidenciar distribuições, estatísticas e problemas observados   |
| Matriz de achados                      | Organizar problemas, hipóteses e ações propostas                |
| Lista de decisões de pré-processamento | Indicar quais técnicas serão aplicadas e por quê                |

Ao final do teste piloto, a equipe deverá emitir um parecer sobre a qualidade inicial do dataset e indicar quais ações de pré-processamento serão necessárias.

A conclusão deverá responder:

* O dataset está estruturalmente correto?
* Os valores faltantes estão presentes e controlados?
* O ruído é plausível?
* Os outliers são interpretáveis?
* Há atributos irrelevantes que devem ser removidos?
* Há atributos que podem dominar a classificação?
* É necessário normalizar os dados?
* Quais decisões de pré-processamento serão aplicadas?
* O dataset está apto para seguir para a etapa de pré-processamento?

O resultado esperado é demonstrar que o pré-processamento será orientado por uma análise exploratória documentada, e não aplicado de forma automática ou sem justificativa.

