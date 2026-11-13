# Modelagem Preditiva de WinShares na NBA (1950 - 2017)

Projeto pessoal de Ciencia de Dados e Machine Learning desenvolvido em Python com o objetivo de prever a metrica **WinShares** (estimativa da contribuicao individual de um jogador em vitorias da sua equipe) a partir de estatisticas historicas da NBA entre 1950 e 2017.

---

## Contexto e Objetivo

No basquete profissional, avaliar o impacto real de um atleta vai alem da pontuacao bruta. A metrica `WinShares` sintetiza o quanto um jogador contribui para as vitorias do time ao longo de uma temporada.

Este projeto implementa um pipeline completo de **Aprendizagem Supervisionada de Regressao** em Jupyter Notebook para estimar o `WinShares` exclusivamente a partir de estatisticas de desempenho em quadra (jogos disputados, arremessos convertidos e tentados, aproveitamento real, lances livres, assistencias, faltas e pontos), garantindo integridade metodologica e interpretabilidade.

---

## Estrutura do Repositorio

- `nba_winshares_prediction.ipynb`: Notebook Jupyter principal contendo toda a analise exploratoria, preparacao de dados, selecao de atributos, modelagem preditiva, avaliacao estatistica e visualizacoes graficas.
- `nba_stats.csv`: Conjunto de dados historicos utilizado na modelagem (24.624 registros e 22 atributos).
- `.gitignore`: Configuracao de arquivos ignorados no versionamento.

---

## Metodologia Aplicada

### 1. Exploracao e Limpeza dos Dados
- Inspecao inicial de tipos de variaveis, distribuicoes e valores ausentes.
- Remocao de 39 registros sem valor preenchido na variavel-alvo (`WinShares`), resultando em **24.585 registros validos**.

### 2. Prevencao de Vazamento de Dados (*Data Leakage*)
- Na estrutura original da base, `WinShares` equivale diretamente a soma de `OffensiveWinShares` e `DefensiveWinShares`.
- Ambas as colunas foram removidas antes da etapa de selecao de atributos e modelagem, assegurando que o algoritmo aprenda padroes genuinos a partir das estatisticas individuais em quadra.

### 3. Preparacao de Atributos
- **Imputacao de valores ausentes:** Preenchimento de lacunas em variaveis numericas utilizando a mediana de cada atributo.
- **Codificacao categorica:** Agrupamento de posicoes raras (< 100 ocorrencias) na categoria `Outros` e aplicacao de *One-Hot Encoding* na coluna `Posicao`, totalizando **26 atributos** prontos para modelagem.

### 4. Selecao de Atributos (*Feature Selection*)
- Aplicacao do metodo estatistico `SelectKBest` com a funcao de pontuacao `f_regression` para ranquear os atributos pela forca de correlacao com o alvo `WinShares`.
- Selecao dos **12 atributos mais informativos**: `Jogos`, `TrueShootingPercentage`, `FieldGoals`, `FieldGoalAttempts`, `FieldGoalPercentage`, `TwoPointFieldGoals`, `TwoPointFieldGoalAttempts`, `FreeThrows`, `FreeThrowAttempts`, `Assists`, `PersonalFouls` e `Points`.

### 5. Divisao Treino/Teste e Modelagem
- Divisao reprodutivel da base em **75% para treinamento (18.438 registros)** e **25% para teste (6.147 registros)** com `random_state=42`.
- Treinamento de um modelo **Random Forest Regressor** (`RandomForestRegressor`, `n_estimators=150`, `max_depth=18`, `min_samples_leaf=3`) utilizando a biblioteca `scikit-learn`.

---

## Resultados Obtidos (Base de Teste - 25%)

| Metrica | Valor Obtido | Interpretacao |
| :--- | :--- | :--- |
| **R2 (Coeficiente de Determinacao)** | **0.8600 (86.00%)** | O modelo explica 86% da variancia do `WinShares` em dados ineditos. |
| **MAE (Erro Absoluto Medio)** | **0.7091** | Desvio medio inferior a 0,71 vitoria estimada por temporada. |
| **RMSE (Raiz do Erro Quadratico Medio)** | **1.1476** | Estabilidade consistente sem grande dispersao de erros extremos. |

O notebook tambem apresenta comparacao direta entre valores reais e previstos, grafico de dispersao com linha de referencia ideal ($y = x$) e histograma de distribuicao dos residuos.

---

## Tecnologias Utilizadas

- **Linguagem:** Python 3.12
- **Manipulacao e Analise de Dados:** `pandas`, `numpy`
- **Visualizacao de Dados:** `matplotlib`, `seaborn`
- **Machine Learning:** `scikit-learn`
- **Ambiente:** Jupyter Notebook

---

## Como Executar Localmente

1. Clone o repositorio:
   ```bash
   git clone https://github.com/LuizSantiagojpg/nba-winshares-ml.git
   cd nba-winshares-ml
   ```
2. Instale as dependencias principais:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```
3. Abra o notebook:
   ```bash
   jupyter notebook nba_winshares_prediction.ipynb
   ```
