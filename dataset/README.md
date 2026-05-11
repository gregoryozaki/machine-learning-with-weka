# Documentação da Geração do Dataset Sintético

## Classificação do Nível de Risco de Desperdício Ambiental em Racks de Datacenters Voltados a Cargas de IA

---

## 1. Objetivo do Dataset

O objetivo deste dataset é representar, de forma sintética e controlada, situações operacionais de racks de datacenter para permitir a aplicação de técnicas de aprendizado de máquina na tarefa de **classificação do nível de desperdício ambiental**.

Cada instância do dataset representa o comportamento de **um rack de datacenter em uma hora de operação**.

A classe-alvo do dataset é o nível de desperdício ambiental, dividido em três categorias:

- **baixo**
- **moderado**
- **alto**

O dataset será gerado com apoio de Modelos de Linguagem de Grande Escala (LLMs), seguindo um processo documentado, rastreável e baseado em fundamentação obtida por meio dos Mapeamentos Sistemáticos da Literatura.

---

## 2. Requisitos do Trabalho

Segundo as especificações do trabalho, o dataset deverá atender aos seguintes requisitos mínimos:

| Requisito | Como será atendido |
|---|---|
| Pelo menos 500 instâncias | O dataset será gerado com quantidade igual ou superior a 500 registros. |
| Pelo menos 5 atributos | O dataset conterá atributos energéticos, térmicos, computacionais, operacionais e ambientais. |
| Pelo menos 1 atributo irrelevante | Será incluído um atributo sem relação direta com a classe-alvo. |
| Presença de valores faltantes | Serão inseridos valores ausentes de forma intencional e documentada. |
| Presença de ruído | Serão inseridas pequenas variações ou inconsistências controladas. |
| Presença de outliers | Serão criadas instâncias discrepantes em relação à distribuição principal. |
| Uso de LLM | O dataset será gerado com apoio de Modelo de Linguagem de Grande Escala. |
| Prompts documentados | Todos os prompts utilizados serão registrados e justificados. |
| Rastreabilidade | O processo de geração será documentado do planejamento até o dataset final. |
| Figura do pipeline | Será criada uma imagem representando o fluxo de geração dos dados. |
| Formato compatível com Weka | O dataset final será salvo em `.arff`. |

---

## 3. Estrutura Geral do Dataset

### 3.1. Unidade de Análise

Cada linha do dataset representa:

> Um rack de datacenter em uma hora de operação.

Essa unidade permite relacionar variáveis de consumo, refrigeração, carga computacional e impacto ambiental em uma janela temporal simples e interpretável.

---

### 3.2. Classe-Alvo

A variável-alvo será:

```bash
environmental_waste_risk_level
```

Com os seguintes valores:

| Classe       | Descrição                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------- |
| **baixo**    | Operação proporcional entre uso computacional, consumo energético e refrigeração.           |
| **moderado** | Presença de sinais intermediários de ineficiência ou desperdício.                           |
| **alto**     | Consumo, refrigeração ou impacto ambiental desproporcional em relação à utilização do rack. |

---

## Artefatos Finais da Etapa de Geração

Ao final da geração, devem existir os seguintes artefatos:

| Artefato                                  | Finalidade                                      |
| ----------------------------------------- | ----------------------------------------------- |
| `dataset/documentacao_geracao_dataset.md` | Documento central do processo de geração        |
| `dataset/dataset_original.csv`            | Dataset original em CSV                         |
| `dataset/dataset_original.arff`           | Dataset original em formato compatível com Weka |
| `prompts/prompts_utilizados.txt`          | Registro dos prompts usados                     |
| `imagens/pipeline_geracao.png`            | Figura do fluxo de geração                      |

