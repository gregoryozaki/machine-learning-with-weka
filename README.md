# Classificação do Nível de Desperdício Ambiental em Racks de Datacenter

Projeto desenvolvido para a disciplina **Inteligência Artificial**, ministrada pelo Prof.Dr. **Andrey Rodrigues**.

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
5. [Estrutura do Repositório](#estrutura-do-repositório)
6. [Fases do Trabalho](#fases-do-trabalho)
7. [Geração do Dataset Sintético](#geração-do-dataset-sintético)
8. [Mapeamentos Sistemáticos da Literatura](#mapeamentos-sistemáticos-da-literatura)
9. [Pré-processamento](#pré-processamento)
10. [Relatório Final](#relatório-final)
11. [Licença](#licença)

---

## Descrição do Projeto

Este projeto tem como objetivo construir e analisar um dataset sintético voltado à **classificação do nível de desperdício ambiental em racks de datacenter**.

O dataset foi planejado para representar situações operacionais de racks em datacenters, considerando atributos energéticos, térmicos, computacionais, operacionais e ambientais. A proposta busca simular cenários em que racks podem operar com baixo, moderado ou alto nível de desperdício ambiental.

A construção do dataset é feita com apoio de **Modelos de Linguagem de Grande Escala (LLMs)**, seguindo um processo documentado, rastreável e baseado em fundamentação bibliográfica.

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

