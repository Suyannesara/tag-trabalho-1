# Projeto 1: Análise de Redes Sociais com Métricas de Grafos

**Universidade de Brasília (UnB)**
**Departamento de Ciência da Computação (CIC)**
**Disciplina:** Teoria e Aplicação de Grafos (TAG) — Turma 01, 2026/2
**Docente:** Prof. Dr. Díbio Leandro Borges

---

## Integrantes do Grupo

| Nome | Matrícula |
| :--- | :---: |
| Matheus Duarte da Silva | 211062277 |
| Suyanne Sara Miranda Silva | 222006445 |

---

## Sumário

- [Projeto 1: Análise de Redes Sociais com Métricas de Grafos](#projeto-1-análise-de-redes-sociais-com-métricas-de-grafos)
  - [Integrantes do Grupo](#integrantes-do-grupo)
  - [Sumário](#sumário)
  - [Visão Geral](#visão-geral)
  - [Base de Dados e Amostragem](#base-de-dados-e-amostragem)
  - [Métricas e Algoritmos](#métricas-e-algoritmos)
  - [Visualizações e Dashboard Interativo](#visualizações-e-dashboard-interativo)
  - [Estrutura do Repositório](#estrutura-do-repositório)
  - [Instalação e Execução](#instalação-e-execução)
    - [Pré-requisitos](#pré-requisitos)
    - [1. Clonar o repositório](#1-clonar-o-repositório)
    - [2. Criar e ativar o ambiente virtual](#2-criar-e-ativar-o-ambiente-virtual)
    - [3. Instalar as dependências](#3-instalar-as-dependências)
    - [4. Executar o Jupyter Notebook](#4-executar-o-jupyter-notebook)
    - [5. Abrir o Dashboard Interativo](#5-abrir-o-dashboard-interativo)
  - [Resumo dos Principais Resultados](#resumo-dos-principais-resultados)
    - [Principais Insights](#principais-insights)
  - [Tecnologias Utilizadas](#tecnologias-utilizadas)

---

## Visão Geral

Este projeto tem como objetivo analisar a estrutura de uma rede social real por meio de conceitos fundamentais da **Teoria dos Grafos**. Utilizando o conjunto de dados público de egonetworks do Facebook disponibilizado pelo [Stanford Large Network Dataset Collection (SNAP)](https://snap.stanford.edu/data/egonets-Facebook.html), o estudo avalia influência, prestígio, capacidade de disseminação de informações e formação de comunidades entre usuários.

O projeto atende a todas as diretrizes especificadas pelo professor:

1. **Coleta de dados:** Rede completa de 4.039 nós e extração de uma amostra de 404 nós de forma pseudoaleatória e reproduzível.
2. **Construção do grafo:** Representação por subgrafo induzido utilizando NetworkX.
3. **Cálculo de métricas:**
   - Detecção de comunidades pelo **Algoritmo de Louvain**;
   - Quatro medidas de centralidade: **Grau**, **Intermediação** (*Betweenness*), **Proximidade** (*Closeness*) e **Autovetor** (*Eigenvector*).
4. **Visualização explícita:**
   - Destaque com rótulos e valores dos nós de maiores métricas;
   - Mapas em cores das comunidades detectadas;
   - Exportação para o **Gephi** (`.gexf`) e gráficos interativos no navegador (**Pyvis**).
5. **Relatório e Análise:** Discussão aprofundada dos nós de maior importância e do impacto da amostragem na topologia da rede.

---

## Base de Dados e Amostragem

- **Fonte:** SNAP Stanford — [Egonets Facebook](https://snap.stanford.edu/data/egonets-Facebook.html) (`facebook_combined.txt.gz`).
- **Grafo Original:** 4.039 nós (usuários anonimizados) e 88.234 arestas (relações de amizade).
- **Amostragem:**
  - **Tamanho da amostra:** 404 nós ($N = 404$, aproximadamente 10% da rede total).
  - **Semente pseudoaleatória (Seed):** `17919131` (garante unicidade da amostra da equipe e reprodutibilidade científica).
  - **Método:** Subgrafo induzido ($G[V']$), mantendo todos os 404 nós sorteados e todas as arestas existentes entre eles.
  - **Propriedades da Amostra:** 404 nós, 1.075 arestas, múltiplos componentes conexos e 97 nós isolados (grau 0), refletindo a quebra de conectividade típica de amostragem uniforme em redes livres de escala (*scale-free*).

---

## Métricas e Algoritmos

1. **Detecção de Comunidades (Louvain):**
   - Otimiza a modularidade para agrupar nós que possuem maior densidade de arestas entre si do que com o restante da rede, identificando grupos sociais coesos.
2. **Grau de Centralidade (*Degree Centrality*):**
   - Mede a quantidade de conexões diretas de um nó normalizada por $N - 1$. Identifica a **popularidade local imediata** (hubs da rede).
3. **Centralidade de Intermediação (*Betweenness Centrality*):**
   - Quantifica a fração de caminhos mínimos entre todos os pares de nós que atravessam determinado vértice. Identifica **pontes e intermediadores de informação (*gatekeepers*)**, essenciais para a coesão entre clusters distintos.
4. **Centralidade de Proximidade (*Closeness Centrality*):**
   - Inverso da soma das distâncias mínimas do nó aos demais nós alcançáveis. Indica quais indivíduos têm o maior **potencial de espalhar informações com menor latência/número de saltos**.
5. **Centralidade de Autovetor (*Eigenvector Centrality*):**
   - Atribui importância a um nó proporcionalmente à importância dos seus vizinhos. Identifica o **prestígio e influência sistêmica** dentro dos núcleos mais densos da rede.

---

## Visualizações e Dashboard Interativo

O projeto oferece três formatos complementares de visualização:

1. **Estática (NetworkX + Matplotlib):**
   - Gráficos integrados no notebook destacando os 10 maiores nós de cada métrica em vermelho com rótulos `[ID: Valor]`.
   - Mapa de coloração das comunidades de Louvain com colormap categórico (`tab20`).
2. **Dashboard Interativo em HTML (Pyvis):**
   - Painel unificado com abas navegáveis disponível em [`metrics/html/analise_grafo.html`].
   - Permite alternar entre abas interativas com zoom, drag-and-drop e tooltips detalhados:
     - Comunidades de Louvain (`comunidades_louvain.html`)
     - Grau de Centralidade (`grau.html`)
     - Centralidade de Intermediação (`intermediacao.html`)
     - Centralidade de Proximidade (`proximidade.html`)
     - Centralidade de Autovetor (`autovetor.html`)
     - Rede Base Completa (`generated_graph.html`)
3. **Exportação para o Gephi (`.gexf`):**
    - Arquivo [`metrics/graph_metrics.gexf`] com todas as 5 métricas gravadas como atributos de nós, pronto para layout espacial (ForceAtlas2, Fruchterman Reingold) e renderização profissional.

---

## Estrutura do Repositório

```plaintext
tag-trabalho-1/
├── README.md                          # Documentação completa do projeto
├── requirements.txt                   # Dependências Python do projeto
├── readme-egonets.txt                  # Metadados oficiais do dataset SNAP
│
├── pdf/
│   └── enunciado.pdf                  # Diretrizes e requisitos oficiais do trabalho
│
├── data/
│   ├── facebook_combined.txt.gz       # Dataset bruto compactado da rede SNAP (4039 nós)
│   └── facebook.tar.gz                # Arquivo complementar com egonets
│
├── src/
│   └── facebook_egonet.ipynb          # Notebook principal com todo o pipeline executável
│
└── metrics/
    ├── graph_metrics.gexf             # Grafo com atributos exportado para o Gephi
    ├── graph_metrics.csv              # Tabela consolidada com as métricas calculadas por nó
    └── html/                          # Visualizações interativas standalone e dashboard
        ├── analise_grafo.html         # Dashboard unificado com sistema de abas
        ├── comunidades_louvain.html   # Visualização interativa das comunidades
        ├── grau.html                  # Visualização interativa do Grau de Centralidade
        ├── intermediacao.html         # Visualização interativa de Intermediação
        ├── proximidade.html           # Visualização interativa de Proximidade
        ├── autovetor.html             # Visualização interativa de Autovetor
        └── generated_graph.html       # Visualização interativa da topologia base
```

---

## Instalação e Execução

### Pré-requisitos

- Sistema operacional Linux, macOS ou Windows (com WSL).
- Python 3.10 ou superior.
- Git instalado.

### 1. Clonar o repositório

```bash
git clone https://github.com/smmstakes/projeto-grafos.git
cd tag-trabalho-1
```

### 2. Criar e ativar o ambiente virtual

```bash
python3 -m venv .venv

# No Linux / macOS:
source .venv/bin/activate

# No Windows (PowerShell):
# .venv\Scripts\Activate.ps1
```

### 3. Instalar as dependências

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Executar o Jupyter Notebook

```bash
jupyter notebook src/facebook_egonet.ipynb
```

Execute as células sequencialmente (`Kernel -> Restart & Run All`). Todos os dados, gráficos estáticos, arquivos CSV, arquivos GEXF e páginas HTML serão gerados automaticamente.

### 5. Abrir o Dashboard Interativo

Para navegar pelas visualizações interativas sem necessidade de servidor web, basta abrir o arquivo no seu navegador preferido:

```bash
# No Linux:
xdg-open metrics/html/analise_grafo.html

# No macOS:
open metrics/html/analise_grafo.html

# Ou simplesmente dê duplo-clique no arquivo através do explorador de arquivos.
```

---

## Resumo dos Principais Resultados

A tabela abaixo sintetiza os principais nós da amostra analisada e suas respectivas lideranças:

| Métrica | Nó Líder | Valor | Papel Estrutural na Rede |
| :--- | :---: | :---: | :--- |
| **Grau** | **1447** | `0.0546` | **Hub Local:** Maior número de conexões imediatas de amizade na amostra. |
| **Intermediação** | **1537** | `0.0149` | **Gatekeeper / Ponte:** Interliga grupos distintos; sua remoção causaria fragmentação. |
| **Proximidade** | **1029** | `0.0676` | **Difusor Eficiente:** Menor distância média aos outros membros; ideal para propagação rápida. |
| **Autovetor** | **1447** | `0.2684` | **Prestígio Social:** Conectado diretamente a nós que também são altamente conectados. |

### Principais Insights

- **Divergência entre Grau e Intermediação:** O nó `1537` é o líder absoluto em intermediação, mas não ocupa o primeiro lugar em grau. Isso demonstra que **popularidade direta não implica necessariamente em controle do fluxo de informações**.
- **Impacto da Amostragem:** A amostragem de ~10% de uma rede livre de escala provocou a presença de 97 nós isolados e dezenas de pequenos componentes desconectados do componente gigante, influenciando diretamente a distribuição de escores de proximidade e autovetor.

---

## Tecnologias Utilizadas

- **Linguagem:** Python 3.10+
- **Manipulação de Grafos:** [NetworkX](https://networkx.org/)
- **Análise de Dados:** [Pandas](https://pandas.pydata.org/)
- **Visualização Estática:** [Matplotlib](https://matplotlib.org/)
- **Visualização Interativa:** [Pyvis](https://pyvis.readthedocs.io/)
- **Visualização Externa:** [Gephi](https://gephi.org/) (compatível via `.gexf`)
- **Ambiente de Desenvolvimento:** [Jupyter Notebook](https://jupyter.org/)
