# Descrição das etapas de pré-processamento

#### Responsável: `Gregory Ozaki`

## Objetivo

Este documento descreve as etapas de pré-processamento aplicadas ao arquivo `dataset/dataset_original.arff` no Weka, com base nos achados do teste piloto registrados em `preprocessamento/02_analise_inicial.md`.

O objetivo não é limpar o dataset de forma automática, mas aplicar transformações justificadas por evidências observadas na análise inicial. Assim, o pré-processamento deve corrigir problemas objetivos da base, preservar relações semânticas relevantes e preparar o dataset para a etapa posterior de visualização pós-processamento.

Arquivo de entrada:

```bash
dataset/dataset_original.arff
```

Arquivo de saída principal:

```bash
dataset/dataset_preprocessado.arff
```

Arquivo de saída complementar:

```bash
dataset/dataset_preprocessado_attrselect.arff
```

---

## 1. Síntese dos achados do teste piloto

O teste piloto indicou que o dataset v2 está estruturalmente válido para uso no Weka. O arquivo foi carregado corretamente, possui 674 instâncias, 30 atributos, classe-alvo nominal e tipos de atributos reconhecidos adequadamente.

Os principais achados que orientam o pré-processamento foram:

| Achado | Impacto no pré-processamento |
|---|---|
| Valores faltantes em atributos ambientais, térmicos e operacionais | Devem ser tratados antes das próximas etapas |
| Atributos administrativos irrelevantes | Devem ser removidos |
| `num_gpus` reconhecido como numérico, mas com comportamento discreto | Deve ser convertido para nominal |
| Escalas numéricas muito diferentes | Exigem normalização |
| Ruído leve e plausível | Deve ser preservado |
| Outliers interpretáveis | Devem ser mantidos inicialmente |
| Possível dominância de `rack_power_density_kw` | Deve ser observada nas análises posteriores |
| Desbalanceamento moderado da classe `alto` | Deve ser considerado na interpretação dos resultados futuros |
| Relações semânticas coerentes | Devem ser preservadas |

---

## 2. Princípio adotado

O pré-processamento será aplicado de forma seletiva.

Nem todo valor extremo será removido. Alguns outliers representam situações críticas plausíveis no domínio, como alta temperatura, alto consumo, jobs longos, alta densidade energética ou baixa utilização de GPU com alto consumo. Esses casos são relevantes para caracterizar situações de risco e, por isso, não serão removidos automaticamente.

A distinção adotada será:

| Tipo de caso | Decisão |
|---|---|
| Valor faltante | Tratar com filtro do Weka |
| Atributo irrelevante | Remover |
| Atributo numérico com comportamento discreto | Converter para nominal, quando justificado |
| Escala numérica muito diferente | Normalizar |
| Ruído leve e plausível | Manter |
| Outlier interpretável | Manter inicialmente |
| Valor impossível ou erro estrutural | Corrigir ou remover, se identificado |
| Atributo potencialmente dominante | Manter e observar nas análises posteriores |

---

## 3. Etapa 1 — Carregamento do dataset no Weka

### Objetivo

Abrir o dataset original no Weka e confirmar que a classe-alvo está corretamente definida antes da aplicação dos filtros.

### Procedimento no Weka

```text
Explorer > Preprocess > Open file
```

Selecionar:

```bash
dataset/dataset_original.arff
```

Verificar se a classe selecionada é:

```bash
environmental_waste_risk_level
```

### Decisão

O arquivo `dataset_original.arff` será mantido sem alterações como versão bruta de referência.

---

## 4. Etapa 2 — Tratamento de valores faltantes

### Objetivo

Substituir valores ausentes nos atributos em que a ausência foi planejada e justificada no teste piloto.

### Justificativa

O teste piloto identificou valores faltantes apenas nos seguintes atributos:

```bash
gpu_temperature_c
fan_speed_rpm
water_usage_effectiveness
carbon_intensity_gco2_kwh
job_status
```

Como esses atributos são relevantes para a análise do risco de desperdício ambiental, a remoção de instâncias não é a melhor decisão inicial. A estratégia adotada será a imputação automática pelo Weka.

### Filtro do Weka

```bash
weka.filters.unsupervised.attribute.ReplaceMissingValues
```

### Procedimento no Weka

```text
Explorer > Preprocess > Filter > Choose > unsupervised > attribute > ReplaceMissingValues > Apply
```

### Resultado esperado

- Valores faltantes em atributos numéricos serão substituídos por medida central usada pelo filtro do Weka.
- Valores faltantes em atributos nominais, como `job_status`, serão substituídos pela categoria mais frequente.
- A quantidade de valores faltantes deve passar para zero.

### Evidência a registrar

```bash
imagens/prints_weka/preprocessamento/figura_58_replace_missing_values.png
```

---

## 5. Etapa 3 — Remoção de atributos irrelevantes

### Objetivo

Remover atributos administrativos que não possuem relação semântica direta com o risco de desperdício ambiental.

### Atributos removidos

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

### Justificativa

O teste piloto indicou que esses atributos representam informações administrativas ou artificiais. Eles não explicam diretamente consumo energético, temperatura, utilização computacional, refrigeração, duração de jobs ou densidade de potência.

Mantê-los poderia introduzir viés sintético nas etapas posteriores.

### Filtro do Weka

```bash
weka.filters.unsupervised.attribute.Remove
```

### Procedimento no Weka

```text
Explorer > Preprocess > Filter > Choose > unsupervised > attribute > Remove
```

Considerando a ordem original dos atributos, os atributos removidos correspondem às posições:

```bash
27,28,29
```

Configuração:

```bash
attributeIndices: 27-29
invertSelection: False
```

Depois aplicar:

```text
Apply
```

### Resultado esperado

O dataset passa de 30 atributos para 27 atributos, considerando a remoção dos três atributos irrelevantes e a manutenção da classe-alvo.

### Evidência a registrar

```bash
imagens/prints_weka/preprocessamento/figura_59_remove_atributos_irrelevantes.png
```

---

## 6. Etapa 4 — Conversão de `num_gpus` para nominal

### Objetivo

Converter o atributo `num_gpus` de numérico para nominal.

### Justificativa

No teste piloto, `num_gpus` foi reconhecido pelo Weka como atributo numérico. Porém, semanticamente, ele representa uma configuração discreta de hardware, assumindo valores específicos como 1, 2, 4, 8 e 16 GPUs.

A conversão para nominal evita que o atributo seja interpretado como uma medida contínua linear, como potência, temperatura ou duração. Isso torna a representação mais coerente com o domínio.

### Filtro do Weka

```bash
weka.filters.unsupervised.attribute.NumericToNominal
```

### Procedimento no Weka

```text
Explorer > Preprocess > Filter > Choose > unsupervised > attribute > NumericToNominal
```

Aplicar apenas ao atributo:

```bash
num_gpus
```

Na ordem original, `num_gpus` corresponde à posição:

```bash
15
```

Configuração:

```bash
attributeIndices: 15
```

Depois aplicar:

```text
Apply
```

### Resultado esperado

O atributo `num_gpus` passa de `numeric` para `nominal`, sem alteração na quantidade total de atributos.

### Evidência a registrar

```bash
imagens/prints_weka/preprocessamento/figura_60_numeric_to_nominal_num_gpus.png
```

---

## 7. Etapa 5 — Aplicação do filtro `RemoveUseless`

### Objetivo

Aplicar um filtro adicional do Weka para verificar se ainda existem atributos constantes ou com variação insuficiente após as etapas iniciais de pré-processamento.

### Justificativa

O teste piloto já havia identificado atributos irrelevantes planejados, removidos manualmente com o filtro `Remove`. Ainda assim, foi aplicado o filtro `RemoveUseless` como uma verificação automatizada adicional.

Esse filtro atende ao requisito de uso de filtro adicional do Weka e funciona como uma etapa preventiva: se algum atributo restante não apresentar variação útil, ele pode ser removido automaticamente.

### Filtro do Weka

```bash
weka.filters.unsupervised.attribute.RemoveUseless
```

### Procedimento no Weka

```text
Explorer > Preprocess > Filter > Choose > unsupervised > attribute > RemoveUseless > Apply
```

### Resultado observado

O filtro foi aplicado e não removeu nenhum atributo. Isso indica que, segundo o critério do Weka, os atributos restantes possuem variação suficiente para permanecer no dataset.

### Decisão

O resultado será registrado como evidência de verificação adicional. Como nenhum atributo foi removido, a estrutura do dataset permanece com 27 atributos nesta etapa.

### Evidência a registrar

```bash
imagens/prints_weka/preprocessamento/figura_61_remove_useless.png
```

---

## 8. Etapa 6 — Normalização dos atributos numéricos

### Objetivo

Ajustar as escalas dos atributos numéricos para reduzir o impacto de diferenças muito grandes entre unidades e faixas de valores.

### Justificativa

O teste piloto mostrou atributos em escalas muito diferentes:

| Atributo | Escala aproximada |
|---|---:|
| `gpu_utilization_percent` | 0 a 100 |
| `fan_speed_rpm` | milhares de rpm |
| `model_parameter_size_million` | até dezenas de milhares |
| `training_samples` | até dezenas de milhões |
| `active_power_w` | centenas a milhares de watts |
| `rack_power_density_kw` | até 120 kW |

Sem normalização, atributos com valores absolutos maiores poderiam influenciar desproporcionalmente análises e algoritmos sensíveis à escala.

### Filtro do Weka

```bash
weka.filters.unsupervised.attribute.Normalize
```

### Procedimento no Weka

```text
Explorer > Preprocess > Filter > Choose > unsupervised > attribute > Normalize > Apply
```

### Resultado esperado

Os atributos numéricos passam a ter escala comparável, geralmente entre 0 e 1.

O atributo `num_gpus`, já convertido para nominal, não deve ser tratado como atributo numérico contínuo nesta etapa.

### Evidência a registrar

```bash
imagens/prints_weka/preprocessamento/figura_62_normalize_atributos_numericos.png
```

---

## 9. Etapa 7 — Salvamento da versão principal preprocessada

### Objetivo

Salvar a versão principal do dataset após a aplicação dos filtros definidos para o pré-processamento.

### Filtros aplicados na versão principal

| Ordem | Filtro | Finalidade |
|---:|---|---|
| 1 | `ReplaceMissingValues` | Tratar valores faltantes |
| 2 | `Remove` | Remover atributos irrelevantes |
| 3 | `NumericToNominal` | Converter `num_gpus` para nominal |
| 4 | `RemoveUseless` | Verificar atributos constantes ou com variação insuficiente |
| 5 | `Normalize` | Ajustar escalas dos atributos numéricos |

### Arquivo de saída

```bash
dataset/dataset_preprocessado.arff
```

### Procedimento no Weka

```text
Explorer > Preprocess > Save
```

Salvar como:

```bash
dataset/dataset_preprocessado.arff
```

### Observação

O arquivo original deve permanecer inalterado:

```bash
dataset/dataset_original.arff
```

---

## 10. Etapa 8 — Versão complementar com `AttributeSelection`

### Objetivo

Criar uma versão alternativa do dataset para análise complementar da relevância dos atributos.

### Justificativa

O teste piloto identificou:

- possível redundância entre `active_power_w` e `energy_consumption_kwh`;
- possível dominância de `rack_power_density_kw`;
- atributos com diferentes níveis de contribuição para a classe-alvo.

Por isso, o filtro `AttributeSelection` será usado em uma cópia alternativa, não na versão principal. O objetivo é observar quais atributos o Weka considera mais relevantes, sem substituir a análise manual do teste piloto.

### Filtro do Weka

```bash
weka.filters.supervised.attribute.AttributeSelection
```

### Configuração adotada

```bash
Evaluator: CfsSubsetEval
Search: BestFirst
```

Essa configuração avalia subconjuntos de atributos considerando relevância em relação à classe e redundância entre atributos.

### Procedimento no Weka

A partir da versão preprocessada principal ou de uma cópia dela:

```text
Explorer > Preprocess > Filter > Choose > supervised > attribute > AttributeSelection
```

Configurar:

```bash
Evaluator: CfsSubsetEval
Search: BestFirst
```

Aplicar:

```text
Apply
```

### Resultado observado

A configuração `CfsSubsetEval + BestFirst` selecionou um subconjunto reduzido de atributos:

```bash
water_usage_effectiveness
inlet_temperature_c
gpu_utilization_percent
job_status
rack_power_density_kw
environmental_waste_risk_level
```

Essa seleção será tratada como análise complementar, pois remove vários atributos que possuem interpretação semântica relevante no teste piloto.

### Arquivo de saída complementar

```bash
dataset/dataset_preprocessado_attrselect.arff
```

### Evidência a registrar

```bash
imagens/prints_weka/preprocessamento/figura_63_attribute_selection.png
```

---

## 11. Ordem final das etapas no Weka

### Versão principal

```text
1. Abrir dataset_original.arff
2. Definir environmental_waste_risk_level como classe-alvo
3. Aplicar ReplaceMissingValues
4. Aplicar Remove nos atributos 27-29
5. Aplicar NumericToNominal no atributo num_gpus
6. Aplicar RemoveUseless
7. Aplicar Normalize nos atributos numéricos restantes
8. Salvar como dataset_preprocessado.arff
```

### Versão complementar

```text
1. Abrir uma cópia da versão preprocessada principal
2. Aplicar AttributeSelection com CfsSubsetEval + BestFirst
3. Salvar como dataset_preprocessado_attrselect.arff
```

---

## 12. Versões geradas

| Versão | Conteúdo | Finalidade |
|---|---|---|
| `dataset_original.arff` | Dataset sintético original | Referência bruta |
| `dataset_preprocessado.arff` | Faltantes tratados, irrelevantes removidos, `num_gpus` convertido, verificação com `RemoveUseless` e normalização | Versão principal preprocessada |
| `dataset_preprocessado_attrselect.arff` | Versão complementar com seleção automática de atributos | Análise complementar de relevância |

---

## 13. Relação com a visualização pós-processamento

Após o pré-processamento, será realizada uma etapa de visualização dos dados preprocessados. Essa etapa deverá verificar se as transformações aplicadas produziram os efeitos esperados.

Pontos a observar na visualização pós-processamento:

| Ponto | Verificação esperada |
|---|---|
| Valores faltantes | Confirmar ausência de missing values |
| Atributos irrelevantes | Confirmar remoção de `manufacturer_sku_id`, `rack_label_color` e `rack_inventory_zone` |
| `num_gpus` | Confirmar tipo nominal |
| `RemoveUseless` | Registrar que nenhum atributo foi removido |
| Normalização | Confirmar escala dos atributos numéricos entre 0 e 1 |
| Relações semânticas | Verificar se padrões principais foram preservados |
| Classe-alvo | Confirmar manutenção de `environmental_waste_risk_level` como nominal |

---

## 14. Conclusão

As etapas de pré-processamento foram definidas com base nos achados do teste piloto. O dataset v2 não apresentou erros estruturais graves, mas exigiu tratamento de valores faltantes, remoção de atributos irrelevantes, correção de representação do atributo `num_gpus` e normalização dos atributos numéricos.

A versão principal `dataset_preprocessado.arff` será composta por dados tratados e transformados sem remoção automática de ruídos ou outliers interpretáveis. Essa decisão preserva casos críticos relevantes ao problema, evitando uma limpeza mecânica que poderia eliminar padrões importantes.

A versão `dataset_preprocessado_attrselect.arff` será mantida apenas como complemento para análise de relevância dos atributos. Ela não substitui a versão principal, pois a seleção automática reduziu fortemente o conjunto de atributos e deve ser interpretada com cautela.

Com isso, o dataset fica preparado para a etapa seguinte: visualização e análise dos dados após o pré-processamento.
