# Classificação do Nível de Desperdício Ambiental em Racks de Datacenter

Projeto desenvolvido para a disciplina **Inteligência Artificial**, ministrada pelo Prof. Dr. **Andrey Rodrigues**.

## Integrantes

- [Gregory Ozaki](https://github.com/gregoryozaki)
- [Ana Paula Xavier](https://github.com/ana-xavier19)
- [Calil Lima](https://github.com/Kallicco)
- [Gabriel Batista](https://github.com/Gaabrhiel)
- [Tiago Santos](https://github.com/TiagoSE)
- [Wamberson Pacheco](https://github.com/Dev-WambersonPacheco)

---

## Sumário

1. [Descrição do Projeto](#descrição-do-projeto)
2. [Definição do Problema](#definição-do-problema)
3. [Estrutura do Repositório](#estrutura-do-repositório)
4. [Fases do Trabalho](#fases-do-trabalho)
5. [Relatório Final](#relatório-final)
6. [Tecnologias Utilizadas](#tecnologias-utilizadas)
7. [Licença](#licença)

---

## Descrição do Projeto

Este projeto tem como objetivo construir, pré-processar e analisar um dataset sintético voltado à **classificação do nível de desperdício ambiental em racks de datacenter**.

O dataset representa situações operacionais de racks em datacenters, considerando atributos energéticos, térmicos, computacionais, operacionais e ambientais. A proposta é simular cenários em que racks possam apresentar baixo, moderado ou alto nível de desperdício ambiental.

A construção do dataset é feita com apoio de **Modelos de Linguagem de Grande Escala (LLMs)**, seguindo um processo documentado, rastreável e fundamentado por literatura. Após a geração, o dataset será analisado, pré-processado e utilizado em experimentos de classificação no **Weka**.

---

## Definição do Problema

| Item | Definição |
|---|---|
| **Tema** | Classificação do nível de desperdício ambiental em racks de datacenter |
| **Tarefa de ML** | Classificação |
| **Unidade da instância** | Um rack em uma hora de operação |
| **Entrada** | Atributos energéticos, térmicos, computacionais, operacionais e ambientais |
| **Saída esperada** | Classe de desperdício ambiental |
| **Classes** | Baixo, moderado e alto |

O problema consiste em classificar o nível de desperdício ambiental de um rack de datacenter a partir de variáveis relacionadas ao seu funcionamento. Cada instância do dataset representa o comportamento de um rack durante uma hora de operação.

---

## Estrutura do Repositório

```bash
.
├── dataset
│   ├── dataset_original.arff
│   └── dataset_preprocessado.arff
├── imagens
│   └── pipeline_geracao.png
├── LICENSE
├── msl
├── preprocessamento
│   ├── analise_inicial.md
│   └── descricao_etapas.md
├── prompts
│   └── prompts_utilizados.txt
├── README.md
└── relatorio
    └── relatorio_final.md

```

---

## Fases do Trabalho

| Fase | Descrição |
|---|---|
| 1. Definição do Problema |  Formulação do problema de classificação, definição das entradas, saída esperada e classes do dataset. |
| 2. Fundamentação teórica e MSL | Realização de mapeamentos sistemáticos da literatura para fundamentar o problema, os atributos do dataset e o pipeline de geração sintética com LLM. |
| 3. Geração do dataset sintético  |  Construção do dataset com apoio de LLMs, incluindo prompts documentados, controle semântico, valores faltantes, ruído, outliers e atributo irrelevante. |
|  4. Teste Piloto  |  Investigação preliminar do dataset original antes do pré-processamento, observando distribuições, inconsistências, valores faltantes, ruído e outliers. |
|  5. Pré-processamento  | Aplicação justificada de técnicas de limpeza, tratamento de valores faltantes, análise de atributos, tratamento de outliers e filtros no Weka. |
| 6. Visualização dos dados |  Exploração visual do dataset no Weka para observar distribuições, relações entre atributos e separação entre classes. |
| 7. Treinamento e avaliação  |  Execução de algoritmos de classificação no Weka, comparação de métricas e análise crítica dos resultados obtidos. |


---

## Relatório Final

O relatório final estará disponível em:

- [relatorio/relatorio_final.md](https://github.com/gregoryozaki/machine-learning-with-weka/blob/main/relatorio/relatorio_final.md)

Ele reunirá a definição do problema, a fundamentação teórica, o processo de geração do dataset, as decisões de pré-processamento, os experimentos no Weka e a análise dos resultados.

---

## Tecnologias Utilizadas

- Weka — ferramenta principal para pré-processamento, visualização, treinamento e avaliação dos modelos.
- LLMs — apoio à geração sintética do dataset.
- SciSpace — apoio aos mapeamentos sistemáticos da literatura.
- NotebookLM — apoio à leitura, triagem e extração de dados dos artigos.
- GitHub — versionamento, organização e documentação do projeto.

---

## Licença

Este projeto está licenciado conforme os termos definidos no arquivo [LICENSE](https://github.com/gregoryozaki/machine-learning-with-weka/blob/main/LICENSE)
