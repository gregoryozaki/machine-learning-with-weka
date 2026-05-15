# Classificação do Nível de Risco de Desperdício Ambiental em Racks de Datacenters Voltados a Cargas de IA com Algoritmos de Aprendizado de Máquina

Gregory Gabriel Ozaki Coelho¹, Ana Paula Guimarães Xavier 2¹, Tiago dos Santos Mendonça 3¹, Gabriel Batista dos Santos 4¹, Calil Lima Pereira 5¹, Wamberson Pacheco Araújo 6¹

¹Instituto de Ciências Exatas e Tecnologia — Universidade Federal do Amazonas (ICET/UFAM)  
Itacoatiara — AM — Brasil

```bash
{gregory.coelho, tiago.mendonca, ana.xavier, gabriel-batista.santos, calil.lima, wamberson.pacheco}@ufam.edu.br
```

## Resumo

O crescimento de aplicações baseadas em inteligência artificial tem ampliado a demanda por infraestrutura computacional em datacenters, elevando o consumo energético, a necessidade de refrigeração e o impacto ambiental associado à operação de servidores e racks. Nesse contexto, este trabalho apresenta a construção e avaliação de um dataset sintético para classificação do nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de IA. Cada instância representa um rack em uma hora de operação, com atributos energéticos, térmicos, ambientais, computacionais e operacionais. O dataset foi gerado com apoio de LLMs, seguindo regras semânticas e incluindo valores faltantes, ruídos, outliers interpretáveis e atributos irrelevantes para permitir uma etapa completa de pré-processamento. O experimento foi conduzido no Weka, envolvendo teste piloto, pré-processamento, visualização e treinamento de classificadores. Foram avaliados os algoritmos `ZeroR`, `OneR`, `NaiveBayes`, `J48`, `RandomForest`, `IBk` e `SMO`, usando validação cruzada com 10 folds. Os resultados indicaram que o `RandomForest` com `numTrees = 200`, aplicado ao dataset preprocessado completo, obteve o melhor desempenho geral, com acurácia de 91,9881%, Kappa de 0,8769, F1 ponderado de 0,920 e recall de 0,975 para a classe `alto`. Conclui-se que a combinação de atributos energéticos, térmicos e operacionais permite classificar o risco de desperdício ambiental de forma consistente no cenário experimental analisado.

**Palavras-chave:** Datacenters; Desperdício ambiental; Aprendizado de máquina; Weka; Dataset sintético; Inteligência artificial.

---

## Abstract

The growth of artificial intelligence applications has increased the demand for computational infrastructure in data centers, raising energy consumption, cooling requirements, and the environmental impact associated with server and rack operation. In this context, this work presents the construction and evaluation of a synthetic dataset for classifying the environmental waste risk level in data center racks designed for AI workloads. Each instance represents a rack during one hour of operation, described by energy, thermal, environmental, computational, and operational attributes. The dataset was generated with the support of LLMs, following semantic rules and including missing values, noise, interpretable outliers, and irrelevant attributes to enable a complete preprocessing stage. The experiment was conducted in Weka, involving pilot testing, preprocessing, data visualization, and classifier training. The evaluated algorithms were `ZeroR`, `OneR`, `NaiveBayes`, `J48`, `RandomForest`, `IBk`, and `SMO`, using 10-fold cross-validation. The results showed that `RandomForest` with `numTrees = 200`, applied to the complete preprocessed dataset, achieved the best overall performance, with 91.9881% accuracy, 0.8769 Kappa, 0.920 weighted F1-score, and 0.975 recall for the `alto` class. The results indicate that combining energy, thermal, and operational attributes enables consistent classification of environmental waste risk in the experimental scenario analyzed.

**Keywords:** Data centers; Environmental waste; Machine learning; Weka; Synthetic dataset; Artificial intelligence.

---

## 1. Introdução

O crescimento de aplicações baseadas em inteligência artificial, computação em nuvem, big data e sistemas de alto desempenho tem ampliado a demanda por infraestrutura computacional em datacenters. Esses ambientes são responsáveis por sustentar serviços digitais essenciais, mas também concentram consumo expressivo de energia elétrica, necessidade contínua de refrigeração e uso intensivo de recursos computacionais. Com a expansão de cargas de IA, especialmente aquelas baseadas em GPUs e treinamentos de modelos, a eficiência operacional dos datacenters torna-se um problema técnico e ambiental relevante [Khan et al. 2023].

A análise do desperdício ambiental nesse contexto não deve considerar apenas o consumo absoluto de energia. Um rack pode apresentar alto consumo por estar executando uma carga computacional intensa e bem aproveitada, o que não necessariamente caracteriza desperdício. Por outro lado, situações com alta potência ativa, baixa utilização de GPU, temperatura elevada, refrigeração intensa, jobs interrompidos ou recursos subutilizados podem indicar operação ineficiente. Portanto, o desperdício ambiental deve ser entendido como uma condição associada à relação entre consumo energético, aproveitamento computacional, comportamento térmico e eficiência operacional.

A literatura sobre eficiência energética em datacenters mostra que variáveis como utilização de CPU e memória, consumo de potência, temperatura, sistemas de refrigeração e características das cargas de trabalho podem ser usadas para modelar o comportamento energético da infraestrutura [Ismail and Materwala 2020]. Estudos recentes também indicam que técnicas de análise de dados e aprendizado de máquina podem apoiar a identificação de padrões de consumo, previsão de estados operacionais e suporte à tomada de decisão em ambientes de alto desempenho [Chinnici et al. 2024]. Isso reforça a viabilidade de tratar o problema como uma tarefa de classificação supervisionada.

Neste trabalho, o foco está na classificação do nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de IA. Para isso, foi construído um dataset sintético no qual cada instância representa um rack em uma hora de operação. A classe-alvo, denominada `environmental_waste_risk_level`, possui três categorias: `baixo`, `moderado` e `alto`. Os atributos incluem variáveis energéticas, térmicas, computacionais, operacionais e ambientais, como potência ativa, consumo energético, temperatura de entrada e exaustão, utilização de GPU, duração de jobs, densidade de potência do rack e eficiência hídrica.

A construção de um dataset sintético foi adotada devido à dificuldade de acesso a bases públicas específicas para classificação de desperdício ambiental em nível de rack. Para tornar o experimento mais realista e compatível com os requisitos da atividade, o dataset incluiu valores faltantes, ruídos controlados, outliers interpretáveis e atributos irrelevantes. Essas características permitiram a aplicação de uma etapa completa de pré-processamento, incluindo tratamento de valores ausentes, remoção de atributos, conversão de tipos, normalização e seleção de atributos.

A metodologia experimental foi conduzida no Weka, envolvendo teste piloto, análise visual, pré-processamento, treinamento e avaliação de algoritmos de aprendizado de máquina. Foram testados os classificadores `ZeroR`, `OneR`, `NaiveBayes`, `J48`, `RandomForest`, `IBk` e `SMO`, utilizando validação cruzada com 10 folds. Além disso, foram comparadas duas versões do dataset: uma versão preprocessada completa e uma versão reduzida com `AttributeSelection`.

A principal contribuição deste trabalho está na estruturação de um problema de classificação multiclasse aplicado ao desperdício ambiental em racks de datacenter, articulando atributos energéticos, térmicos e operacionais em um dataset controlado. Com isso, busca-se avaliar quais algoritmos apresentam melhor desempenho na identificação de diferentes níveis de risco, com atenção especial à classe `alto`, que representa os casos mais críticos do ponto de vista ambiental.

Este relatório está organizado da seguinte forma: a Seção 2 apresenta os objetivos do trabalho; a Seção 3 descreve a fundamentação teórica e os trabalhos relacionados; a Seção 4 apresenta a metodologia utilizada na geração do dataset, pré-processamento, visualização e modelagem; a Seção 5 discute os resultados obtidos; e a Seção 6 apresenta as conclusões e possibilidades de trabalhos futuros.

---

## 2. Objetivos

### 2.1 Objetivo geral

Desenvolver e avaliar uma abordagem baseada em aprendizado de máquina para classificar o nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de inteligência artificial, utilizando um dataset sintético com atributos energéticos, térmicos, computacionais e operacionais.

### 2.2 Objetivos específicos

- Construir um dataset sintético representando racks de datacenter em uma hora de operação, contendo atributos coerentes com o domínio analisado.

- Aplicar etapas de teste piloto, pré-processamento e visualização no Weka para verificar a qualidade, a coerência e a estrutura dos dados.

- Avaliar diferentes algoritmos de classificação supervisionada no Weka, incluindo modelos simples, probabilísticos, baseados em árvores, vizinhança e margem.

- Comparar o desempenho dos modelos nas versões completa e reduzida do dataset, considerando métricas como acurácia, Kappa, F1 ponderado, recall por classe e matriz de confusão.

- Identificar o algoritmo e a configuração mais adequados para classificar o risco de desperdício ambiental, com atenção especial à classe `alto`.

## 3. Fundamentação Teórica

### 3.1 Datacenters, sustentabilidade e desperdício ambiental

Datacenters são infraestruturas essenciais para serviços digitais contemporâneos, como computação em nuvem, inteligência artificial, Internet das Coisas, big data e sistemas de alto desempenho. Essas infraestruturas concentram grande volume de processamento, armazenamento e transmissão de dados, mas também apresentam consumo significativo de energia elétrica e recursos ambientais [Khan et al. 2023].

O consumo energético de um datacenter envolve tanto os equipamentos de tecnologia da informação, como servidores, dispositivos de armazenamento e rede, quanto a infraestrutura de suporte, especialmente sistemas de refrigeração, distribuição elétrica e condicionamento térmico. Assim, o impacto ambiental não está associado apenas ao processamento computacional direto, mas também aos recursos necessários para manter a operação estável e segura [Ismail and Materwala 2020].

Nesse contexto, o desperdício ambiental pode ser entendido como o uso de energia, refrigeração e capacidade computacional sem aproveitamento proporcional. Um rack pode apresentar alto consumo energético mesmo com baixa utilização de CPU, memória ou GPU. Também pode demandar refrigeração elevada devido a temperaturas críticas, jobs longos, falhas de execução ou má alocação de cargas. Portanto, desperdício ambiental não é apenas “consumir muito”, mas consumir de forma ineficiente.

Rehan et al. (2025) discutem desperdícios em sistemas distribuídos, incluindo desperdício energético, desperdício de recursos e desperdício associado à pegada de carbono. Embora o foco dos autores esteja em sistemas distribuídos de processamento de dados, esses conceitos podem ser adaptados ao contexto de racks de datacenter. Neste trabalho, essa adaptação fundamenta a classificação dos registros em três níveis de risco: `baixo`, `moderado` e `alto`.

---

### 3.2 Eficiência energética e modelagem de potência

A modelagem de potência é relevante para compreender o comportamento energético de servidores e racks. Ismail e Materwala (2020) apresentam uma taxonomia de modelos de potência baseados em software, relacionando consumo de energia com métricas de desempenho do sistema, como uso de CPU, memória, disco, rede e contadores de hardware.

Essa perspectiva sustenta a escolha de atributos computacionais e energéticos no dataset, como:

```bash
active_power_w
energy_consumption_kwh
cpu_utilization_percent
memory_utilization_percent
gpu_power_w
gpu_utilization_percent
rack_power_density_kw
power_cap_w
```

Esses atributos permitem representar diferentes dimensões do consumo e da utilização dos recursos. Para o problema deste trabalho, a relação entre potência consumida e utilização efetiva é central, pois um cenário de alto consumo com baixa utilização tende a indicar maior risco de desperdício.

Além disso, métricas como rack_power_density_kw são importantes porque expressam a concentração de potência em um rack. Racks de maior densidade tendem a exigir maior atenção operacional, principalmente quando combinam alta potência, temperaturas elevadas e baixa eficiência computacional.

### 3.3 Aspectos térmicos e refrigeração em datacenters

A dimensão térmica é fundamental na análise de desperdício ambiental em datacenters. O aumento da temperatura dos equipamentos pode elevar a necessidade de refrigeração e, consequentemente, ampliar o consumo energético da infraestrutura.

Chinnici et al. (2024) analisam estratégias de sustentabilidade e eficiência energética em datacenters HPC, considerando dados operacionais, térmicos e de resfriamento. O estudo reforça que variáveis térmicas e operacionais podem ser usadas para compreender condições de ineficiência e apoiar decisões de gestão energética.

Com base nessa literatura, este trabalho considera atributos como:

```bash
inlet_temperature_c
exhaust_temperature_c
delta_t_c
fan_speed_rpm
cooling_method
gpu_temperature_c
``` 

Esses atributos permitem observar se o rack opera em condições térmicas plausíveis e se há esforço excessivo de refrigeração. Por exemplo, temperaturas elevadas combinadas com alta rotação de fans podem indicar maior pressão térmica. Quando esse comportamento ocorre junto de baixa utilização computacional, há indício de desperdício operacional e ambiental.

### 3.4 Aprendizado de máquina aplicado à eficiência energética

A complexidade operacional dos datacenters dificulta análises puramente manuais. A classificação do risco de desperdício ambiental envolve múltiplas variáveis energéticas, térmicas, computacionais e operacionais. Nesse cenário, técnicas de aprendizado de máquina são adequadas por permitirem identificar padrões em dados multivariados.

Khan et al. (2023) demonstram o uso de técnicas de análise de dados e aprendizado de máquina para gestão energética em datacenters, incluindo modelagem de comportamento energético, térmico e operacional. Chinnici et al. (2024) também mostram a aplicação de análise de dados em ambientes HPC para apoiar eficiência energética e sustentabilidade.

Neste trabalho, o problema é formulado como uma tarefa de classificação supervisionada. Cada instância representa um rack de datacenter em uma hora de operação, e o objetivo é prever a classe:

`environmental_waste_risk_level

com os valores:

```bash
baixo
moderado
alto
``` 

A abordagem supervisionada é adequada porque o dataset possui rótulos previamente definidos, permitindo que os algoritmos aprendam relações entre atributos e níveis de risco.

### 3.5 Algoritmos de classificação adotados

Foram utilizados algoritmos de classificação disponíveis no Weka e abordados em sala, incluindo modelos simples, probabilísticos, baseados em árvores, vizinhança e margem.

O ZeroR foi usado como baseline mínimo, pois sempre prediz a classe majoritária. O OneR também foi usado como baseline, criando uma regra baseada em apenas um atributo. Esses modelos não são esperados como os melhores, mas são importantes para verificar se algoritmos mais complexos realmente aprendem padrões acima de uma referência simples.

O NaiveBayes representa uma abordagem probabilística baseada no Teorema de Bayes. Embora seja eficiente e simples, assume independência condicional entre atributos, o que pode ser uma limitação neste dataset, já que existem relações evidentes entre variáveis, como potência ativa e consumo energético.

O J48, implementação de árvore de decisão no Weka, foi adotado por sua interpretabilidade. Ele gera regras baseadas em divisões sucessivas nos atributos, permitindo compreender quais variáveis aparecem nas decisões do modelo.

O RandomForest utiliza um conjunto de árvores de decisão, aumentando a robustez do modelo. Esse algoritmo é adequado para cenários com múltiplas variáveis, ruído e relações não lineares, características presentes no dataset deste trabalho.

O IBk, implementação do KNN no Weka, classifica instâncias com base em vizinhos próximos. Como depende de distância, sua aplicação exige normalização dos atributos numéricos. O SMO, por sua vez, implementa SVM no Weka e busca separar classes por meio de margens, também sendo sensível à escala dos dados.

### 3.6 Dataset sintético e ambiente experimental

A ausência de bases públicas diretamente alinhadas à classificação de desperdício ambiental em racks de datacenter motivou a construção de um dataset sintético. Essa decisão é aceitável em contexto experimental, desde que os dados sejam gerados com coerência semântica e com atributos fundamentados no domínio.

O dataset sintético construído neste trabalho inclui atributos energéticos, térmicos, computacionais, operacionais e administrativos. Também foram inseridos ruídos, valores ausentes, outliers interpretáveis e atributos irrelevantes, conforme exigência da atividade. Essa escolha permitiu realizar uma etapa completa de pré-processamento, visualização, treino e teste no Weka.

O Weka foi utilizado como ambiente experimental por oferecer recursos integrados para pré-processamento, visualização, aplicação de filtros, treinamento de classificadores e avaliação por validação cruzada. Kumar et al. (2020) também citam o Weka em contexto de avaliação de aprendizado de máquina e eficiência energética, reforçando sua relevância como ferramenta experimental em estudos acadêmicos.

### 3.7 Trabalhos Relacionados

Khan et al. (2023) propõem uma abordagem de análise avançada de dados para gestão energética em datacenters. O trabalho utiliza dados operacionais e modelos analíticos para apoiar a eficiência energética, com foco em sistemas de TI e refrigeração. A contribuição para este projeto está na demonstração de que variáveis energéticas, térmicas e operacionais podem ser combinadas em modelos de aprendizado de máquina para análise de eficiência em datacenters.

Chinnici et al. (2024) investigam sustentabilidade e eficiência energética em datacenters HPC, considerando características térmicas, dados de sensores e comportamento operacional. Esse estudo se relaciona diretamente com este trabalho ao evidenciar que variáveis como temperatura, resfriamento, potência e carga computacional são relevantes para caracterizar condições de operação eficientes ou ineficientes.

Ismail e Materwala (2020) apresentam uma revisão e taxonomia de modelos de potência em servidores de datacenter. O trabalho é importante para este projeto porque fundamenta a relação entre consumo energético e métricas computacionais, como utilização de CPU, memória e outros indicadores de desempenho. Essa base teórica apoiou a escolha de atributos energéticos e computacionais no dataset sintético.

Rehan et al. (2025) discutem categorias de desperdício em sistemas distribuídos de processamento de dados, incluindo desperdício de energia, recursos e pegada de carbono. Embora o foco não seja especificamente racks de datacenter, a taxonomia proposta pelos autores contribui para a formulação conceitual deste trabalho, especialmente na definição dos níveis baixo, moderado e alto de desperdício ambiental.

Kumar et al. (2020) abordam aprendizado de máquina eficiente em energia, com foco em sistemas de borda e otimizações voltadas à redução de consumo. Apesar de o contexto ser diferente, o estudo contribui para a discussão de Green AI, reforçando que modelos de aprendizado de máquina também devem ser avaliados sob a perspectiva de eficiência computacional e energética.

Diferentemente desses trabalhos, o presente estudo foca na classificação supervisionada do nível de risco de desperdício ambiental em racks de datacenter voltados a cargas de IA. A contribuição específica está na construção de um dataset sintético controlado, com atributos energéticos, térmicos, computacionais e operacionais, e na comparação experimental de classificadores no Weka para a tarefa multiclasse baixo, moderado e alto.

## 4. Metodologia

A metodologia deste trabalho foi organizada em cinco etapas principais: geração do dataset sintético, teste piloto, pré-processamento, visualização de dados e modelagem com algoritmos de aprendizado de máquina no Weka. Todas as etapas foram documentadas em arquivos específicos do projeto, permitindo rastreabilidade do processo experimental.

Os documentos completos das etapas podem ser consultados nos seguintes arquivos:

| Etapa | Arquivo |
|---|---|
| Pipeline de geração do dataset | [`dataset/01_pipeline_geracao_dataset.md`](../dataset/01_pipeline_geracao_dataset.md) |
| Engenharia de atributos | [`dataset/02_engenharia_atributos.md`](../dataset/02_engenharia_atributos.md) |
| Regras semânticas | [`dataset/03_regras_semanticas.md`](../dataset/03_regras_semanticas.md) |
| Planejamento de anomalias, ruídos e faltantes | [`dataset/04_planejamento_anomalias_inconsistencias.md`](../dataset/04_planejamento_anomalias_inconsistencias.md) |
| Protocolo do mapeamento sistemático | [`mapeamento_sistematico/protocolo.md`](../mapeamento_sistematico/protocolo.md) |
| Teste piloto | [`preprocessamento/01_teste_piloto.md`](../preprocessamento/01_teste_piloto.md) |
| Análise inicial | [`preprocessamento/02_analise_inicial.md`](../preprocessamento/02_analise_inicial.md) |
| Descrição do pré-processamento | [`preprocessamento/03_descricao_etapas.md`](../preprocessamento/03_descricao_etapas.md) |
| Visualização de dados | [`preprocessamento/04_visualizacao_dados.md`](../preprocessamento/04_visualizacao_dados.md) |
| Método de treino e teste | [`treino_teste/01_metodo_treino_teste.md`](../treino_teste/01_metodo_treino_teste.md) |
| Análise dos resultados | [`treino_teste/02_analise_resultados.md`](../treino_teste/02_analise_resultados.md) |

### 4.1 Geração do dataset sintético

O dataset foi gerado com apoio de LLMs, seguindo um pipeline metodológico composto por definição do esquema, escolha dos atributos, definição de regras semânticas, planejamento de imperfeições, construção de prompts, geração de amostras, geração completa e validação estrutural.

A unidade de análise definida foi:

```bash
um rack de datacenter em uma hora de operação
````

A classe-alvo do problema foi:

```bash
environmental_waste_risk_level
```

com três classes:

```bash
baixo
moderado
alto
```

A geração não foi feita de forma aleatória. O dataset foi projetado para conter relações plausíveis entre consumo energético, utilização computacional, temperatura, refrigeração, duração dos jobs, densidade de potência e impacto ambiental. As regras semânticas foram usadas para evitar combinações incoerentes, como consumo incompatível com potência ativa, temperaturas fisicamente impossíveis ou categorias não previstas.

O dataset também foi planejado para conter imperfeições controladas, conforme exigido no trabalho: valores faltantes, ruído, outliers e atributos irrelevantes. Esses elementos foram inseridos intencionalmente para simular problemas comuns em bases operacionais reais e permitir a aplicação justificada de técnicas de pré-processamento.

### 4.2 Engenharia de atributos

Os atributos foram organizados em grupos energéticos, térmicos, ambientais, computacionais, operacionais e administrativos. A seleção buscou representar o comportamento de racks voltados a cargas de IA, incluindo variáveis relacionadas a GPUs, workloads, consumo energético, temperatura e eficiência ambiental.

Entre os principais atributos utilizados estão:

```bash
active_power_w
energy_consumption_kwh
water_usage_effectiveness
carbon_intensity_gco2_kwh
inlet_temperature_c
exhaust_temperature_c
delta_t_c
fan_speed_rpm
gpu_power_w
gpu_utilization_percent
gpu_temperature_c
num_gpus
ai_workload_type
job_duration_hours
job_status
rack_power_density_kw
gpu_sharing_mode
power_cap_w
```

Também foram incluídos atributos administrativos propositalmente irrelevantes:

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

Esses atributos foram mantidos no dataset original para permitir que a etapa de pré-processamento identificasse e removesse variáveis sem relação semântica direta com o risco de desperdício ambiental.

### 4.3 Teste piloto

Antes do pré-processamento, foi realizado um teste piloto no Weka com o arquivo:

```bash
dataset/dataset_original.arff
```

O objetivo foi verificar a integridade estrutural do dataset, analisar distribuições, identificar valores faltantes, observar ruídos e outliers e avaliar relações entre atributos.

O dataset original apresentou:

| Item           |                        Resultado |
| -------------- | -------------------------------: |
| Instâncias     |                              674 |
| Atributos      |                               30 |
| Classe-alvo    | `environmental_waste_risk_level` |
| Tipo da tarefa |                    Classificação |
| Ferramenta     |                             Weka |
| Formato        |                             ARFF |

O teste piloto confirmou que o arquivo abriu corretamente no Weka, a classe-alvo foi reconhecida como nominal e os atributos numéricos e categóricos foram interpretados adequadamente. Também foram confirmados valores faltantes apenas nos atributos planejados, ruídos plausíveis, outliers interpretáveis e ausência de problemas estruturais graves, como linhas quebradas ou categorias inválidas.

Os principais achados do teste piloto foram:

| Achado                                                             | Decisão posterior              |
| ------------------------------------------------------------------ | ------------------------------ |
| Valores faltantes em atributos ambientais, térmicos e operacionais | Tratar no pré-processamento    |
| Atributos administrativos irrelevantes                             | Remover                        |
| `num_gpus` com comportamento discreto                              | Converter para nominal         |
| Escalas numéricas muito diferentes                                 | Normalizar                     |
| Ruído leve e plausível                                             | Manter                         |
| Outliers interpretáveis                                            | Manter inicialmente            |
| Forte relação de `rack_power_density_kw` com a classe              | Monitorar na modelagem         |
| Sobreposição entre `baixo` e `moderado`                            | Avaliar por matriz de confusão |

### 4.4 Pré-processamento

O pré-processamento foi realizado no Weka de forma seletiva e justificada. A intenção não foi eliminar automaticamente todo valor extremo, mas tratar problemas objetivos sem remover padrões importantes para a classificação.

Foram aplicados os seguintes filtros:

| Ordem | Filtro                 | Finalidade                            |
| ----: | ---------------------- | ------------------------------------- |
|     1 | `ReplaceMissingValues` | Tratar valores ausentes               |
|     2 | `Remove`               | Remover atributos irrelevantes        |
|     3 | `NumericToNominal`     | Converter `num_gpus` para nominal     |
|     4 | `RemoveUseless`        | Verificar atributos sem variação útil |
|     5 | `Normalize`            | Normalizar atributos numéricos        |
|     6 | `AttributeSelection`   | Gerar versão complementar reduzida    |

Na versão principal, foram removidos os atributos:

```bash
manufacturer_sku_id
rack_label_color
rack_inventory_zone
```

A versão principal gerada foi:

```bash
dataset/dataset_preprocessamento.arff
```

Também foi criada uma versão complementar com seleção automática de atributos:

```bash
dataset/dataset_preprocessamento_attributeSelection.arff
```

O filtro `AttributeSelection`, configurado com `CfsSubsetEval` e `BestFirst`, selecionou os seguintes atributos:

```bash
water_usage_effectiveness
inlet_temperature_c
gpu_utilization_percent
job_status
rack_power_density_kw
environmental_waste_risk_level
```

Essa versão reduzida foi usada apenas como comparação, pois remove atributos semanticamente relevantes para a interpretação do problema.

### 4.5 Visualização dos dados

Após o pré-processamento, foram realizadas visualizações no Weka com o objetivo de verificar se as transformações aplicadas produziram os efeitos esperados e se os padrões relevantes foram preservados.

As visualizações analisaram:

* estrutura do dataset preprocessado;
* conversão de `num_gpus` para nominal;
* normalização dos atributos numéricos;
* distribuições de atributos relevantes;
* relações entre pares de atributos;
* separação entre classes;
* visualizações de fronteira no BoundaryVisualizer.

As imagens da etapa estão organizadas em:

```bash
imagens/visualizacao/dataset_preprocessado/
imagens/visualizacao/datatset_preprocessado_attrselect/
```

Foram observados padrões importantes, especialmente envolvendo:

```bash
rack_power_density_kw
gpu_utilization_percent
water_usage_effectiveness
inlet_temperature_c
active_power_w
energy_consumption_kwh
job_status
```

As visualizações indicaram que `rack_power_density_kw` e `gpu_utilization_percent` são atributos fortes para a separação das classes. Também foi observado que as classes `baixo` e `moderado` possuem maior sobreposição, enquanto a classe `alto` tende a ser mais distinguível em cenários de alta densidade energética, baixa utilização computacional ou pior eficiência ambiental.

### 4.6 Modelagem e avaliação

A etapa de modelagem foi conduzida no Weka com validação cruzada estratificada de 10 folds:

```bash
Cross-validation
Folds: 10
```

Essa estratégia foi escolhida porque o dataset possui 674 instâncias, três classes, ruído planejado, outliers interpretáveis e desbalanceamento moderado da classe `alto`. A validação cruzada reduz a dependência de uma única divisão treino/teste e permite avaliação mais estável dos classificadores.

Foram avaliados os seguintes algoritmos:

| Algoritmo     | Nome no Weka   | Papel                 |
| ------------- | -------------- | --------------------- |
| ZeroR         | `ZeroR`        | Baseline mínimo       |
| OneR          | `OneR`         | Baseline simples      |
| Naive Bayes   | `NaiveBayes`   | Modelo probabilístico |
| J48           | `J48`          | Árvore de decisão     |
| Random Forest | `RandomForest` | Ensemble de árvores   |
| IBk           | `IBk`          | KNN                   |
| SMO           | `SMO`          | SVM                   |

Também foram realizados ajustes de hiperparâmetros em dois algoritmos:

| Algoritmo     | Parâmetro  | Valores testados |
| ------------- | ---------- | ---------------- |
| Random Forest | `numTrees` | 50, 100, 200     |
| IBk           | `K`        | 1, 3, 5, 7       |

As métricas analisadas foram acurácia, Kappa, F1 ponderado, recall da classe `alto` e matriz de confusão.

---

## 5. Resultados e Discussão

### 5.1 Resultado do teste piloto

O teste piloto indicou que o dataset estava estruturalmente adequado para seguir para o pré-processamento. O arquivo foi carregado corretamente no Weka, com 674 instâncias, 30 atributos e classe-alvo nominal.

Os valores faltantes apareceram apenas nos atributos planejados:

```bash
gpu_temperature_c
fan_speed_rpm
water_usage_effectiveness
carbon_intensity_gco2_kwh
job_status
```

Os ruídos e outliers encontrados foram considerados plausíveis no domínio. Exemplos incluem temperaturas elevadas, alta rotação de fans, jobs longos e alta densidade de potência. Por isso, esses registros não foram removidos automaticamente.

A análise também confirmou que os atributos administrativos `manufacturer_sku_id`, `rack_label_color` e `rack_inventory_zone` não possuíam relação semântica direta com o risco ambiental, justificando sua remoção no pré-processamento.

### 5.2 Resultado do pré-processamento

O pré-processamento corrigiu problemas objetivos sem descaracterizar o dataset. Os valores faltantes foram tratados, os atributos irrelevantes foram removidos, `num_gpus` foi convertido para nominal e os atributos numéricos foram normalizados.

A versão principal manteve maior riqueza informacional, enquanto a versão com `AttributeSelection` reduziu o conjunto para atributos visualmente e estatisticamente fortes. Essa redução foi útil para alguns algoritmos, mas também removeu informações contextuais importantes.

Assim, a versão completa preprocessada foi mantida como base principal:

```bash
dataset/dataset_preprocessamento.arff
```

e a versão reduzida foi usada como comparação:

```bash
dataset/dataset_preprocessamento_attributeSelection.arff
```

### 5.3 Resultado da visualização

As visualizações pós-pré-processamento confirmaram que as transformações aplicadas no Weka produziram os efeitos esperados. Não havia mais valores faltantes, os atributos irrelevantes haviam sido removidos, `num_gpus` foi reconhecido como nominal e os atributos numéricos estavam normalizados.

A análise visual mostrou que `rack_power_density_kw` e `gpu_utilization_percent` foram os atributos com maior força visual para a separação entre classes. A relação entre densidade energética e utilização da GPU foi especialmente importante, pois representa a combinação entre pressão energética e aproveitamento computacional.

Também foi observada sobreposição entre `baixo` e `moderado`, o que indicou uma provável dificuldade de classificação entre essas duas classes. A classe `alto`, por outro lado, apresentou padrões mais evidentes em regiões de maior densidade de potência, menor utilização de GPU ou maior impacto ambiental.

### 5.4 Resultado geral dos classificadores

A tabela a seguir resume os principais resultados da etapa de treino e teste:

| Dataset                                            | Algoritmo    | Configuração   | Acurácia |  Kappa | F1 ponderado | Recall `alto` |
| -------------------------------------------------- | ------------ | -------------- | -------: | -----: | -----------: | ------------: |
| `dataset_preprocessamento.arff`                    | ZeroR        | padrão         | 39,7626% | 0,0000 |        0,226 |         0,000 |
| `dataset_preprocessamento.arff`                    | OneR         | padrão         | 68,3976% | 0,5175 |        0,683 |         0,791 |
| `dataset_preprocessamento.arff`                    | NaiveBayes   | padrão         | 67,6558% | 0,5063 |        0,676 |         0,886 |
| `dataset_preprocessamento.arff`                    | J48          | padrão         | 86,4985% | 0,7928 |        0,865 |         0,930 |
| `dataset_preprocessamento.arff`                    | SMO          | padrão         | 80,8605% | 0,7065 |        0,808 |         0,981 |
| `dataset_preprocessamento.arff`                    | IBk          | K = 1          | 75,5193% | 0,6236 |        0,757 |         0,892 |
| `dataset_preprocessamento.arff`                    | RandomForest | numTrees = 200 | 91,9881% | 0,8769 |        0,920 |         0,975 |
| `dataset_preprocessamento_attributeSelection.arff` | NaiveBayes   | padrão         | 74,7774% | 0,6111 |        0,743 |         0,899 |
| `dataset_preprocessamento_attributeSelection.arff` | J48          | padrão         | 84,4214% | 0,7615 |        0,844 |         0,956 |
| `dataset_preprocessamento_attributeSelection.arff` | SMO          | padrão         | 76,7062% | 0,6403 |        0,761 |         0,911 |
| `dataset_preprocessamento_attributeSelection.arff` | IBk          | K = 3          | 85,0148% | 0,7701 |        0,850 |         0,968 |
| `dataset_preprocessamento_attributeSelection.arff` | RandomForest | numTrees = 200 | 87,0920% | 0,8019 |        0,871 |         0,975 |

Os resultados completos por algoritmo estão disponíveis em:

| Algoritmo     | Arquivo                                                                                         |
| ------------- | ----------------------------------------------------------------------------------------------- |
| ZeroR         | [`treino_teste/algoritmos/01_zeror.md`](../treino_teste/algoritmos/01_zeror.md)                 |
| OneR          | [`treino_teste/algoritmos/02_oner.md`](../treino_teste/algoritmos/02_oner.md)                   |
| IBk / KNN     | [`treino_teste/algoritmos/03_ibk_knn.md`](../treino_teste/algoritmos/03_ibk_knn.md)             |
| Naive Bayes   | [`treino_teste/algoritmos/04_naive_bayes.md`](../treino_teste/algoritmos/04_naive_bayes.md)     |
| SMO / SVM     | [`treino_teste/algoritmos/05_smo_smv.md`](../treino_teste/algoritmos/05_smo_smv.md)             |
| J48           | [`treino_teste/algoritmos/06_j48.md`](../treino_teste/algoritmos/06_j48.md)                     |
| Random Forest | [`treino_teste/algoritmos/07_random_forest.md`](../treino_teste/algoritmos/07_random_forest.md) |

### 5.5 Discussão dos resultados

Os baselines tiveram papel importante na interpretação dos resultados. O `ZeroR` obteve apenas 39,7626% de acurácia, pois classificou todas as instâncias como a classe majoritária `baixo`. Esse resultado representa o desempenho mínimo esperado e mostra que qualquer modelo útil deve superar essa referência.

O `OneR` apresentou 68,3976% de acurácia usando apenas o atributo `gpu_utilization_percent`. Esse resultado confirma que a utilização da GPU é um atributo individualmente forte, mas também mostra que uma única variável não é suficiente para capturar toda a complexidade do problema.

O `NaiveBayes` melhorou com `AttributeSelection`, saindo de 67,6558% para 74,7774% de acurácia. Isso sugere que o algoritmo foi prejudicado pela presença de atributos correlacionados no dataset completo, o que é compatível com sua suposição de independência condicional entre atributos.

O `IBk` também foi bastante beneficiado pela versão com `AttributeSelection`. No dataset completo, seu melhor resultado foi 75,5193% com `K = 1`. Na versão reduzida, o melhor resultado foi 85,0148% com `K = 3`. Isso indica que a seleção de atributos tornou a distância entre instâncias mais informativa, favorecendo um algoritmo baseado em vizinhança.

O `SMO` teve melhor desempenho no dataset completo, com 80,8605% de acurácia e recall de 0,981 para a classe `alto`. Apesar disso, seu equilíbrio geral foi inferior ao do Random Forest e do J48. O modelo foi muito eficiente para identificar casos críticos, mas apresentou maior confusão entre `baixo` e `moderado`.

O `J48` apresentou bom desempenho e alta interpretabilidade. No dataset completo, obteve 86,4985% de acurácia e F1 ponderado de 0,865. A árvore usou `rack_power_density_kw` como uma das divisões principais, confirmando a importância desse atributo observada na visualização. A versão com `AttributeSelection` gerou uma árvore menor, mas com leve perda de desempenho geral.

O melhor resultado geral foi obtido pelo `RandomForest` com `numTrees = 200` no dataset completo:

```bash
Acurácia: 91,9881%
Kappa: 0,8769
F1 ponderado: 0,920
Recall alto: 0,975
```

A matriz de confusão desse modelo foi:

```bash
   a   b   c   <-- classified as
 249  19   0 |   a = baixo
  28 217   3 |   b = moderado
   0   4 154 |   c = alto
```

Esse resultado é relevante porque nenhum registro da classe `alto` foi classificado como `baixo`. Os quatro erros da classe `alto` foram classificados como `moderado`, o que é menos grave do que classificá-los como baixo risco.

### 5.6 Melhor algoritmo e melhor dataset

O melhor algoritmo geral foi:

```bash
RandomForest
```

com:

```bash
numTrees = 200
```

aplicado ao dataset:

```bash
dataset/dataset_preprocessamento.arff
```

Essa escolha é justificada porque o modelo apresentou o melhor equilíbrio entre acurácia, Kappa, F1 ponderado e recall da classe `alto`.

O melhor dataset para desempenho geral foi a versão completa preprocessada. A versão com `AttributeSelection` foi útil para alguns algoritmos, especialmente `IBk` e `NaiveBayes`, mas não superou o melhor resultado geral obtido com o dataset completo.

Portanto, a versão com `AttributeSelection` deve ser interpretada como análise complementar, não como substituta da base principal.

### 5.7 Síntese da discussão

Os resultados confirmam que o risco de desperdício ambiental não depende de um único atributo isolado. Embora `gpu_utilization_percent` e `rack_power_density_kw` sejam atributos fortes, o melhor desempenho foi obtido por um algoritmo capaz de combinar múltiplas variáveis e lidar com relações não lineares.

A classe `alto` foi bem identificada pelos modelos mais fortes, principalmente `RandomForest`, `SMO`, `J48` e `IBk` com `AttributeSelection`. A maior dificuldade esteve na separação entre `baixo` e `moderado`, o que é coerente com a natureza intermediária da classe `moderado`.

Assim, os resultados sustentam a escolha do `RandomForest` com `numTrees = 200` como melhor modelo para a classificação do nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de IA.


## 6. Conclusão

Este trabalho apresentou a construção, o pré-processamento, a visualização e a avaliação de algoritmos de aprendizado de máquina para classificar o nível de risco de desperdício ambiental em racks de datacenters voltados a cargas de inteligência artificial.

A proposta partiu da definição de um dataset sintético em que cada instância representa um rack em uma hora de operação. A base incluiu atributos energéticos, térmicos, ambientais, computacionais e operacionais, como potência ativa, consumo energético, utilização de GPU, temperatura, método de refrigeração, duração de jobs e densidade de potência do rack. Também foram inseridos valores faltantes, ruídos, outliers interpretáveis e atributos irrelevantes, permitindo realizar uma etapa de pré-processamento coerente com os requisitos do trabalho.

O teste piloto indicou que o dataset estava estruturalmente válido para uso no Weka, com 674 instâncias, 30 atributos e classe-alvo nominal. A análise inicial também mostrou que os valores faltantes estavam restritos aos atributos planejados, que os outliers eram plausíveis no domínio e que os atributos administrativos não possuíam relação semântica direta com a classe-alvo. Com base nisso, o pré-processamento aplicou filtros como `ReplaceMissingValues`, `Remove`, `NumericToNominal`, `RemoveUseless`, `Normalize` e `AttributeSelection`.

A etapa de visualização confirmou que atributos como `rack_power_density_kw`, `gpu_utilization_percent`, `water_usage_effectiveness`, `inlet_temperature_c` e `job_status` apresentavam relação relevante com a classe-alvo. Também foi observado que as classes `baixo` e `moderado` possuíam maior sobreposição, enquanto a classe `alto` apresentava separação mais evidente em situações de maior densidade energética, menor utilização computacional ou pior eficiência ambiental.

Na etapa de modelagem, foram avaliados os algoritmos `ZeroR`, `OneR`, `NaiveBayes`, `J48`, `RandomForest`, `IBk` e `SMO`, usando validação cruzada com 10 folds. Também foram testadas duas versões do dataset: a versão preprocessada completa e a versão reduzida com `AttributeSelection`.

Os resultados mostraram que os modelos simples cumpriram bem o papel de baseline. O `ZeroR` obteve apenas 39,7626% de acurácia, enquanto o `OneR` alcançou 68,3976% usando apenas `gpu_utilization_percent`, confirmando a importância desse atributo. O `NaiveBayes` e o `IBk` foram beneficiados pela versão com `AttributeSelection`, indicando sensibilidade à presença de atributos redundantes ou correlacionados.

O melhor desempenho geral foi obtido pelo `RandomForest` aplicado ao dataset preprocessado completo, com `numTrees = 200`. Esse modelo alcançou:

```bash
Acurácia: 91,9881%
Kappa: 0,8769
F1 ponderado: 0,920
Recall alto: 0,975
```

Além disso, a matriz de confusão mostrou que nenhum registro da classe `alto` foi classificado como `baixo`, o que é relevante para o problema, pois evita o erro mais crítico: considerar como baixo risco um caso de alto desperdício ambiental.

Assim, conclui-se que o `RandomForest` com 200 árvores foi o algoritmo mais adequado para a tarefa proposta, pois apresentou o melhor equilíbrio entre desempenho geral, identificação da classe crítica e robustez frente aos atributos do dataset. A versão completa preprocessada foi mais adequada do que a versão com `AttributeSelection`, pois preservou informações úteis para modelos capazes de lidar com múltiplas variáveis e relações não lineares.

Como limitação, destaca-se que o dataset utilizado é sintético. Embora tenha sido construído com regras semânticas, atributos fundamentados e imperfeições controladas, ele não substitui uma base real coletada em datacenters. Portanto, os resultados devem ser interpretados como uma avaliação experimental e metodológica, não como validação definitiva em ambiente operacional real.

Como trabalhos futuros, recomenda-se:

* validar a abordagem com dados reais de telemetria de datacenters;
* ampliar o tamanho do dataset;
* testar novas estratégias de balanceamento entre classes;
* comparar outros métodos de seleção de atributos;
* avaliar modelos com ajuste mais amplo de hiperparâmetros;
* investigar explicabilidade dos modelos, especialmente para interpretar decisões do `RandomForest`;
* explorar métricas ambientais derivadas, como emissão estimada de carbono e energia desperdiçada por workload.

De modo geral, o trabalho demonstrou que técnicas de aprendizado de máquina podem apoiar a classificação do risco de desperdício ambiental em racks de datacenter, especialmente quando combinadas com um pipeline estruturado de geração, validação, pré-processamento, visualização e avaliação crítica dos modelos.

---

## Referências

CHINNICI, A.; AHMADZADA, E.; KOR, A.-L.; DE CHIARA, D.; DOMÍNGUEZ-DÍAZ, A.; DE MARCOS ORTEGA, L.; CHINNICI, M. Towards Sustainability and Energy Efficiency Using Data Analytics for HPC Data Center. *Electronics*, v. 13, n. 17, p. 3542, 2024. Disponível em: [https://doi.org/10.3390/electronics13173542](https://doi.org/10.3390/electronics13173542).

HALL, M.; FRANK, E.; HOLMES, G.; PFAHRINGER, B.; REUTEMANN, P.; WITTEN, I. H. The WEKA Data Mining Software: An Update. *SIGKDD Explorations*, v. 11, n. 1, p. 10–18, 2009. Disponível em: [https://doi.org/10.1145/1656274.1656278](https://doi.org/10.1145/1656274.1656278).

ISMAIL, L.; MATERWALA, H. Computing Server Power Modeling in a Data Center: Survey, Taxonomy, and Performance Evaluation. *ACM Computing Surveys*, v. 53, n. 3, Article 58, p. 1–34, 2020. Disponível em: [https://doi.org/10.1145/3390605](https://doi.org/10.1145/3390605).

KHAN, W.; DE CHIARA, D.; KOR, A.-L.; CHINNICI, M. Advanced data analytics modeling for evidence-based data center energy management. *Physica A: Statistical Mechanics and its Applications*, v. 624, p. 128966, 2023. Disponível em: [https://doi.org/10.1016/j.physa.2023.128966](https://doi.org/10.1016/j.physa.2023.128966).

KUMAR, M.; ZHANG, X.; LIU, L.; WANG, Y.; SHI, W. Energy-Efficient Machine Learning on the Edges. In: *IEEE International Parallel and Distributed Processing Symposium Workshops (IPDPSW)*, 2020. Proceedings [...]. IEEE, 2020. p. 912–921. Disponível em: [https://doi.org/10.1109/IPDPSW50202.2020.00153](https://doi.org/10.1109/IPDPSW50202.2020.00153).

REHAN, A.; FATIMA, N.; NAZIR, S. Waste Generated in Distributed Data Processing Systems: Strategies and Future Directions. *Pakistan Journal of Scientific Research*, v. 4, n. 2 Suppl., p. 82–91, 2025. Disponível em: [https://doi.org/10.57041/78vyv598](https://doi.org/10.57041/78vyv598).
