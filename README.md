# 📚 Discovery Bottom-Up com Dados: Análise Goodreads

## 🎯 Sobre o Projeto

Este projeto demonstra um **discovery bottom-up orientado a dados** usando uma base de livros do Goodreads. O objetivo é explorar como dados podem revelar oportunidades de produto e soluções baseadas em Machine Learning.

### 🔍 O que é Discovery Bottom-Up?

Discovery bottom-up é um processo de descoberta que começa com ideias, soluções ou dados disponíveis e busca conectá-los à estratégia e objetivos de negócio. Diferente do discovery top-down (que parte dos objetivos), este método explora possibilidades a partir do que já existe.

---

## 📊 Dataset

**Fonte:** Goodreads (Kaggle)  
**Arquivo:** `book1-100k.csv`  
**Registros:** 58.292 livros  
**Período:** Dados coletados até 2020

### Estrutura dos Dados

| Coluna | Descrição | Tipo | % Nulos |
|--------|-----------|------|---------|
| `id` | Identificador único do livro | int | 0% |
| `name` | Título do livro | str | 0% |
| `authors` | Autor(es) do livro | str | 0% |
| `publishyear` | Ano de publicação | int | ~0% |
| `publisher` | Editora | str | 0.8% |
| `language` | Idioma | str | 65% |
| `isbn` | ISBN (International Standard Book Number) | str | 0.9% |
| `pagesnumber` | Número de páginas | int | 58% |
| `rating` | Avaliação média | float | 0% |
| `countsofreview` | Quantidade de reviews | int | 0% |
| `ratingdist1-5` | Distribuição de notas 1-5 | str | 0% |
| `ratingdisttotal` | Total de avaliações | str | 0% |

---

## 🛠️ Análises Realizadas

### 1️⃣ **Análise Exploratória de Dados (EDA)**
- Avaliação de qualidade dos dados
- Identificação de valores nulos e outliers
- Análise de distribuições estatísticas

### 2️⃣ **Validação e Limpeza**
#### ✅ Anos de Publicação
- Filtro de anos inválidos (> 2020)
- Detecção de outliers temporais

#### ✅ Validação de ISBN
- Limpeza e normalização de ISBNs
- Validação de ISBN-10 e ISBN-13
- Conversão de ISBN-10 para ISBN-13
- Detecção de duplicatas

#### ✅ Detecção de Duplicatas de Títulos
- Normalização de texto (remoção de acentos, case, pontuação)
- Vetorização TF-IDF
- Similaridade cosseno
- Clustering por Union-Find
- **Resultado:** 2.060 clusters de duplicatas encontrados

### 3️⃣ **Canonização de Dados**

> **Canonização** é o processo de identificar registros diferentes que representam a mesma entidade real.

**Métodos utilizados:**
- **Chaves fortes:** ISBN (194 duplicatas)
- **TF-IDF + Similaridade:** Títulos (3.074 duplicatas - 5%)
- **Embeddings:** Agrupamento por características

### 4️⃣ **Dataset Limpo**
Criação de dataset processado removendo:
- Anos inválidos (> 2020)
- Duplicatas de ISBN
- Duplicatas de título

### 5️⃣ **Clustering de Livros Similares (POC)**
**Objetivo:** Agrupar livros similares para recomendação

**Features utilizadas:**
- Título (TF-IDF, peso 3.0)
- Autor (TF-IDF, peso 2.0)
- Língua (One-Hot Encoding)
- Ano (normalizado, peso 0.5)
- Publisher (TF-IDF, peso 0.5)

**Algoritmos testados:**
- **K-Means:** Clustering baseado em centróides
- **DBSCAN:** Clustering por densidade
- **Hierarchical:** Clustering hierárquico

**Métricas de avaliação:**
- Silhouette Score
- Davies-Bouldin Score
- Calinski-Harabasz Score

---

## 📁 Estrutura do Projeto

```
.
├── data/
│   └── book1-100k.csv                          # Dataset original
├── exports/
│   ├── clean_data/
│   │   ├── book_data_clean.csv                 # Dataset limpo
│   │   ├── book_data_removed.csv               # Registros removidos
│   │   └── cleaning_statistics.csv             # Estatísticas da limpeza
│   ├── duplicatas/
│   │   ├── title_near_duplicates_pairs.csv     # Pares de títulos similares
│   │   ├── title_near_duplicates_clusters.csv  # Clusters de duplicatas
│   │   ├── books_with_duplicates.csv           # Livros duplicados
│   │   ├── duplicates_statistics.csv           # Stats de duplicatas
│   │   ├── isbn_relatorio_completo.csv         # Relatório ISBN
│   │   ├── isbn_duplicatas.csv                 # ISBNs duplicados
│   │   ├── isbn_invalidos.csv                  # ISBNs inválidos
│   │   └── isbn_statistics.csv                 # Stats de ISBN
│   └── clustering/
│       ├── books_clustered.csv                 # Dataset com clusters
│       ├── cluster_summary.csv                 # Resumo dos clusters
│       ├── clustering_metrics.csv              # Métricas de avaliação
│       ├── cluster_X_examples.csv              # Exemplos por cluster
│       ├── clustering_visualization.png        # Visualizações 2D
│       ├── clustering_3d.png                   # Visualização 3D
│       └── elbow_analysis.png                  # Análise de K ótimo
├── goodreads_book1-100_eda.ipynb              # Notebook principal
└── README.md                                   # Este arquivo
```

---

## 🚀 Como Executar

### Pré-requisitos
```bash
pip install pandas numpy matplotlib scikit-learn pathlib
```

### Executar as Análises

#### 1. Análise Exploratória
Abra o notebook `goodreads_book1-100_eda.ipynb` e execute as células sequencialmente.

#### 2. Validação de Anos Inválidos
```python
# Execute o código de filtro de anos
year_col = "publishyear"
invalidYear = df[df[year_col] > 2020].copy()
validYear = df[df[year_col] <= 2020].copy()
```

#### 3. Validação de ISBN
```python
# Execute isbn_simples_v3.py
# Gera relatórios de ISBN na pasta exports/duplicatas/
```

#### 4. Detecção de Duplicatas de Título
```python
# Execute duplicatas_melhorado_v2.py
# Gera clusters e estatísticas na pasta exports/duplicatas/
```

#### 5. Criar Dataset Limpo
```python
# Execute criar_dataset_limpo.py
# Gera dataset limpo em exports/clean_data/
```

#### 6. Clustering POC
```python
# Execute clustering_poc_v2.py
# Requer dataset limpo previamente gerado
# Gera clusters e visualizações em exports/clustering/
```

---

## 📈 Resultados Principais

### 🧹 Qualidade dos Dados

| Métrica | Valor |
|---------|-------|
| Total de livros | 58.292 |
| Anos inválidos (> 2020) | ~0.5% |
| ISBNs duplicados | 194 (chave forte) |
| Títulos duplicados (TF-IDF) | 3.074 (5%) |
| Clusters de duplicatas | 2.060 |

### 🎯 Insights do Clustering

- **Número ótimo de clusters:** Determinado por Elbow Method e Silhouette Score
- **Silhouette Score:** ~0.3-0.5 (depende de K)
- **Features mais importantes:** Título > Autor > Língua
- **Aplicação:** Sistema de recomendação por similaridade

---

## 🤖 Soluções Possíveis com Dados

### ✅ Viáveis com Dataset Atual

| Solução | Descrição | Viabilidade |
|---------|-----------|-------------|
| **Clustering de livros** | Agrupar livros similares por metadados | ✅ Implementado |
| **Previsão de popularidade** | Estimar engajamento futuro | ✅ Viável |
| **Detecção de duplicatas** | Canonização automática | ✅ Implementado |
| **Análise de tendências** | Padrões temporais de publicação | ✅ Viável |

### ⚠️ Limitações do Dataset

| Solução | Motivo da Limitação |
|---------|---------------------|
| **Classificação de gênero** | Falta coluna `genres` ou `description` |
| **Recomendação personalizada** | Falta dados de interação de usuários |
| **Análise de sentimento** | Falta texto de reviews |
| **Geração de sinopses** | Falta `description` para treino |

---

## 💡 Aprendizados e Boas Práticas

### 📊 Qualidade de Dados
1. **Sempre valide antes de usar:** Anos, ISBNs, duplicatas
2. **Canonização é essencial:** 5% de duplicatas podem distorcer resultados
3. **Dados não normalizados exigem tratamento:** Títulos, autores, publishers

### 🤖 Machine Learning
1. **PCA reduz dimensionalidade:** Preserva 95% da variância
2. **Multiple algoritmos:** Compare K-Means, DBSCAN, Hierarchical
3. **Métricas são críticas:** Silhouette, Davies-Bouldin, Calinski-Harabasz
4. **Pesos nas features:** Título tem mais importância que ano

### 🎯 Produto
1. **Discovery bottom-up:** Conecte dados disponíveis à estratégia
2. **POC antes de escalar:** Valide viabilidade técnica
3. **Custos importam:** ML supervisionado vs não supervisionado
4. **Erros são inevitáveis:** Soluções probabilísticas precisam de guardrails

---

## 📚 Referências e Conceitos

### Machine Learning

#### **Aprendizado Não Supervisionado**
- **K-Means:** Agrupa por distância a centróides
- **DBSCAN:** Agrupa por densidade, detecta outliers
- **PCA:** Reduz dimensionalidade mantendo variância

#### **Embeddings e Similaridade**
- **TF-IDF:** Term Frequency - Inverse Document Frequency
- **Similaridade Cosseno:** Mede ângulo entre vetores
- **Union-Find:** Algoritmo para agrupar clusters

### Métricas de Clustering

- **Silhouette Score:** [-1, 1] - Maior é melhor (>0.5 é bom)
- **Davies-Bouldin:** [0, ∞] - Menor é melhor
- **Calinski-Harabasz:** [0, ∞] - Maior é melhor

### Canonização de Dados

- **Chaves fortes:** ISBN, IDs únicos
- **Chaves fracas:** Nome, autor (requerem normalização)
- **Métodos probabilísticos:** TF-IDF, BERT, embeddings

---

## 🎓 Contexto: Discovery Bottom-Up

### Missão Goodreads
> "O livro certo, nas mãos certas, no tempo certo, pode mudar o mundo."

### Objetivo
Ajudar usuários a descobrir livros que amam e tirar mais da leitura.

### Modelo de Receita
- Links afiliados (compre aqui)
- Anúncios
- Sorteios (Giveaways)

### Oportunidades Identificadas
1. **Recomendação por similaridade:** Agrupar livros similares
2. **Canonização de dados:** Melhorar qualidade do catálogo
3. **Detecção de tendências:** Insights de publicação
4. **Previsão de popularidade:** Identificar best-sellers

---

## 🔧 Tecnologias Utilizadas

- **Python 3.x**
- **Pandas:** Manipulação de dados
- **NumPy:** Computação numérica
- **Scikit-learn:** Machine Learning
  - TfidfVectorizer
  - KMeans, DBSCAN, AgglomerativeClustering
  - PCA
  - Métricas (silhouette_score, davies_bouldin_score, etc.)
- **Matplotlib:** Visualizações
- **Pathlib:** Gerenciamento de arquivos

---

## 👥 Autor

**Autor**: João Caetano Groff Gomes, Gestor de Produto AI

**Projeto Educacional:** Discovery Bottom-Up com Dados  
**Contexto:** Análise exploratória para gestão de produto orientada a dados

---

## 📝 Licença

Este é um projeto educacional para demonstração de conceitos de discovery e análise de dados.

---

## 📞 Contribuições

Sugestões e melhorias são bem-vindas! Abra uma issue ou pull request.

---

## 🙏 Agradecimentos

- **Kaggle:** Pela disponibilização do dataset Goodreads
- **Goodreads:** Pelos dados públicos de livros
- **Comunidade Python:** Pelas excelentes bibliotecas open-source

---

**Última atualização:** Outubro 2025
