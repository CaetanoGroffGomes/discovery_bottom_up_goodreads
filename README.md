# 📚 Discovery Bottom-Up com Dados - Goodreads

Projeto prático de **Product Discovery orientado a dados**, explorando como Machine Learning e análise de dados podem revelar oportunidades de produto e validar hipóteses técnicas.

## 🎯 Sobre o Projeto

Este projeto demonstra um processo completo de **discovery bottom-up** usando dados reais de livros do Goodreads. O objetivo é mostrar como gestores de produto podem avaliar a viabilidade técnica de soluções baseadas em dados antes de priorizá-las.

### 📖 Contexto

Baseado no artigo "Discovery Bottom-Up com Dados", este projeto ilustra:
- Como avaliar qualidade e estrutura de dados
- Identificar o que é possível (e o que não é) com dados disponíveis
- Validar hipóteses técnicas através de POCs (Provas de Conceito)
- Conectar soluções técnicas com objetivos de negócio

## 🗂️ Estrutura do Projeto

```
.
├── data/                          # Dados originais (não versionados)
│   └── book1-100k.csv            # Dataset Goodreads (~58k livros)
│
├── exports/                       # Todos os outputs do projeto
│   ├── clean_data/               # Dados limpos e processados
│   ├── clustering/               # Resultados de clusterização
│   │   └── analysis/            # Análises e visualizações
│   ├── duplicatas/               # Análises de ISBN e duplicatas
│   └── eda/                      # Análise exploratória
│
├── scripts/                       # Scripts Python organizados
│   ├── 01_limpeza_dados.py
│   ├── 02_analise_eda.py
│   ├── 03_clustering_poc.py
│   ├── 04_isbn_validacao.py
│   └── 05_visualizar_clusters.py
│
├── notebooks/                     # Jupyter notebooks (opcional)
│   └── goodreads_book1-100_eda.ipynb
│
├── requirements.txt               # Dependências Python
└── README.md                      # Este arquivo
```

## 🚀 Começando

### Pré-requisitos

- Python 3.8 ou superior
- pip (gerenciador de pacotes Python)

### Instalação

1. **Clone o repositório:**
```bash
git clone <url-do-repositorio>
cd discovery-bottom-up-dados
```

2. **Crie um ambiente virtual (recomendado):**
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

3. **Instale as dependências:**
```bash
pip install -r requirements.txt
```

4. **Coloque o dataset na pasta correta:**
```bash
# Crie a pasta data/ e adicione o arquivo book1-100k.csv
mkdir data
# Baixe o dataset do Kaggle e coloque em data/book1-100k.csv
```

## 📊 Pipeline de Análise

### 1️⃣ Limpeza e Preparação dos Dados

```bash
python scripts/01_limpeza_dados.py
```

**O que faz:**
- Carrega e padroniza nomes de colunas
- Trata valores ausentes e outliers
- Normaliza datas de publicação
- Cria dataset limpo em `exports/clean_data/`

**Principais descobertas:**
- 58% de dados ausentes em `pageNumbers`
- 65% de dados ausentes em `Language`
- Datas de publicação com outliers (livros em 3006!)
- ISBN com ~1% de valores nulos

### 2️⃣ Análise Exploratória (EDA)

```bash
python scripts/02_analise_eda.py
```

**O que faz:**
- Estatísticas descritivas completas
- Distribuições de variáveis
- Análise de correlações
- Identificação de padrões temporais
- Visualizações exportadas

**Outputs:**
- `exports/eda/` - Gráficos e relatórios
- Estatísticas por década, autor, língua, publisher

### 3️⃣ Validação de ISBN e Duplicatas

```bash
python scripts/04_isbn_validacao.py
```

**O que faz:**
- Valida ISBNs usando algoritmos de check digit
- Detecta duplicatas usando chave forte (ISBN)
- Identifica problemas de normalização
- Exporta relatórios detalhados

**Principais descobertas:**
- 194 duplicatas encontradas via ISBN
- ISBNs-10 e ISBN-13 misturados
- Necessidade de canonização de dados

### 4️⃣ Prova de Conceito - Clustering

```bash
python scripts/03_clustering_poc.py
```

**O que faz:**
- Implementa K-Means e Hierarchical Clustering
- Usa TF-IDF para vetorização de texto
- Avalia qualidade dos clusters (Silhouette, Davies-Bouldin)
- Aplica PCA para redução dimensional
- Exporta clusters e métricas

**Features utilizadas:**
- Nome do livro (TF-IDF)
- Autor
- Língua
- Ano de publicação
- Publisher

**Resultados:**
- ✅ **Sucesso técnico**: Clusters foram criados
- ⚠️ **Limitação de valor**: Agrupamentos muito genéricos
- 📋 **Conclusão**: Dados insuficientes para recomendações satisfatórias

### 5️⃣ Análise Visual dos Clusters

```bash
python scripts/05_visualizar_clusters.py
```

**O que faz:**
- Gera tabelas interativas HTML
- Cria visualizações comparativas
- Exporta análise detalhada de cada cluster
- Identifica características distintivas

**Outputs:**
- `cluster_table.html` - Tabela interativa com busca
- `cluster_simple.html` - Visão simplificada em cards
- `cluster_analysis.html` - Relatório completo
- Gráficos comparativos (PNG)

## 🎓 Aprendizados e Conclusões

### ✅ Viabilidade de Negócio
**Solução proposta:** Agrupamento de livros similares para recomendação inteligente

**Impacto esperado:**
- ✅ Aumentar engajamento na plataforma
- ✅ Melhorar descoberta de livros
- ✅ Potencial aumento em cliques afiliados

### ⚠️ Viabilidade Técnica

**Status:** Parcialmente viável com limitações

**Dados disponíveis:**
- ✅ Título, autor, ano, língua, publisher
- ❌ Descrição/sinopse dos livros
- ❌ Gêneros/categorias
- ❌ Reviews e comentários
- ❌ Dados de usuários
- ❌ Histórico de leitura

**Conclusão:**
> "Apenas com título, língua, autor, ano de publicação e dados gerais, os agrupamentos de livros são muito genéricos e não entregam valor de uma recomendação satisfatória."

### 📊 Problemas Identificados

1. **Pipeline de dados:**
   - Falta de normalização (autores, publishers)
   - Ausência de canonização (194 duplicatas por ISBN, ~3074 por TF-IDF)
   - Outliers temporais não tratados

2. **Qualidade de cadastros:**
   - Campos não normalizados
   - Validações fracas durante cadastro
   - Dados inconsistentes

3. **Completude:**
   - Dados críticos ausentes (descrição, gênero, reviews)
   - Sem dados de usuários
   - Sem histórico de interações

## 🔄 Próximos Passos

### Curto Prazo
1. **Melhorar pipeline de dados:**
   - Implementar validações no cadastro
   - Normalizar autores e publishers
   - Canonizar dados existentes

2. **Testar abordagens alternativas:**
   - DBSCAN para clusters de densidade
   - Outros métodos de embedding (BERT)
   - Combinação de múltiplos modelos

### Médio Prazo
3. **Adquirir dados complementares:**
   - Scraping de descrições/sinopses
   - Integração com APIs de livros
   - Coleta de gêneros via crowdsourcing

4. **Dados de usuários:**
   - Histórico de leitura
   - Reviews e avaliações
   - Interações sociais

### Longo Prazo
5. **Nova POC com dados completos:**
   - Recomendação colaborativa
   - Hybrid models (conteúdo + colaborativo)
   - A/B testing com usuários reais

## 🛠️ Tecnologias Utilizadas

- **Python 3.8+**
- **pandas** - Manipulação de dados
- **numpy** - Computação numérica
- **scikit-learn** - Machine Learning (clustering, TF-IDF, PCA)
- **matplotlib** - Visualizações

## 📚 Referências e Recursos

### Conceitos Abordados
- Discovery bottom-up em produto
- Análise exploratória de dados (EDA)
- Canonização de dados
- Clustering não-supervisionado
- Validação de hipóteses técnicas

### Modelos de ML Mencionados
- **Clusterização:** K-Means, Hierarchical, DBSCAN
- **Embeddings:** TF-IDF, BERT, word2vec
- **Classificação:** Decision Trees, Random Forest, XGBoost
- **Recomendação:** Collaborative Filtering, Content-Based

### Links Úteis
- [Kaggle - Goodreads Dataset](https://www.kaggle.com/)
- [Scikit-learn Documentation](https://scikit-learn.org/)
- [Hugging Face - Models](https://huggingface.co/models)

## 💡 Para Gestores de Produto

### O que Este Projeto Ensina

1. **Avaliar antes de priorizar:**
   - Nem toda ideia com dados é tecnicamente viável
   - POCs baratas validam hipóteses antes de investir

2. **Qualidade > Quantidade:**
   - Ter dados não é suficiente
   - Dados limpos e canonizados são essenciais

3. **Entender o possível:**
   - Machine Learning tem aplicações bem definidas
   - Conhecer as ferramentas disponíveis expande possibilidades

4. **Riscos probabilísticos:**
   - Soluções de ML/AI sempre erram
   - Avaliar impacto do erro é crucial
   - Intervenção humana pode ser necessária

### Framework de Decisão

```
Ideia com dados
    ↓
Avaliar qualidade dos dados
    ↓
Identificar o que é possível
    ↓
POC técnica rápida
    ↓
Dados suficientes? → NÃO → Adquirir dados ou pivotar
    ↓ SIM
Valor entregue satisfatório? → NÃO → Testar alternativas
    ↓ SIM
Conectar com objetivos de negócio
    ↓
Priorizar ou descartar
```

## 📄 Licença

Este projeto é um material educacional baseado no artigo "Discovery Bottom-Up com Dados".

## 👥 Autor

Projeto criado como demonstração prática de product discovery orientado a dados.

---

## 🔍 Análise Detalhada do Dataset

### Colunas e Qualidade

| Coluna | Tipo | Obrigatório | Normalizado | % Null/Inválido | Uso em ML |
|--------|------|-------------|-------------|-----------------|-----------|
| id | int | ✅ Sim | ✅ Sim | 0% | Chave primária |
| name | str | ✅ Sim | ❌ Não | 0% | ✅ Feature principal |
| pageNumbers | int | ❌ Não | ✅ Sim | 58% | ⚠️ Limitado |
| publishMonth | int | ✅ Sim | ✅ Sim | 0% | ✅ Feature temporal |
| publishDay | int | ✅ Sim | ✅ Sim | 0% | ✅ Feature temporal |
| publishYear | int | ✅ Sim | ❌ Não | ~0% | ✅ Feature temporal |
| publisher | str | ❌ Não | ❌ Não | 0.8% | ⚠️ Requer normalização |
| countsOfReview | int | ✅ Sim | ✅ Sim | 0% | ✅ Popularidade |
| language | str | ❌ Não | ❌ Não | 65% | ⚠️ Muito ausente |
| authors | str | ✅ Sim | ❌ Não | 0% | ⚠️ Requer normalização |
| rating | decimal | ✅ Sim | ✅ Sim | 0% | ✅ Qualidade |
| isbn | int | ❌ Não | ❌ Não | 0.9% | ⚠️ Chave fraca |

### Problemas Identificados

#### 🔴 Críticos
- Falta de descrição/sinopse dos livros
- Falta de gêneros/categorias
- Ausência de dados de usuários
- 65% de dados ausentes em língua

#### 🟡 Importantes
- Autores não normalizados (duplicatas)
- Publishers não normalizados
- ISBN com duplicatas e inconsistências
- Outliers temporais (livros em 3006)

#### 🟢 Menores
- PageNumbers com muitos nulos (58%)
- Datas separadas em 3 colunas
- Formato CSV (lento para produção)

---

## 📞 Suporte

Para dúvidas sobre o projeto:
1. Leia a documentação completa
2. Revise o artigo "Discovery Bottom-Up com Dados"
3. Analise os notebooks e scripts
4. Abra uma issue no repositório

---

**🎯 Lembre-se:** O objetivo não é ter a melhor solução técnica, mas **validar se seus dados permitem entregar valor real ao usuário antes de priorizar a solução**.
