# Projeto 1: Análise de Redes Sociais com Métricas de Grafos

- **Universidade de Brasília (UnB)**
- **Departamento de Ciência da Computação (CIC)**
- **Disciplina:** Teoria e Aplicação de Grafos (TAG) — Turma 01, 2026/2
- **Docente:** Prof. Dr. Díbio Leandro Borges

---

## Integrantes do Grupo

| Nome | Matrícula |
| :--- | :---: |
| Matheus Duarte da Silva | 211062277 |
| Suyanne Sara Miranda Silva | 222006445 |

---

## Visão Geral

Este projeto tem como objetivo analisar a estrutura de uma rede social real por meio de conceitos fundamentais da **Teoria dos Grafos**.
Utilizando o conjunto de dados público de egonetworks do Facebook disponibilizado pelo [Stanford Large Network Dataset Collection (SNAP)](https://snap.stanford.edu/data/egonets-Facebook.html), o estudo avalia influência, prestígio, capacidade de disseminação de informações e formação de comunidades usuários.

O projeto atende as seguintes especificações:

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
  - **Propriedades da Amostra:** 404 nós, 824 arestas, 99 componentes conexos, 105 comunidades de Louvain e 71 nós isolados (grau 0), refletindo a quebra de conectividade típica de amostragem uniforme em redes livres de escala (*scale-free*).

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
   - Painel unificado com abas navegáveis disponível em [`metrics/html/graph_visualizer.html`].
   - Permite alternar entre abas interativas com abertura instantânea, zoom de 60 FPS, drag-and-drop e tooltips detalhados:
     - Comunidades de Louvain (`louvain_communities.html`)
     - Grau de Centralidade (`degree.html`)
     - Centralidade de Intermediação (`betweenness.html`)
     - Centralidade de Proximidade (`closeness.html`)
     - Centralidade de Autovetor (`eigenvector.html`)
     - Rede Base Completa (`generated_graph.html`)
3. **Exportação para o Gephi (`.gexf`):**
    - Arquivo [`metrics/graph_metrics.gexf`] com todas as 5 métricas gravadas como atributos de nós, pronto para layout espacial (ForceAtlas2, Fruchterman Reingold) e renderização profissional.

---

## Instalação e Execução

### Pré-requisitos

- Sistema operacional Linux, macOS ou Windows (com WSL).
- Python 3.14 ou superior.
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
xdg-open metrics/html/graph_visualizer.html

# No macOS:
open metrics/html/graph_visualizer.html

# Ou simplesmente dê duplo-clique no arquivo através do explorador de arquivos.
```

---

## Resumo dos Principais Resultados

A tabela abaixo sintetiza os principais nós da amostra analisada e suas respectivas lideranças:

| Métrica | Top 1 | Top 2 | Top 3 | Papel Estrutural na Rede |
| :--- | :---: | :---: | :---: | :--- |
| **Grau** | **107** (`0.2804`) | **2328** (`0.0571`) | **2511** (`0.0471`) | **Super-Hub Local:** Maior número de conexões diretas de amizade na amostra. |
| **Intermediação** | **107** (`0.1170`) | **1666** (`0.0444`) | **3111** (`0.0422`) | **Gatekeepers / Pontes:** Interligam grupos distintos; sua remoção causa fragmentação. |
| **Proximidade** | **107** (`0.2141`) | **1666** (`0.1608`) | **1450** (`0.1482`) | **Difusores Eficientes:** Menor distância média aos outros membros; propagação rápida. |
| **Autovetor** | **107** (`0.5657`) | **1622** (`0.1860`) | **1377** (`0.1781`) | **Prestígio Social:** Conectados diretamente a nós que também são altamente conectados. |

### Principais Insights

- **Presença de um *Ego-Node* Central (Nó `107`):** O nó `107` (um dos principais *ego-centers* do dataset original do Facebook) foi capturado na amostragem, conectando-se diretamente a ~28% da amostra e liderando nas quatro métricas.
- **Divergência entre Grau e Intermediação (Nós `1666` e `3111`):** Embora não estejam no Top 5 de Grau, os nós `1666` e `3111` assumem o 2º e 3º lugar em Intermediação. Isso demonstra que atuam como **pontes estratégicas (*gatekeepers*)** entre comunidades distintas e o componente principal.
- **Impacto da Amostragem:** A amostragem uniforme de ~10% de uma rede livre de escala gerou 99 componentes conexos e 71 nós isolados (grau 0), influenciando diretamente a distribuição de escores de proximidade e autovetor fora do componente gigante.

---

## Tecnologias Utilizadas

- **Linguagem:** Python 3.14+
- **Manipulação de Grafos:** [NetworkX](https://networkx.org/)
- **Análise de Dados:** [Pandas](https://pandas.pydata.org/)
- **Visualização Estática:** [Matplotlib](https://matplotlib.org/)
- **Visualização Interativa:** [Pyvis](https://pyvis.readthedocs.io/)
- **Visualização Externa:** [Gephi](https://gephi.org/)
- **Ambiente de Desenvolvimento:** [Jupyter Notebook](https://jupyter.org/)
