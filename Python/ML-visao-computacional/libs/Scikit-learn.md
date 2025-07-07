# [⬅ Voltar para o índice principal](../../../README.md)

# Documentação Completa de Scikit-learn

## Sumário

1. [Introdução ao Scikit-learn](#introdução-ao-scikit-learn)
2. [Instalação e Configuração](#instalação-e-configuração)
3. [Conceitos Fundamentais](#conceitos-fundamentais)
   - [Estimadores](#estimadores)
   - [Transformadores](#transformadores)
   - [Pipelines](#pipelines)
   - [Conjunto de Dados](#conjunto-de-dados)
4. [Pré-processamento de Dados](#pré-processamento-de-dados)
   - [Tratamento de Dados Faltantes](#tratamento-de-dados-faltantes)
   - [Codificação de Variáveis Categóricas](#codificação-de-variáveis-categóricas)
   - [Normalização e Padronização](#normalização-e-padronização)
   - [Redução de Dimensionalidade](#redução-de-dimensionalidade)
5. [Algoritmos de Aprendizado Supervisionado](#algoritmos-de-aprendizado-supervisionado)
   - [Regressão Linear](#regressão-linear)
   - [Regressão Logística](#regressão-logística)
   - [Árvores de Decisão](#árvores-de-decisão)
   - [Random Forests](#random-forests)
   - [Support Vector Machines (SVM)](#support-vector-machines-svm)
   - [Naive Bayes](#naive-bayes)
   - [K-Nearest Neighbors (KNN)](#k-nearest-neighbors-knn)
   - [Gradient Boosting](#gradient-boosting)
6. [Algoritmos de Aprendizado Não-Supervisionado](#algoritmos-de-aprendizado-não-supervisionado)
   - [K-Means](#k-means)
   - [DBSCAN](#dbscan)
   - [Agrupamento Hierárquico](#agrupamento-hierárquico)
   - [PCA](#pca)
   - [t-SNE](#t-sne)
7. [Seleção e Validação de Modelos](#seleção-e-validação-de-modelos)
   - [Validação Cruzada](#validação-cruzada)
   - [Grid Search](#grid-search)
   - [Métricas de Avaliação](#métricas-de-avaliação)
8. [Ajuste de Hiperparâmetros](#ajuste-de-hiperparâmetros)
9. [Trabalho com Dados Desbalanceados](#trabalho-com-dados-desbalanceados)
10. [Exemplos Práticos Completos](#exemplos-práticos-completos)
11. [Dicas e Boas Práticas](#dicas-e-boas-práticas)
12. [Recursos Adicionais](#recursos-adicionais)

## Introdução ao Scikit-learn

Scikit-learn (também conhecido como sklearn) é uma biblioteca de aprendizado de máquina de código aberto para Python, projetada para interoperar com as bibliotecas numéricas e científicas NumPy e SciPy. Ela inclui diversos algoritmos de classificação, regressão, agrupamento e redução de dimensionalidade, além de ferramentas para seleção e avaliação de modelos.

### Principais características

- Interface consistente e simples de usar
- Grande variedade de ferramentas para modelagem e análise de dados
- Documentação completa e de qualidade
- Alto desempenho para algoritmos científicos e numéricos
- Código aberto e acessível a todos

## Instalação e Configuração

### Instalação via pip

```python
# Instalação básica
pip install scikit-learn

# Ou com especificação de versão
pip install scikit-learn==1.3.0
```

### Instalação via conda

```python
# Usando conda
conda install scikit-learn

# Para um ambiente específico
conda install -c conda-forge scikit-learn
```

### Verificação da instalação

```python
# Verificando a instalação e versão
import sklearn
print(sklearn.__version__)

# Saída esperada: (por exemplo) 1.6.1
```

### Dependências principais

Scikit-learn depende de várias bibliotecas que serão instaladas automaticamente:

- NumPy: Para operações numéricas eficientes
- SciPy: Para algoritmos científicos
- Joblib: Para paralelização de código
- Matplotlib (opcional): Para visualizações
- Pandas (opcional): Para manipulação de dados

## Conceitos Fundamentais

### Estimadores

Os estimadores são objetos que implementam os métodos `fit()` e `predict()` (ou similares).

```python
# Exemplo completo de um modelo de Regressão Linear com Scikit-Learn

from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
import numpy as np
import matplotlib.pyplot as plt

# Gerando dados sintéticos (simplesmente para exemplo)
# X: variável independente, y: variável dependente (y = 2x + ruído)
np.random.seed(42)
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)

# Separando os dados em treino e teste (80% treino, 20% teste)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Criando o estimador (modelo de regressão linear)
modelo = LinearRegression()

# Treinando o modelo
modelo.fit(X_train, y_train)

# Fazendo previsões
previsoes = modelo.predict(X_test)

# Avaliando o modelo
mse = mean_squared_error(y_test, previsoes)
r2 = r2_score(y_test, previsoes)

print("Coeficiente angular (slope):", modelo.coef_[0][0])
print("Intercepto:", modelo.intercept_[0])
print("Erro quadrático médio (MSE):", mse)
print("R² (coeficiente de determinação):", r2)

# Visualizando os resultados
plt.scatter(X_test, y_test, color="blue", label="Dados reais")
plt.plot(X_test, previsoes, color="red", label="Previsão")
plt.xlabel("X")
plt.ylabel("y")
plt.title("Regressão Linear - Previsões vs Dados Reais")
plt.legend()
plt.show()

# Saída:
# Coeficiente angular (slope): 2.7993236574802762
# Intercepto: 4.142913319458566
# Erro quadrático médio (MSE): 0.6536995137170021
# R² (coeficiente de determinação): 0.8072059636181392
```

### Transformadores

Os transformadores são objetos que modificam ou extraem recursos dos dados, implementando os métodos `fit()` e `transform()`.

```python
# Exemplo completo de uso do StandardScaler com Scikit-Learn

from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
import numpy as np
import matplotlib.pyplot as plt

# Gerando dados sintéticos com múltiplas features
np.random.seed(42)
X = np.random.rand(100, 2) * [100, 10] + [50, 5]  # escala diferente para cada feature

# Separando os dados em treino e teste
X_train, X_test = train_test_split(X, test_size=0.2, random_state=42)

# Criando o transformador
scaler = StandardScaler()

# Ajustando o transformador aos dados de treino
scaler.fit(X_train)

# Transformando os dados de treino e teste
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Alternativa: ajustar e transformar em uma só operação
X_train_scaled_alt = scaler.fit_transform(X_train)  # mesmo que os anteriores

# Visualizando antes e depois da padronização
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.scatter(X_train[:, 0], X_train[:, 1], color="blue", alpha=0.6)
plt.title("Antes da Padronização")
plt.xlabel("Feature 1")
plt.ylabel("Feature 2")

plt.subplot(1, 2, 2)
plt.scatter(X_train_scaled[:, 0], X_train_scaled[:, 1], color="green", alpha=0.6)
plt.title("Depois da Padronização")
plt.xlabel("Feature 1 (padronizada)")
plt.ylabel("Feature 2 (padronizada)")

plt.tight_layout()
plt.show()

```

### Pipelines

Pipelines permitem encadear vários estimadores em uma sequência única.

```python
# Exemplo de pipeline com pré-processamento e modelo
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC

# Criando o pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),  # Primeiro passo: padronização
    ('svm', SVC(kernel='rbf'))     # Segundo passo: classificador SVM
])

# Treinando o pipeline completo
pipeline.fit(X_train, y_train)

# Fazendo previsões
previsoes = pipeline.predict(X_test)
```

### Conjunto de Dados

Scikit-learn inclui conjuntos de dados para teste e aprendizado.

```python
# Carregando conjuntos de dados embutidos
from sklearn.datasets import load_iris, load_boston, load_digits, fetch_california_housing

# Dataset para classificação
iris = load_iris()
X_iris, y_iris = iris.data, iris.target

# Dataset para regressão
housing = fetch_california_housing()
X_housing, y_housing = housing.data, housing.target

# Acessando informações do dataset
print(iris.DESCR)  # Descrição do conjunto de dados
print(iris.feature_names)  # Nomes dos recursos
print(iris.target_names)  # Nomes das classes alvo
```

## Pré-processamento de Dados

### Tratamento de Dados Faltantes

```python
# Imputação de valores faltantes
from sklearn.impute import SimpleImputer
import numpy as np

# Criando dados com valores faltantes
X = np.array([
    [1, 2, np.nan],
    [3, np.nan, 0],
    [np.nan, 4, 5]
])

# Criando o imputador (substituir por média)
imputer = SimpleImputer(strategy='mean')

# Aplicando a imputação
X_imputed = imputer.fit_transform(X)
print("Dados originais:\n", X)
print("Dados após imputação:\n", X_imputed)
```

### Codificação de Variáveis Categóricas

```python
# One-Hot Encoding para variáveis categóricas
from sklearn.preprocessing import OneHotEncoder
import numpy as np
import pandas as pd

# Dados categóricos de exemplo
dados = pd.DataFrame({
    'cor': ['vermelho', 'azul', 'verde', 'vermelho', 'azul'],
    'tamanho': ['pequeno', 'médio', 'grande', 'médio', 'pequeno']
})

# Criando e aplicando o codificador
encoder = OneHotEncoder(sparse_output=False)
dados_encoded = encoder.fit_transform(dados)

# Visualizando os resultados
print("Dados originais:\n", dados)
print("\nDados codificados:\n", dados_encoded)
print("\nNomes das colunas após codificação:\n", encoder.get_feature_names_out())
```

```python
# Label Encoding para variáveis categóricas ordinais
from sklearn.preprocessing import LabelEncoder

# Dados categóricos ordinais
tamanhos = ['pequeno', 'médio', 'grande', 'médio', 'pequeno']

# Aplicando Label Encoding
encoder = LabelEncoder()
tamanhos_encoded = encoder.fit_transform(tamanhos)

print("Originais:", tamanhos)
print("Codificados:", tamanhos_encoded)
print("Classes:", encoder.classes_)
```

### Normalização e Padronização

```python
# Padronização (Z-score): media=0, desvio-padrão=1
from sklearn.preprocessing import StandardScaler
import numpy as np

# Dados de exemplo
X = np.array([[0, 0], [0, 0], [1, 1], [1, 1]])

# Padronização
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

print("Dados originais:\n", X)
print("Dados padronizados:\n", X_scaled)
print("Média dos recursos:", scaler.mean_)
print("Desvio padrão dos recursos:", np.sqrt(scaler.var_))
```

```python
# Normalização Min-Max (intervalo [0,1])
from sklearn.preprocessing import MinMaxScaler

# Dados de exemplo
X = np.array([[1, -1, 2], [2, 0, 0], [0, 1, -1]])

# Normalização
normalizer = MinMaxScaler()
X_norm = normalizer.fit_transform(X)

print("Dados originais:\n", X)
print("Dados normalizados [0,1]:\n", X_norm)
```

### Redução de Dimensionalidade

```python
# PCA - Análise de Componentes Principais
from sklearn.decomposition import PCA
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt

# Carregando o dataset
iris = load_iris()
X = iris.data
y = iris.target

# Aplicando PCA para reduzir para 2 dimensões
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

# Visualização dos resultados
plt.figure(figsize=(10, 8))
for i, target_name in enumerate(iris.target_names):
    plt.scatter(X_pca[y == i, 0], X_pca[y == i, 1],
                label=target_name, alpha=0.7)

plt.xlabel('Primeira Componente Principal')
plt.ylabel('Segunda Componente Principal')
plt.legend()
plt.title('PCA do conjunto Iris')
plt.grid(True)

# Verificando a variância explicada
print("Variância explicada por componente:", pca.explained_variance_ratio_)
print("Variância explicada acumulada:", sum(pca.explained_variance_ratio_))
```

## Algoritmos de Aprendizado Supervisionado

### Regressão Linear

```python
# Regressão Linear Simples
from sklearn.linear_model import LinearRegression
import numpy as np
import matplotlib.pyplot as plt
from sklearn.metrics import mean_squared_error, r2_score

# Criando dados sintéticos
np.random.seed(42)
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)

# Dividindo em treino e teste
X_train, X_test = X[:80], X[80:]
y_train, y_test = y[:80], y[80:]

# Criando e treinando o modelo
modelo = LinearRegression()
modelo.fit(X_train, y_train)

# Fazendo previsões
y_pred = modelo.predict(X_test)

# Avaliando o modelo
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print(f"Coeficiente (inclinação): {modelo.coef_[0][0]:.4f}")
print(f"Intercepto: {modelo.intercept_[0]:.4f}")
print(f"Erro Quadrático Médio: {mse:.4f}")
print(f"R² Score: {r2:.4f}")

# Visualizando os resultados
plt.figure(figsize=(10, 6))
plt.scatter(X_train, y_train, color='blue', label='Dados de treino')
plt.scatter(X_test, y_test, color='green', label='Dados de teste')
plt.plot(X_test, y_pred, color='red', linewidth=2, label='Previsões')
plt.xlabel('X')
plt.ylabel('y')
plt.title('Regressão Linear')
plt.legend()
plt.grid(True)
```

### Regressão Logística

```python
# Regressão Logística para classificação binária
from sklearn.linear_model import LogisticRegression
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# Carregando o dataset (classificação de câncer de mama)
cancer = load_breast_cancer()
X = cancer.data
y = cancer.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42)

# Padronizando os dados
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Criando e treinando o modelo
modelo = LogisticRegression(random_state=42, max_iter=1000)
modelo.fit(X_train_scaled, y_train)

# Fazendo previsões
y_pred = modelo.predict(X_test_scaled)

# Avaliando o modelo
acuracia = accuracy_score(y_test, y_pred)
matriz_confusao = confusion_matrix(y_test, y_pred)
relatorio = classification_report(y_test, y_pred, target_names=['maligno', 'benigno'])

print(f"Acurácia: {acuracia:.4f}")
print(f"\nMatriz de Confusão:\n{matriz_confusao}")
print(f"\nRelatório de Classificação:\n{relatorio}")

# Probabilidades das previsões
y_prob = modelo.predict_proba(X_test_scaled)
print("\nPrimeiras 5 probabilidades de previsão:")
for i in range(5):
    print(f"Amostra {i+1}: Classe 0 = {y_prob[i][0]:.4f}, Classe 1 = {y_prob[i][1]:.4f}")
```

### Árvores de Decisão

```python
# Árvore de Decisão para classificação
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report
import matplotlib.pyplot as plt

# Carregando o dataset Iris
iris = load_iris()
X = iris.data
y = iris.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)

# Criando e treinando o modelo
arvore = DecisionTreeClassifier(max_depth=3, random_state=42)
arvore.fit(X_train, y_train)

# Fazendo previsões
y_pred = arvore.predict(X_test)

# Avaliando o modelo
acuracia = accuracy_score(y_test, y_pred)
relatorio = classification_report(y_test, y_pred, target_names=iris.target_names)

print(f"Acurácia: {acuracia:.4f}")
print(f"\nRelatório de Classificação:\n{relatorio}")

# Visualizando a árvore
plt.figure(figsize=(20, 10))
plot_tree(arvore, filled=True, feature_names=iris.feature_names,
          class_names=iris.target_names, rounded=True)
plt.title("Árvore de Decisão - Dataset Iris")

# Importância dos recursos
importancia = arvore.feature_importances_
indices = np.argsort(importancia)[::-1]

print("\nImportância dos recursos:")
for i, idx in enumerate(indices):
    print(f"{i+1}. {iris.feature_names[idx]}: {importancia[idx]:.4f}")
```

### Random Forests

```python
# Random Forest para classificação
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report
import matplotlib.pyplot as plt
import numpy as np

# Carregando o dataset de vinhos
wine = load_wine()
X = wine.data
y = wine.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)

# Criando e treinando o modelo
rf = RandomForestClassifier(n_estimators=100, max_depth=5,
                           random_state=42, oob_score=True)
rf.fit(X_train, y_train)

# Fazendo previsões
y_pred = rf.predict(X_test)

# Avaliando o modelo
acuracia = accuracy_score(y_test, y_pred)
relatorio = classification_report(y_test, y_pred, target_names=wine.target_names)
oob_score = rf.oob_score_

print(f"Acurácia: {acuracia:.4f}")
print(f"Score Out-of-Bag: {oob_score:.4f}")
print(f"\nRelatório de Classificação:\n{relatorio}")

# Importância dos recursos
importancia = rf.feature_importances_
indices = np.argsort(importancia)[::-1]

# Visualizando a importância dos recursos
plt.figure(figsize=(12, 8))
plt.title("Importância dos Recursos - Random Forest")
plt.bar(range(X.shape[1]), importancia[indices], align="center")
plt.xticks(range(X.shape[1]), [wine.feature_names[i] for i in indices], rotation=90)
plt.tight_layout()
plt.ylabel("Importância")
plt.grid(axis='y')
```

### Support Vector Machines (SVM)

```python
# SVM para classificação
from sklearn.svm import SVC
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score, classification_report
from sklearn.model_selection import GridSearchCV

# Carregando o dataset
cancer = load_breast_cancer()
X = cancer.data
y = cancer.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42)

# Padronizando os dados
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Criando e treinando o modelo básico
svm = SVC(kernel='rbf', probability=True, random_state=42)
svm.fit(X_train_scaled, y_train)

# Avaliando o modelo básico
y_pred = svm.predict(X_test_scaled)
acuracia = accuracy_score(y_test, y_pred)
relatorio = classification_report(y_test, y_pred)

print(f"Acurácia do modelo básico: {acuracia:.4f}")
print(f"\nRelatório de Classificação:\n{relatorio}")

# Busca em grade para encontrar os melhores hiperparâmetros
param_grid = {
    'C': [0.1, 1, 10, 100],
    'gamma': [0.001, 0.01, 0.1, 1],
    'kernel': ['rbf', 'linear']
}

grid_search = GridSearchCV(SVC(probability=True), param_grid, cv=5, scoring='accuracy')
grid_search.fit(X_train_scaled, y_train)

# Melhores parâmetros e resultados
print(f"\nMelhores parâmetros: {grid_search.best_params_}")
print(f"Melhor score de validação cruzada: {grid_search.best_score_:.4f}")

# Avaliando o modelo otimizado
best_svm = grid_search.best_estimator_
y_pred_best = best_svm.predict(X_test_scaled)
acuracia_best = accuracy_score(y_test, y_pred_best)
relatorio_best = classification_report(y_test, y_pred_best)

print(f"\nAcurácia do modelo otimizado: {acuracia_best:.4f}")
print(f"\nRelatório de Classificação (modelo otimizado):\n{relatorio_best}")
```

### Naive Bayes

```python
# Classificador Naive Bayes
from sklearn.naive_bayes import GaussianNB
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
import seaborn as sns
import matplotlib.pyplot as plt

# Carregando o dataset Iris
iris = load_iris()
X = iris.data
y = iris.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)

# Criando e treinando o modelo
nb = GaussianNB()
nb.fit(X_train, y_train)

# Fazendo previsões
y_pred = nb.predict(X_test)

# Avaliando o modelo
acuracia = accuracy_score(y_test, y_pred)
matriz_confusao = confusion_matrix(y_test, y_pred)
relatorio = classification_report(y_test, y_pred, target_names=iris.target_names)

print(f"Acurácia: {acuracia:.4f}")
print(f"\nRelatório de Classificação:\n{relatorio}")

# Visualizando a matriz de confusão
plt.figure(figsize=(10, 8))
sns.heatmap(matriz_confusao, annot=True, fmt='d', cmap='Blues',
            xticklabels=iris.target_names,
            yticklabels=iris.target_names)
plt.xlabel('Previsão')
plt.ylabel('Valor Real')
plt.title('Matriz de Confusão - Naive Bayes')

# Probabilidades posteriores
y_prob = nb.predict_proba(X_test)
print("\nPrimeiras 3 probabilidades de previsão:")
for i in range(3):
    probas = [f"{iris.target_names[j]}: {y_prob[i][j]:.4f}" for j in range(3)]
    print(f"Exemplo {i+1}: {', '.join(probas)}")
```

### K-Nearest Neighbors (KNN)

```python
# Classificador K-Nearest Neighbors
from sklearn.neighbors import KNeighborsClassifier
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report
from sklearn.preprocessing import StandardScaler
import matplotlib.pyplot as plt
import numpy as np

# Carregando o dataset de dígitos
digits = load_digits()
X = digits.data
y = digits.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42)

# Padronizando os dados
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Criando e treinando o modelo
knn = KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train_scaled, y_train)

# Fazendo previsões
y_pred = knn.predict(X_test_scaled)

# Avaliando o modelo
acuracia = accuracy_score(y_test, y_pred)
relatorio = classification_report(y_test, y_pred)

print(f"Acurácia: {acuracia:.4f}")
print(f"\nRelatório de Classificação:\n{relatorio}")

# Testando diferentes valores de k
k_values = list(range(1, 31, 2))
accuracy_scores = []

for k in k_values:
    knn = KNeighborsClassifier(n_neighbors=k)
    knn.fit(X_train_scaled, y_train)
    y_pred = knn.predict(X_test_scaled)
    accuracy_scores.append(accuracy_score(y_test, y_pred))

# Visualizando a acurácia para diferentes valores de k
plt.figure(figsize=(12, 6))
plt.plot(k_values, accuracy_scores, marker='o', linestyle='-')
plt.xlabel('Número de Vizinhos (k)')
plt.ylabel('Acurácia')
plt.title('Efeito do Parâmetro k na Acurácia do KNN')
plt.xticks(k_values)
plt.grid(True)

# Encontrando o melhor k
melhor_k = k_values[np.argmax(accuracy_scores)]
melhor_acuracia = max(accuracy_scores)
print(f"\nMelhor valor de k: {melhor_k}")
print(f"Melhor acurácia: {melhor_acuracia:.4f}")
```

### Gradient Boosting

```python
# Gradient Boosting para classificação
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report, roc_curve, auc
import matplotlib.pyplot as plt
import numpy as np

# Carregando o dataset
cancer = load_breast_cancer()
X = cancer.data
y = cancer.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42)

# Criando e treinando o modelo
gb = GradientBoostingClassifier(n_estimators=100, learning_rate=0.1,
                               max_depth=3, random_state=42)
gb.fit(X_train, y_train)

# Fazendo previsões
y_pred = gb.predict(X_test)
y_prob = gb.predict_proba(X_test)[:, 1]

# Avaliando o modelo
acuracia = accuracy_score(y_test, y_pred)
relatorio = classification_report(y_test, y_pred, target_names=cancer.target_names)

print(f"Acurácia: {acuracia:.4f}")
print(f"\nRelatório de Classificação:\n{relatorio}")

# Curva ROC
fpr, tpr, _ = roc_curve(y_test, y_prob)
roc_auc = auc(fpr, tpr)

plt.figure(figsize=(10, 8))
plt.plot(fpr, tpr, color='darkorange', lw=2,
         label=f'ROC curve (area = {roc_auc:.2f})')
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.05])
plt.xlabel('Taxa de Falsos Positivos')
plt.ylabel('Taxa de Verdadeiros Positivos')
plt.title('Curva ROC - Gradient Boosting')
plt.legend(loc="lower right")
plt.grid(True)

# Importância dos recursos
importancia = gb.feature_importances_
indices = np.argsort(importancia)[::-1]

# Top 10 recursos mais importantes
plt.figure(figsize=(12, 6))
plt.title("Top 10 Recursos Mais Importantes")
plt.bar(range(10), importancia[indices[:10]], align="center")
plt.xticks(range(10), [cancer.feature_names[i] for i in indices[:10]], rotation=90)
plt.tight_layout()
plt.grid(axis='y')
```

## Algoritmos de Aprendizado Não-Supervisionado

### K-Means

```python
# Algoritmo K-Means para clustering
from sklearn.cluster import KMeans
from sklearn.datasets import make_blobs
import matplotlib.pyplot as plt
from sklearn.metrics import silhouette_score
import numpy as np

# Gerando dados sintéticos com 3 clusters
X, y_true = make_blobs(n_samples=300, centers=3, cluster_std=0.60, random_state=42)

# Criando e treinando o modelo
kmeans = KMeans(n_clusters=3, init='k-means++', max_iter=300, n_init=10, random_state=42)
y_pred = kmeans.fit_predict(X)

# Centróides dos clusters
centroides = kmeans.cluster_centers_

# Avaliando o modelo
silhouette_avg = silhouette_score(X, y_pred)
print(f"Silhouette Score: {silhouette_avg:.4f}")

# Visualizando os clusters
plt.figure(figsize=(10, 8))
plt.scatter(X[:, 0], X[:, 1], c=y_pred, s=50, cmap='viridis', alpha=0.8)
plt.scatter(centroides[:, 0], centroides[:, 1], c='red', marker='X', s=200, label='Centróides')
plt.title(f'Clusters K-Means (Silhouette Score: {silhouette_avg:.4f})')
plt.legend()
plt.grid(True)

# Encontrando o número ideal de clusters com o método Elbow
distortions = []
silhouette_scores = []
K_range = range(1, 11)

for k in K_range:
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(X)
    distortions.append(kmeans.inertia_)

    if k > 1:  # Silhouette Score precisa de pelo menos 2 clusters
        silhouette_scores.append(silhouette_score(X, kmeans.labels_))

# Plotando a curva do método Elbow
plt.figure(figsize=(12, 5))

# Gráfico da inércia
plt.subplot(1, 2, 1)
plt.plot(K_range, distortions, marker='o', linestyle='-')
plt.xlabel('Número de Clusters (k)')
plt.ylabel('Inércia')
plt.title('Método Elbow para Determinar k Ideal')
plt.grid(True)

# Gráfico Silhouette Score
plt.subplot(1, 2, 2)
plt.plot(list(K_range)[1:], silhouette_scores, marker='o', linestyle='-')
plt.xlabel('Número de Clusters (k)')
plt.ylabel('Silhouette Score')
plt.title('Silhouette Score por Número de Clusters')
plt.grid(True)

plt.tight_layout()
```

### DBSCAN

```python
# DBSCAN - Clustering baseado em densidade
from sklearn.cluster import DBSCAN
from sklearn.datasets import make_moons
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import silhouette_score
import matplotlib.pyplot as plt
import numpy as np

# Gerando dados em formato de lua (não lineares)
X, y = make_moons(n_samples=200, noise=0.05, random_state=42)

# Padronizando os dados
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Aplicando DBSCAN
dbscan = DBSCAN(eps=0.3, min_samples=5)
y_pred = dbscan.fit_predict(X_scaled)

# Contando o número de clusters e ruídos
n_clusters = len(set(y_pred)) - (1 if -1 in y_pred else 0)
n_noise = list(y_pred).count(-1)

print(f"Número estimado de clusters: {n_clusters}")
print(f"Número estimado de pontos de ruído: {n_noise}")

# Avaliando o modelo (apenas para amostras não rotuladas como ruído)
if len(set(y_pred)) > 1 and sum(y_pred != -1) > 1:
    # Filtrando os pontos que não são ruído
    X_no_noise = X_scaled[y_pred != -1]
    y_pred_no_noise = y_pred[y_pred != -1]

    if len(set(y_pred_no_noise)) > 1:  # Precisa de pelo menos 2 clusters
        silhouette_avg = silhouette_score(X_no_noise, y_pred_no_noise)
        print(f"Silhouette Score (excluindo ruído): {silhouette_avg:.4f}")

# Visualizando os resultados
plt.figure(figsize=(12, 6))

# Plotando os resultados do DBSCAN
unique_labels = set(y_pred)
colors = plt.cm.Spectral(np.linspace(0, 1, len(unique_labels)))

for k, col in zip(unique_labels, colors):
    if k == -1:
        # Pontos de ruído em preto
        col = 'k'

    mask = (y_pred == k)
    plt.scatter(X_scaled[mask, 0], X_scaled[mask, 1], s=50,
                c=[col], edgecolors='k', label=f'Cluster {k}' if k != -1 else 'Ruído')

plt.title(f'DBSCAN: {n_clusters} clusters encontrados e {n_noise} pontos de ruído')
plt.legend()
plt.grid(True)
```

### Agrupamento Hierárquico

```python
# Agrupamento Hierárquico
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, linkage
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt
import numpy as np

# Carregando o dataset Iris (apenas 2 features para visualização)
iris = load_iris()
X = iris.data[:, :2]  # Usando apenas comprimento e largura da sépala
y = iris.target

# Aplicando o agrupamento hierárquico
modelo = AgglomerativeClustering(n_clusters=3, linkage='ward')
y_pred = modelo.fit_predict(X)

# Visualizando os clusters
plt.figure(figsize=(10, 8))
plt.scatter(X[:, 0], X[:, 1], c=y_pred, s=50, cmap='viridis', alpha=0.8)
plt.title('Agrupamento Hierárquico - Dataset Iris')
plt.xlabel(iris.feature_names[0])
plt.ylabel(iris.feature_names[1])
plt.grid(True)

# Criando o dendrograma (usando scipy)
plt.figure(figsize=(12, 8))
Z = linkage(X, method='ward')
dendrogram(Z, leaf_rotation=90., leaf_font_size=8.)
plt.title('Dendrograma - Agrupamento Hierárquico')
plt.xlabel('Amostras')
plt.ylabel('Distância')
plt.axhline(y=1.0, c='k', linestyle='--', alpha=0.5)
plt.tight_layout()

# Avaliando diferentes números de clusters
n_clusters_range = range(2, 8)
silhouette_scores = []

for n_clusters in n_clusters_range:
    modelo = AgglomerativeClustering(n_clusters=n_clusters, linkage='ward')
    cluster_labels = modelo.fit_predict(X)
    silhouette_avg = silhouette_score(X, cluster_labels)
    silhouette_scores.append(silhouette_avg)
    print(f"Para n_clusters = {n_clusters}, o silhouette score é {silhouette_avg:.4f}")

# Plotando o gráfico Silhouette Score vs Número de Clusters
plt.figure(figsize=(10, 6))
plt.plot(n_clusters_range, silhouette_scores, marker='o', linestyle='-')
plt.xlabel('Número de Clusters')
plt.ylabel('Silhouette Score')
plt.title('Silhouette Score para Diferentes Números de Clusters')
plt.grid(True)
```

### PCA

```python
# PCA - Análise de Componentes Principais (detalhada)
from sklearn.decomposition import PCA
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import StandardScaler
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# Carregando o dataset
cancer = load_breast_cancer()
X = cancer.data
y = cancer.target

# Padronizando os dados
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Aplicando PCA
pca = PCA()
X_pca = pca.fit_transform(X_scaled)

# Variância explicada por componente
explained_variance_ratio = pca.explained_variance_ratio_
cumulative_variance_ratio = np.cumsum(explained_variance_ratio)

# Visualizando a variância explicada
plt.figure(figsize=(12, 6))

plt.subplot(1, 2, 1)
plt.bar(range(1, len(explained_variance_ratio) + 1), explained_variance_ratio)
plt.xlabel('Componentes Principais')
plt.ylabel('Variância Explicada')
plt.title('Variância Explicada por Componente')
plt.grid(True)

plt.subplot(1, 2, 2)
plt.plot(range(1, len(cumulative_variance_ratio) + 1), cumulative_variance_ratio, marker='o')
plt.axhline(y=0.9, color='r', linestyle='--', alpha=0.5, label='90% de Variância Explicada')
plt.xlabel('Número de Componentes')
plt.ylabel('Variância Explicada Acumulada')
plt.title('Variância Explicada Acumulada')
plt.grid(True)
plt.legend()

plt.tight_layout()

# Encontrando o número de componentes para explicar 90% da variância
n_components_90 = np.argmax(cumulative_variance_ratio >= 0.9) + 1
print(f"Número de componentes para explicar 90% da variância: {n_components_90}")

# Reduzindo para 2 componentes para visualização
pca_2d = PCA(n_components=2)
X_pca_2d = pca_2d.fit_transform(X_scaled)

# Visualizando as amostras projetadas em 2D
plt.figure(figsize=(10, 8))
for i, target_name in enumerate(['malignant', 'benign']):
    plt.scatter(X_pca_2d[y == i, 0], X_pca_2d[y == i, 1],
                label=target_name, alpha=0.7)
plt.xlabel('Primeira Componente Principal')
plt.ylabel('Segunda Componente Principal')
plt.title('PCA - Projeção 2D do Dataset de Câncer de Mama')
plt.legend()
plt.grid(True)

# Contribuição das features para as primeiras duas componentes
loadings = pca_2d.components_
feature_names = cancer.feature_names

# Criando um DataFrame para visualização
loadings_df = pd.DataFrame(loadings.T, columns=['PC1', 'PC2'], index=feature_names)
print("\nContribuição das features para as duas primeiras componentes principais:")
print(loadings_df.sort_values(by='PC1', ascending=False).head(10))
```

### t-SNE

```python
# t-SNE para visualização de alta dimensionalidade
from sklearn.manifold import TSNE
from sklearn.datasets import load_digits
import matplotlib.pyplot as plt
import numpy as np

# Carregando o dataset de dígitos
digits = load_digits()
X = digits.data
y = digits.target

# Aplicando t-SNE
tsne = TSNE(n_components=2, random_state=42, perplexity=30, learning_rate=200)
X_tsne = tsne.fit_transform(X)

# Criando um grid para visualização
plt.figure(figsize=(12, 10))
plt.scatter(X_tsne[:, 0], X_tsne[:, 1], c=y, cmap='tab10', alpha=0.8, s=50)
plt.colorbar(label='Dígito')
plt.title('Visualização t-SNE dos Dígitos MNIST')
plt.xlabel('t-SNE Componente 1')
plt.ylabel('t-SNE Componente 2')
plt.grid(True)

# Visualizando alguns dígitos em suas posições t-SNE
plt.figure(figsize=(15, 15))
for i, (x, y_) in enumerate(zip(X_tsne, y)):
    if i % 50 == 0:  # Mostrar apenas alguns dígitos para não sobrecarregar
        plt.text(x[0], x[1], str(y_),
                fontdict={'weight': 'bold', 'size': 12},
                bbox=dict(facecolor='white', alpha=0.7))

plt.scatter(X_tsne[:, 0], X_tsne[:, 1], c=y, cmap='tab10', alpha=0.3, s=30)
plt.title('Visualização t-SNE com Rótulos de Dígitos')
plt.grid(True)
```

## Seleção e Validação de Modelos

### Validação Cruzada

```python
# Validação Cruzada
from sklearn.model_selection import cross_val_score, KFold, StratifiedKFold
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
from sklearn.tree import DecisionTreeClassifier
from sklearn.datasets import load_breast_cancer
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Carregando o dataset
cancer = load_breast_cancer()
X = cancer.data
y = cancer.target

# Criando diferentes modelos para comparar
modelos = {
    'Random Forest': RandomForestClassifier(random_state=42),
    'Logistic Regression': LogisticRegression(max_iter=1000, random_state=42),
    'SVM': SVC(kernel='rbf', random_state=42),
    'Decision Tree': DecisionTreeClassifier(random_state=42)
}

# Realizando validação cruzada estratificada com 5 folds
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
resultados = {}

for nome, modelo in modelos.items():
    # Calculando a acurácia e o desvio padrão
    scores = cross_val_score(modelo, X, y, cv=cv, scoring='accuracy')
    resultados[nome] = {
        'Acurácia Média': scores.mean(),
        'Desvio Padrão': scores.std(),
        'Scores Individuais': scores
    }
    print(f"{nome}: {scores.mean():.4f} (±{scores.std():.4f})")

# Visualizando os resultados com boxplot
scores_df = pd.DataFrame()

for nome, res in resultados.items():
    scores_df[nome] = res['Scores Individuais']

plt.figure(figsize=(12, 6))
boxplot = scores_df.boxplot()
plt.title('Comparação de Modelos com Validação Cruzada')
plt.ylabel('Acurácia')
plt.grid(False)
plt.xticks(rotation=45)
plt.tight_layout()

# Avaliação mais detalhada com cross_validate
from sklearn.model_selection import cross_validate

# Métricas a serem avaliadas
metricas = ['accuracy', 'precision', 'recall', 'f1']
modelo_detalhado = RandomForestClassifier(random_state=42)

# Avaliação detalhada com várias métricas
resultado_detalhado = cross_validate(modelo_detalhado, X, y, cv=cv,
                                     scoring=metricas, return_train_score=True)

# Criando um DataFrame para visualização
df_resultados = pd.DataFrame({
    'Treino - Acurácia': resultado_detalhado['train_accuracy'],
    'Validação - Acurácia': resultado_detalhado['test_accuracy'],
    'Treino - Precisão': resultado_detalhado['train_precision'],
    'Validação - Precisão': resultado_detalhado['test_precision'],
    'Treino - Recall': resultado_detalhado['train_recall'],
    'Validação - Recall': resultado_detalhado['test_recall'],
    'Treino - F1': resultado_detalhado['train_f1'],
    'Validação - F1': resultado_detalhado['test_f1']
})

# Resumo das métricas
print("\nMétricas detalhadas para Random Forest:")
print(df_resultados.describe().loc[['mean', 'std']].T)
```

### Grid Search

```python
# Grid Search para otimização de hiperparâmetros
from sklearn.model_selection import GridSearchCV, train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_wine
from sklearn.metrics import classification_report, confusion_matrix
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import numpy as np

# Carregando o dataset
wine = load_wine()
X = wine.data
y = wine.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42)

# Definindo o modelo e os hiperparâmetros a serem testados
modelo = RandomForestClassifier(random_state=42)
param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4]
}

# Criando o grid search
grid_search = GridSearchCV(
    estimator=modelo,
    param_grid=param_grid,
    cv=5,
    scoring='accuracy',
    n_jobs=-1,  # Usar todos os processadores disponíveis
    verbose=1
)

# Treinando o grid search
grid_search.fit(X_train, y_train)

# Melhores parâmetros e resultados
print(f"Melhores parâmetros: {grid_search.best_params_}")
print(f"Melhor score de validação cruzada: {grid_search.best_score_:.4f}")

# Avaliando o melhor modelo no conjunto de teste
melhor_modelo = grid_search.best_estimator_
y_pred = melhor_modelo.predict(X_test)

# Avaliação detalhada
print("\nRelatório de Classificação no Conjunto de Teste:")
print(classification_report(y_test, y_pred, target_names=wine.target_names))

# Matriz de confusão
conf_matrix = confusion_matrix(y_test, y_pred)

plt.figure(figsize=(10, 8))
sns.heatmap(conf_matrix, annot=True, fmt='d', cmap='Blues',
            xticklabels=wine.target_names,
            yticklabels=wine.target_names)
plt.xlabel('Previsão')
plt.ylabel('Valor Real')
plt.title('Matriz de Confusão - Melhor Modelo')

# Resultados da pesquisa em grid
resultados = pd.DataFrame(grid_search.cv_results_)

# Visualizando a importância dos parâmetros
param_importancia = {}

for param in param_grid:
    param_values = resultados[f'param_{param}'].astype(str)
    media_scores = []

    for valor in sorted(param_values.unique()):
        indice = param_values == valor
        media = resultados.loc[indice, 'mean_test_score'].mean()
        media_scores.append(media)

    # Variação dos scores indica importância do parâmetro
    param_importancia[param] = max(media_scores) - min(media_scores)

# Ordenando por importância
param_importancia = {k: v for k, v in sorted(
    param_importancia.items(), key=lambda item: item[1], reverse=True)}

print("\nImportância relativa dos parâmetros:")
for param, imp in param_importancia.items():
    print(f"{param}: {imp:.4f}")
```

### Métricas de Avaliação

```python
# Métricas de Avaliação para Classificação e Regressão
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    roc_curve, roc_auc_score, precision_recall_curve, average_precision_score,
    mean_squared_error, mean_absolute_error, r2_score, confusion_matrix,
    classification_report, matthews_corrcoef
)
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor
from sklearn.datasets import load_breast_cancer, fetch_california_housing
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import pandas as pd

# --- PARTE 1: MÉTRICAS PARA CLASSIFICAÇÃO ---
# Carregando o dataset para classificação
cancer = load_breast_cancer()
X = cancer.data
y = cancer.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)

# Treinando o modelo
clf = RandomForestClassifier(random_state=42)
clf.fit(X_train, y_train)

# Gerando previsões
y_pred = clf.predict(X_test)
y_prob = clf.predict_proba(X_test)[:, 1]  # Probabilidade da classe positiva

# Métricas básicas
acuracia = accuracy_score(y_test, y_pred)
precisao = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
mcc = matthews_corrcoef(y_test, y_pred)  # Coeficiente de correlação Matthews

print("=== Métricas de Classificação ===")
print(f"Acurácia: {acuracia:.4f}")
print(f"Precisão: {precisao:.4f}")
print(f"Recall: {recall:.4f}")
print(f"F1-Score: {f1:.4f}")
print(f"Matthews Correlation Coefficient: {mcc:.4f}")

# Matriz de Confusão
cm = confusion_matrix(y_test, y_pred)
tn, fp, fn, tp = cm.ravel()

print(f"\nMatriz de Confusão:")
print(f"Verdadeiros Negativos (TN): {tn}")
print(f"Falsos Positivos (FP): {fp}")
print(f"Falsos Negativos (FN): {fn}")
print(f"Verdadeiros Positivos (TP): {tp}")

plt.figure(figsize=(8, 6))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Maligno', 'Benigno'],
            yticklabels=['Maligno', 'Benigno'])
plt.xlabel('Previsão')
plt.ylabel('Valor Real')
plt.title('Matriz de Confusão')

# Curva ROC
fpr, tpr, thresholds = roc_curve(y_test, y_prob)
auc = roc_auc_score(y_test, y_prob)

plt.figure(figsize=(8, 6))
plt.plot(fpr, tpr, color='blue', lw=2, label=f'ROC curve (AUC = {auc:.3f})')
plt.plot([0, 1], [0, 1], color='gray', linestyle='--')
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.05])
plt.xlabel('Taxa de Falsos Positivos')
plt.ylabel('Taxa de Verdadeiros Positivos')
plt.title('Curva ROC')
plt.legend(loc="lower right")
plt.grid(True)

# Curva Precision-Recall
precision, recall, _ = precision_recall_curve(y_test, y_prob)
ap = average_precision_score(y_test, y_prob)

plt.figure(figsize=(8, 6))
plt.plot(recall, precision, color='green', lw=2,
         label=f'Precision-Recall curve (AP = {ap:.3f})')
plt.xlabel('Recall')
plt.ylabel('Precision')
plt.title('Curva Precision-Recall')
plt.legend(loc="lower left")
plt.grid(True)

# --- PARTE 2: MÉTRICAS PARA REGRESSÃO ---
# Carregando o dataset para regressão
housing = fetch_california_housing()
X = housing.data
y = housing.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)

# Treinando o modelo de regressão
reg = RandomForestRegressor(random_state=42)
reg.fit(X_train, y_train)

# Gerando previsões
y_pred_reg = reg.predict(X_test)

# Métricas de regressão
mse = mean_squared_error(y_test, y_pred_reg)
rmse = np.sqrt(mse)
mae = mean_absolute_error(y_test, y_pred_reg)
r2 = r2_score(y_test, y_pred_reg)

print("\n=== Métricas de Regressão ===")
print(f"Erro Quadrático Médio (MSE): {mse:.4f}")
print(f"Raiz do Erro Quadrático Médio (RMSE): {rmse:.4f}")
print(f"Erro Absoluto Médio (MAE): {mae:.4f}")
print(f"Coeficiente de Determinação (R²): {r2:.4f}")

# Visualização dos valores previstos vs reais
plt.figure(figsize=(10, 6))
plt.scatter(y_test, y_pred_reg, alpha=0.5)
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--')
plt.xlabel('Valores Reais')
plt.ylabel('Previsões')
plt.title('Previsões vs Valores Reais')
plt.grid(True)

# Visualização dos resíduos
residuos = y_test - y_pred_reg

plt.figure(figsize=(12, 5))

# Histograma dos resíduos
plt.subplot(1, 2, 1)
plt.hist(residuos, bins=30, color='skyblue', edgecolor='black')
plt.xlabel('Resíduos')
plt.ylabel('Frequência')
plt.title('Histograma dos Resíduos')
plt.grid(True)

# Gráfico de dispersão dos resíduos
plt.subplot(1, 2, 2)
plt.scatter(y_pred_reg, residuos, alpha=0.5)
plt.axhline(y=0, color='r', linestyle='--')
plt.xlabel('Valores Previstos')
plt.ylabel('Resíduos')
plt.title('Resíduos vs Valores Previstos')
plt.grid(True)

plt.tight_layout()
```

## Ajuste de Hiperparâmetros

Os hiperparâmetros são parâmetros que não são aprendidos pelo modelo durante o treinamento, mas definidos antes do treinamento. O ajuste adequado desses parâmetros é crucial para o desempenho do modelo.

### O que são Hiperparâmetros?

Diferentemente dos parâmetros regulares do modelo (como os coeficientes em uma regressão), os hiperparâmetros controlam o processo de aprendizagem em si:

- Em árvores de decisão: profundidade máxima, número mínimo de amostras para divisão
- Em SVM: parâmetro de regularização C, tipo de kernel
- Em redes neurais: taxa de aprendizado, número de camadas ocultas

### Técnicas de Otimização de Hiperparâmetros

#### Grid Search

O `GridSearchCV` testa todas as combinações possíveis de valores de hiperparâmetros definidos:

```python
from sklearn.model_selection import GridSearchCV
from sklearn.ensemble import RandomForestClassifier

param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10]
}

rf = RandomForestClassifier(random_state=42)
grid_search = GridSearchCV(
    estimator=rf,
    param_grid=param_grid,
    cv=5,
    scoring='accuracy',
    n_jobs=-1,
    verbose=2
)

grid_search.fit(X_train, y_train)

# Melhores parâmetros e score
print("Melhores parâmetros:", grid_search.best_params_)
print("Melhor score:", grid_search.best_score_)

# Usar o melhor modelo
best_model = grid_search.best_estimator_
```

#### Random Search

O `RandomizedSearchCV` testa um número fixo de combinações aleatórias, o que pode ser mais eficiente quando o espaço de parâmetros é grande:

```python
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import randint, uniform

param_distributions = {
    'n_estimators': randint(100, 500),
    'max_depth': randint(10, 50),
    'min_samples_split': randint(2, 15),
    'min_samples_leaf': randint(1, 10)
}

rf = RandomForestClassifier(random_state=42)
random_search = RandomizedSearchCV(
    estimator=rf,
    param_distributions=param_distributions,
    n_iter=100,  # número de combinações a testar
    cv=5,
    scoring='accuracy',
    n_jobs=-1,
    random_state=42,
    verbose=2
)

random_search.fit(X_train, y_train)
```

#### Bayesian Optimization

Embora não nativa do scikit-learn, é possível usar bibliotecas como `scikit-optimize`, `hyperopt` ou `optuna` para otimização bayesiana, que é mais eficiente do que Grid Search e Random Search:

```python
from skopt import BayesSearchCV
from skopt.space import Real, Integer

search_spaces = {
    'n_estimators': Integer(100, 500),
    'max_depth': Integer(10, 50),
    'min_samples_split': Integer(2, 15),
    'min_samples_leaf': Integer(1, 10)
}

rf = RandomForestClassifier(random_state=42)
bayes_search = BayesSearchCV(
    estimator=rf,
    search_spaces=search_spaces,
    n_iter=50,
    cv=5,
    scoring='accuracy',
    n_jobs=-1,
    random_state=42,
    verbose=2
)

bayes_search.fit(X_train, y_train)
```

### Avaliando Resultados da Otimização

Após executar a busca de hiperparâmetros, é importante analisar os resultados:

```python
# Visualizar resultados do GridSearchCV
import pandas as pd
import matplotlib.pyplot as plt

results = pd.DataFrame(grid_search.cv_results_)
params = [f'param_{p}' for p in param_grid.keys()]
cols = params + ['mean_test_score', 'rank_test_score']

# Exibir os N melhores resultados
display(results[cols].sort_values('rank_test_score').head(10))

# Visualizar efeito de hiperparâmetros específicos
plt.figure(figsize=(12, 8))
for max_depth in param_grid['max_depth']:
    subset = results[results['param_max_depth'] == max_depth]
    plt.plot(
        subset['param_n_estimators'],
        subset['mean_test_score'],
        'o-',
        label=f'max_depth={max_depth}'
    )

plt.xlabel('n_estimators')
plt.ylabel('Mean CV Score')
plt.title('Efeito dos Hiperparâmetros no Desempenho do Modelo')
plt.legend()
plt.grid(True)
plt.show()
```

### Hiperparâmetros Comuns por Algoritmo

#### Árvores de Decisão e Random Forest

- `max_depth`: Profundidade máxima da árvore
- `min_samples_split`: Número mínimo de amostras necessárias para dividir um nó
- `min_samples_leaf`: Número mínimo de amostras em um nó folha
- `n_estimators` (apenas Random Forest): Número de árvores
- `max_features`: Número de features a considerar em cada divisão

#### SVM

- `C`: Parâmetro de regularização
- `kernel`: Tipo de kernel ('linear', 'poly', 'rbf', 'sigmoid')
- `gamma`: Coeficiente do kernel para 'rbf', 'poly' e 'sigmoid'

#### KNN

- `n_neighbors`: Número de vizinhos
- `weights`: Função de peso ('uniform', 'distance')
- `metric`: Métrica de distância ('euclidean', 'manhattan', etc.)

#### Gradient Boosting

- `learning_rate`: Taxa de aprendizado
- `n_estimators`: Número de estimadores
- `max_depth`: Profundidade máxima
- `subsample`: Fração de amostras usadas para treinar cada estimador
- `colsample_bytree`: Fração de features usadas para treinar cada estimador

## Trabalho com Dados Desbalanceados

Dados desbalanceados são comuns em problemas de classificação, especialmente em detecção de fraudes, diagnósticos médicos e outros casos onde uma classe é muito mais rara que as outras.

### Identificação de Desbalanceamento

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Verificar distribuição das classes
class_counts = pd.Series(y_train).value_counts()
print(class_counts)

# Visualizar a distribuição
plt.figure(figsize=(10, 6))
class_counts.plot(kind='bar')
plt.title('Distribuição das Classes')
plt.xlabel('Classe')
plt.ylabel('Contagem')
plt.grid(axis='y')
plt.show()

# Calcular razão de desbalanceamento
imbalance_ratio = class_counts.max() / class_counts.min()
print(f"Razão de desbalanceamento: {imbalance_ratio:.2f}")
```

### Técnicas de Reamostragem

#### Undersampling (Subamostragem)

Reduz o número de exemplos da classe majoritária:

```python
from imblearn.under_sampling import RandomUnderSampler, NearMiss

# Random Undersampling
rus = RandomUnderSampler(random_state=42)
X_rus, y_rus = rus.fit_resample(X_train, y_train)

# NearMiss (baseado em vizinhança)
nm = NearMiss(version=1)
X_nm, y_nm = nm.fit_resample(X_train, y_train)

print(f"Distribuição original: {pd.Series(y_train).value_counts()}")
print(f"Após Random Undersampling: {pd.Series(y_rus).value_counts()}")
print(f"Após NearMiss: {pd.Series(y_nm).value_counts()}")
```

#### Oversampling (Sobreamostragem)

Aumenta o número de exemplos da classe minoritária:

```python
from imblearn.over_sampling import RandomOverSampler, SMOTE, ADASYN

# Random Oversampling
ros = RandomOverSampler(random_state=42)
X_ros, y_ros = ros.fit_resample(X_train, y_train)

# SMOTE (Synthetic Minority Over-sampling Technique)
smote = SMOTE(random_state=42)
X_smote, y_smote = smote.fit_resample(X_train, y_train)

# ADASYN (Adaptive Synthetic Sampling)
adasyn = ADASYN(random_state=42)
X_adasyn, y_adasyn = adasyn.fit_resample(X_train, y_train)

print(f"Após Random Oversampling: {pd.Series(y_ros).value_counts()}")
print(f"Após SMOTE: {pd.Series(y_smote).value_counts()}")
print(f"Após ADASYN: {pd.Series(y_adasyn).value_counts()}")
```

#### Combinação de Under e Oversampling

```python
from imblearn.combine import SMOTETomek, SMOTEENN

# SMOTETomek
smote_tomek = SMOTETomek(random_state=42)
X_st, y_st = smote_tomek.fit_resample(X_train, y_train)

# SMOTEENN
smote_enn = SMOTEENN(random_state=42)
X_se, y_se = smote_enn.fit_resample(X_train, y_train)

print(f"Após SMOTETomek: {pd.Series(y_st).value_counts()}")
print(f"Após SMOTEENN: {pd.Series(y_se).value_counts()}")
```

### Usar Class Weights

Muitos algoritmos no scikit-learn suportam o parâmetro `class_weight`:

```python
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier

# Logistic Regression com balanceamento de classes
log_reg_balanced = LogisticRegression(class_weight='balanced', random_state=42)
log_reg_balanced.fit(X_train, y_train)

# Random Forest com balanceamento de classes
rf_balanced = RandomForestClassifier(class_weight='balanced', random_state=42)
rf_balanced.fit(X_train, y_train)

# Especificar pesos manualmente
class_weights = {0: 1, 1: 10}  # Classe 1 tem peso 10x maior
rf_custom_weights = RandomForestClassifier(class_weight=class_weights, random_state=42)
rf_custom_weights.fit(X_train, y_train)
```

### Avaliação com Métricas Apropriadas

Para dados desbalanceados, a acurácia pode ser enganosa:

```python
from sklearn.metrics import (
    confusion_matrix, classification_report,
    precision_recall_curve, average_precision_score,
    roc_curve, roc_auc_score
)

# Prever probabilidades
y_pred_proba = model.predict_proba(X_test)[:, 1]
y_pred = model.predict(X_test)

# Confusion Matrix
cm = confusion_matrix(y_test, y_pred)
print("Matriz de Confusão:")
print(cm)

# Classification Report
print("Relatório de Classificação:")
print(classification_report(y_test, y_pred))

# Precision-Recall Curve (melhor que ROC para dados muito desbalanceados)
precision, recall, thresholds = precision_recall_curve(y_test, y_pred_proba)
ap_score = average_precision_score(y_test, y_pred_proba)

plt.figure(figsize=(10, 8))
plt.plot(recall, precision, marker='.', label=f'AP = {ap_score:.3f}')
plt.xlabel('Recall')
plt.ylabel('Precision')
plt.title('Curva Precision-Recall')
plt.legend()
plt.grid(True)
plt.show()

# ROC Curve
fpr, tpr, _ = roc_curve(y_test, y_pred_proba)
auc = roc_auc_score(y_test, y_pred_proba)

plt.figure(figsize=(10, 8))
plt.plot(fpr, tpr, marker='.', label=f'AUC = {auc:.3f}')
plt.plot([0, 1], [0, 1], 'k--')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Curva ROC')
plt.legend()
plt.grid(True)
plt.show()
```

### Ajuste do Limiar de Classificação

Ajustar o limiar de classificação pode melhorar o desempenho em classes minoritárias:

```python
from sklearn.metrics import f1_score

# Encontrar o melhor limiar com base no F1-score
thresholds = np.arange(0.1, 0.9, 0.05)
f1_scores = []

for threshold in thresholds:
    y_pred_threshold = (y_pred_proba >= threshold).astype(int)
    f1 = f1_score(y_test, y_pred_threshold)
    f1_scores.append(f1)

best_threshold_idx = np.argmax(f1_scores)
best_threshold = thresholds[best_threshold_idx]
best_f1 = f1_scores[best_threshold_idx]

print(f"Melhor limiar: {best_threshold:.2f} com F1-score: {best_f1:.3f}")

# Usar o melhor limiar
final_predictions = (y_pred_proba >= best_threshold).astype(int)
print(classification_report(y_test, final_predictions))
```

### Pipeline de Balanceamento Completo

```python
from imblearn.pipeline import Pipeline as ImbPipeline
from sklearn.preprocessing import StandardScaler

# Pipeline completo com balanceamento
pipeline = ImbPipeline([
    ('scaler', StandardScaler()),
    ('sampler', SMOTE(random_state=42)),
    ('classifier', RandomForestClassifier(random_state=42))
])

# Grid Search com o pipeline
param_grid = {
    'classifier__n_estimators': [100, 200],
    'classifier__max_depth': [None, 10, 20],
    'sampler__k_neighbors': [5, 10]  # Parâmetro do SMOTE
}

grid_search = GridSearchCV(
    pipeline, param_grid, cv=5, scoring='f1', n_jobs=-1
)
grid_search.fit(X_train, y_train)

print("Melhores parâmetros:", grid_search.best_params_)
```

## Exemplos Práticos Completos

### Classificação Binária: Previsão de Diabetes

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix
from sklearn.pipeline import Pipeline
from sklearn.model_selection import GridSearchCV

# Carregar dados (exemplo com o conjunto de dados de diabetes)
url = "https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv"
column_names = ['Pregnancies', 'Glucose', 'BloodPressure', 'SkinThickness',
                'Insulin', 'BMI', 'DiabetesPedigreeFunction', 'Age', 'Outcome']
data = pd.read_csv(url, names=column_names)

# Separar features e target
X = data.drop('Outcome', axis=1)
y = data['Outcome']

# Divisão em treinamento e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)

# Criar pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('classifier', RandomForestClassifier(random_state=42))
])

# Definir parâmetros para grid search
param_grid = {
    'classifier__n_estimators': [100, 200],
    'classifier__max_depth': [None, 10, 20],
    'classifier__min_samples_split': [2, 5],
    'classifier__class_weight': [None, 'balanced']
}

# Executar grid search
grid_search = GridSearchCV(
    pipeline, param_grid, cv=5, scoring='f1', n_jobs=-1, verbose=1
)
grid_search.fit(X_train, y_train)

# Melhor modelo
best_model = grid_search.best_estimator_
print("Melhores parâmetros:", grid_search.best_params_)

# Avaliar no conjunto de teste
y_pred = best_model.predict(X_test)
print("\nClassification Report:")
print(classification_report(y_test, y_pred))

print("\nConfusion Matrix:")
print(confusion_matrix(y_test, y_pred))

# Importância das features
if hasattr(best_model['classifier'], 'feature_importances_'):
    importances = best_model['classifier'].feature_importances_
    indices = np.argsort(importances)[::-1]

    print("\nImportância das Features:")
    for i, idx in enumerate(indices):
        print(f"{i+1}. {X.columns[idx]} ({importances[idx]:.4f})")
```

### Regressão: Previsão de Preços de Imóveis

```python
from sklearn.datasets import fetch_california_housing
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.pipeline import Pipeline
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import randint, uniform

# Carregar o conjunto de dados
housing = fetch_california_housing()
X = pd.DataFrame(housing.data, columns=housing.feature_names)
y = housing.target

# Divisão em treinamento e teste
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Criar pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('regressor', GradientBoostingRegressor(random_state=42))
])

# Definir distribuições para random search
param_distributions = {
    'regressor__n_estimators': randint(100, 500),
    'regressor__learning_rate': uniform(0.01, 0.2),
    'regressor__max_depth': randint(3, 10),
    'regressor__min_samples_split': randint(2, 20),
    'regressor__subsample': uniform(0.6, 0.4)
}

# Executar random search
random_search = RandomizedSearchCV(
    pipeline, param_distributions, n_iter=50, cv=5,
    scoring='neg_mean_squared_error', n_jobs=-1, random_state=42, verbose=1
)
random_search.fit(X_train, y_train)

# Melhor modelo
best_model = random_search.best_estimator_
print("Melhores parâmetros:", random_search.best_params_)

# Avaliar no conjunto de teste
y_pred = best_model.predict(X_test)
mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = r2_score(y_test, y_pred)

print(f"\nPerformance no conjunto de teste:")
print(f"MSE: {mse:.4f}")
print(f"RMSE: {rmse:.4f}")
print(f"R²: {r2:.4f}")

# Visualizar resultados
plt.figure(figsize=(10, 6))
plt.scatter(y_test, y_pred, alpha=0.5)
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--')
plt.xlabel('Valor Real')
plt.ylabel('Previsão')
plt.title('Previsões vs. Valores Reais')
plt.grid(True)
plt.show()

# Análise de resíduos
residuals = y_test - y_pred
plt.figure(figsize=(10, 6))
plt.scatter(y_pred, residuals, alpha=0.5)
plt.axhline(y=0, color='r', linestyle='--')
plt.xlabel('Previsão')
plt.ylabel('Resíduo')
plt.title('Análise de Resíduos')
plt.grid(True)
plt.show()
```

### Clustering: Segmentação de Clientes

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.cluster import KMeans, DBSCAN
from sklearn.metrics import silhouette_score

# Gerar dados sintéticos para este exemplo
np.random.seed(42)
n_samples = 1000

# Features sintéticas de clientes
data = {
    'Recencia': np.random.exponential(30, n_samples),
    'Frequencia': np.random.poisson(5, n_samples),
    'MonetarioTotal': np.random.exponential(100, n_samples),
    'TempoCliente': np.random.gamma(2, 2, n_samples) * 10
}

df = pd.DataFrame(data)

# Criar alguns segmentos mais óbvios
for i in range(200):
    df.loc[i, 'Recencia'] *= 0.5
    df.loc[i, 'Frequencia'] *= 2
    df.loc[i, 'MonetarioTotal'] *= 2

for i in range(200, 400):
    df.loc[i, 'Recencia'] *= 2
    df.loc[i, 'Frequencia'] *= 0.5
    df.loc[i, 'MonetarioTotal'] *= 0.7

# Padronizar os dados
scaler = StandardScaler()
df_scaled = scaler.fit_transform(df)
df_scaled = pd.DataFrame(df_scaled, columns=df.columns)

# Determinar o número ideal de clusters para K-Means
wcss = []
silhouette_scores = []
range_clusters = range(2, 11)

for i in range_clusters:
    kmeans = KMeans(n_clusters=i, random_state=42, n_init=10)
    kmeans.fit(df_scaled)
    wcss.append(kmeans.inertia_)

    labels = kmeans.labels_
    silhouette_avg = silhouette_score(df_scaled, labels)
    silhouette_scores.append(silhouette_avg)

# Visualizar o "cotovelo" no gráfico WCSS
plt.figure(figsize=(16, 6))
plt.subplot(1, 2, 1)
plt.plot(range_clusters, wcss, marker='o')
plt.title('Método do Cotovelo')
plt.xlabel('Número de Clusters')
plt.ylabel('WCSS')
plt.grid(True)

# Visualizar o silhouette score
plt.subplot(1, 2, 2)
plt.plot(range_clusters, silhouette_scores, marker='o')
plt.title('Silhouette Score')
plt.xlabel('Número de Clusters')
plt.ylabel('Silhouette Score')
plt.grid(True)
plt.tight_layout()
plt.show()

# Aplicar K-Means com o número ideal de clusters
n_clusters = 4  # Com base na análise de cotovelo e silhouette
kmeans = KMeans(n_clusters=n_clusters, random_state=42, n_init=10)
cluster_labels = kmeans.fit_predict(df_scaled)

# Adicionar rótulos de cluster ao dataframe original
df['Cluster'] = cluster_labels

# Aplicar PCA para visualização em 2D
pca = PCA(n_components=2)
principal_components = pca.fit_transform(df_scaled)
pca_df = pd.DataFrame(data=principal_components, columns=['PC1', 'PC2'])
pca_df['Cluster'] = cluster_labels

# Visualizar clusters
plt.figure(figsize=(10, 8))
for cluster in range(n_clusters):
    subset = pca_df[pca_df['Cluster'] == cluster]
    plt.scatter(subset['PC1'], subset['PC2'], label=f'Cluster {cluster}')

plt.title('Visualização de Clusters com PCA')
plt.xlabel('Componente Principal 1')
plt.ylabel('Componente Principal 2')
plt.legend()
plt.grid(True)
plt.show()

# Análise dos clusters
cluster_analysis = df.groupby('Cluster').mean()
print("Perfil dos clusters:")
print(cluster_analysis)

# Visualizar características dos clusters
plt.figure(figsize=(14, 8))
for i, feature in enumerate(df.columns[:-1]):
    plt.subplot(2, 2, i+1)
    for cluster in range(n_clusters):
        subset = df[df['Cluster'] == cluster]
        plt.hist(subset[feature], alpha=0.5, bins=20, label=f'Cluster {cluster}')
    plt.title(f'Distribuição de {feature} por Cluster')
    plt.xlabel(feature)
    plt.ylabel('Frequência')
    plt.legend()
plt.tight_layout()
plt.show()

# DBSCAN como alternativa ao K-Means
dbscan = DBSCAN(eps=0.5, min_samples=10)
dbscan_labels = dbscan.fit_predict(df_scaled)

# Adicionar rótulos DBSCAN
df['DBSCAN_Cluster'] = dbscan_labels

# Visualizar clusters DBSCAN
pca_df['DBSCAN_Cluster'] = dbscan_labels
plt.figure(figsize=(10, 8))
unique_labels = np.unique(dbscan_labels)

for label in unique_labels:
    if label == -1:
        color = 'black'  # ruído em preto
        marker = 'x'
        label_name = 'Noise'
    else:
        color = plt.cm.jet(label / max(1, len(unique_labels) - 1))
        marker = 'o'
        label_name = f'Cluster {label}'

    subset = pca_df[pca_df['DBSCAN_Cluster'] == label]
    plt.scatter(subset['PC1'], subset['PC2'], c=[color], marker=marker, label=label_name)

plt.title('Visualização de Clusters DBSCAN com PCA')
plt.xlabel('Componente Principal 1')
plt.ylabel('Componente Principal 2')
plt.legend()
plt.grid(True)
plt.show()

# Quantidade de amostras em cada cluster
print("Contagem de amostras por cluster (K-Means):")
print(df['Cluster'].value_counts())

print("\nContagem de amostras por cluster (DBSCAN):")
print(df['DBSCAN_Cluster'].value_counts())
```

## Dicas e Boas Práticas

### Melhorando a Performance

#### Paralelização

Muitos algoritmos do scikit-learn suportam computação paralela:

```python
# Usando múltiplos cores
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(n_jobs=-1)  # -1 usa todos os cores disponíveis
```

#### Redução de Dimensionalidade

```python
from sklearn.decomposition import PCA
from sklearn.feature_selection import SelectKBest, f_classif

# PCA
pca = PCA(n_components=0.95)  # mantém 95% da variância
X_pca = pca.fit_transform(X_scaled)

# Seleção univariada de features
selector = SelectKBest(f_classif, k=10)  # seleciona as 10 melhores features
X_selected = selector.fit_transform(X, y)

# Ver quais features foram selecionadas
selected_features = X.columns[selector.get_support()]
```

#### Feature Engineering Eficiente

```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import FeatureUnion

# Criar features polinomiais
poly = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly.fit_transform(X)

# Combinar diferentes conjuntos de features
feature_union = FeatureUnion([
    ('pca', PCA(n_components=10)),
    ('select_k_best', SelectKBest(f_classif, k=10))
])

X_combined = feature_union.fit_transform(X, y)
```

### Evitando Overfitting

#### Validação Cruzada Robusta

```python
from sklearn.model_selection import cross_val_score, RepeatedStratifiedKFold

# Validação cruzada repetida
cv = RepeatedStratifiedKFold(n_splits=5, n_repeats=3, random_state=42)
scores = cross_val_score(model, X, y, cv=cv, scoring='accuracy')

print(f"Acurácia média: {scores.mean():.4f}")
print(f"Desvio padrão: {scores.std():.4f}")
print(f"Intervalo de confiança: [{scores.mean() - 2*scores.std():.4f}, {scores.mean() + 2*scores.std():.4f}]")
```

#### Regularização

```python
from sklearn.linear_model import Ridge, Lasso, ElasticNet

# Ridge (regularização L2)
ridge = Ridge(alpha=1.0)

# Lasso (regularização L1)
lasso = Lasso(alpha=0.1)

# ElasticNet (combinação de L1 e L2)
elastic = ElasticNet(alpha=0.1, l1_ratio=0.5)
```

#### Early Stopping

```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import train_test_split

# Divisão em treino, validação e teste
X_train_full, X_test, y_train_full, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
X_train, X_val, y_train, y_val = train_test_split(X_train_full, y_train_full, test_size=0.25, random_state=42)

# Treinamento com early stopping
gb = GradientBoostingClassifier(
    n_estimators=1000,
    learning_rate=0.1,
    random_state=42,
    validation_fraction=0.2,
    n_iter_no_change=10,
    tol=0.01
)

gb.fit(X_train, y_train)
print(f"Número de estimadores utilizados: {gb.n_estimators_}")
```

### Tratamento de Dados de Alta Dimensionalidade

```python
from sklearn.decomposition import TruncatedSVD
from sklearn.random_projection import GaussianRandomProjection

# Para dados esparsos, TruncatedSVD é mais eficiente que PCA
svd = TruncatedSVD(n_components=100)
X_svd = svd.fit_transform(X_sparse)

# Projeção aleatória
random_projection = GaussianRandomProjection(n_components=100, random_state=42)
X_projected = random_projection.fit_transform(X)
```

### Manipulação de Conjuntos de Dados Grandes

```python
import numpy as np
from sklearn.feature_extraction.text import HashingVectorizer
from sklearn.linear_model import SGDClassifier

# Use HashingVectorizer para dados grandes
vectorizer = HashingVectorizer(n_features=2**18, alternate_sign=False)
X_hashed = vectorizer.transform(text_data)

# Use aprendizado incremental
sgd = SGDClassifier(loss='log_loss', random_state=42)

# Treinamento em mini-lotes
batch_size = 1000
n_batches = (len(X_train) // batch_size) + 1

for batch in range(n_batches):
    start = batch * batch_size
    end = min((batch + 1) * batch_size, len(X_train))

    if end <= start:
        continue

    X_batch = X_train[start:end]
    y_batch = y_train[start:end]

    sgd.partial_fit(X_batch, y_batch, classes=np.unique(y_train))
```

### Interpretabilidade de Modelos

```python
import matplotlib.pyplot as plt
import numpy as np
from sklearn.inspection import permutation_importance
from sklearn.tree import plot_tree

# Importância de features para Random Forest
feature_importance = model.feature_importances_
sorted_idx = np.argsort(feature_importance)
plt.figure(figsize=(12, 8))
plt.barh(range(len(sorted_idx)), feature_importance[sorted_idx], align='center')
plt.yticks(range(len(sorted_idx)), np.array(X.columns)[sorted_idx])
plt.title('Importância das Features')
plt.tight_layout()
plt.show()

# Permutation Importance (menos enviesada que feature_importances_)
result = permutation_importance(
    model, X_test, y_test, n_repeats=10, random_state=42, n_jobs=-1
)
sorted_idx = result.importances_mean.argsort()
plt.figure(figsize=(12, 8))
plt.boxplot(
    result.importances[sorted_idx].T,
    vert=False,
    labels=np.array(X.columns)[sorted_idx]
)
plt.title("Permutation Importances")
plt.tight_layout()
plt.show()

# Visualizar árvore de decisão
plt.figure(figsize=(20, 10))
tree_in_forest = model.estimators_[0]
plot_tree(
    tree_in_forest,
    feature_names=X.columns,
    class_names=[str(c) for c in model.classes_],
    filled=True,
    rounded=True,
    max_depth=3
)
plt.show()

# Partial Dependence Plots
from sklearn.inspection import partial_dependence, PartialDependenceDisplay

features = [0, 1]  # Índices das features para visualizar
PartialDependenceDisplay.from_estimator(
    model, X_train, features,
    feature_names=X.columns,
    kind="both"
)
plt.tight_layout()
plt.show()
```

### Monitoramento e Logging

```python
from sklearn.model_selection import learning_curve
import matplotlib.pyplot as plt
import numpy as np
import time
import json
import os

# Learning curves para detectar overfitting
train_sizes, train_scores, valid_scores = learning_curve(
    model, X, y, train_sizes=np.linspace(0.1, 1.0, 10),
    cv=5, scoring='accuracy', n_jobs=-1
)

plt.figure(figsize=(10, 6))
plt.plot(train_sizes, train_scores.mean(axis=1), 'o-', label='Treino')
plt.plot(train_sizes, valid_scores.mean(axis=1), 'o-', label='Validação')
plt.xlabel('Tamanho do conjunto de treinamento')
plt.ylabel('Acurácia')
plt.title('Learning Curve')
plt.legend()
plt.grid(True)
plt.show()

# Logging de experimentos
experiment_log = {
    'timestamp': time.strftime('%Y-%m-%d %H:%M:%S'),
    'model': str(model),
    'parameters': model.get_params(),
    'metrics': {
        'accuracy': accuracy,
        'precision': precision,
        'recall': recall,
        'f1': f1
    },
    'feature_importance': feature_importance.tolist() if hasattr(model, 'feature_importances_') else None
}

# Salvar log em JSON
os.makedirs('logs', exist_ok=True)
with open(f'logs/experiment_{time.strftime("%Y%m%d_%H%M%S")}.json', 'w') as f:
    json.dump(experiment_log, f, indent=4)
```

### Deployment de Modelos

```python
import pickle
import joblib
from sklearn.pipeline import Pipeline

# Salvar modelo usando pickle
with open('model.pkl', 'wb') as f:
    pickle.dump(model, f)

# Ou usando joblib (melhor para objetos grandes)
joblib.dump(model, 'model.joblib')

# Salvar pipeline completo
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('pca', PCA(n_components=10)),
    ('model', RandomForestClassifier(random_state=42))
])

pipeline.fit(X_train, y_train)
joblib.dump(pipeline, 'pipeline.joblib')

# Carregar modelo
loaded_model = joblib.load('model.joblib')
y_pred = loaded_model.predict(X_test)
```

### Dicas para Aumento de Performance

1. **Use dados em formato NumPy ou pandas**: Scikit-learn é otimizado para trabalhar com arrays NumPy.

2. **Reduza dimensionalidade antes de algoritmos complexos**: Aplicar PCA ou seleção de features antes de algoritmos como SVM pode melhorar muito a performance.

3. **Teste diferentes algoritmos**: Comece com algoritmos mais simples como Naive Bayes ou Árvores de Decisão antes de partir para modelos mais complexos.

4. **Use streaming/mini-batch para grandes conjuntos de dados**: Algoritmos como SGD suportam treinamento parcial.

5. **Aprenda os parâmetros mais importantes**: Cada algoritmo tem parâmetros críticos que afetam muito mais seu desempenho do que outros.

6. **Use pipelines**: Eles garantem que as transformações de dados sejam aplicadas consistentemente nos conjuntos de treino e teste.

7. **Automatize a busca de hiperparâmetros**: Mas use `cv`, `n_jobs` e outros parâmetros de paralelização para otimizar o tempo de execução.

8. **Equilibre acurácia e velocidade**: Nem sempre o modelo mais complexo é o melhor para seu caso de uso.

9. **Avalie modelos corretamente**: Use técnicas apropriadas de validação cruzada e métricas adequadas ao problema.

10. **Otimize para predição quando necessário**: Para modelos em produção, considere quantização, poda ou outras técnicas de otimização.

## Recursos Adicionais

### Documentação Oficial e Tutoriais

- [Documentação oficial do Scikit-learn](https://scikit-learn.org/stable/documentation.html)
- [Tutoriais em scikit-learn.org](https://scikit-learn.org/stable/tutorial/index.html)
- [Exemplos da galeria](https://scikit-learn.org/stable/auto_examples/index.html)
- [FAQ do Scikit-learn](https://scikit-learn.org/stable/faq.html)

### Livros Recomendados

1. "Introduction to Machine Learning with Python" por Andreas C. Müller e Sarah Guido
2. "Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow" por Aurélien Géron
3. "Python Machine Learning" por Sebastian Raschka
4. "Mastering Machine Learning with scikit-learn" por Gavin Hackeling
5. "Feature Engineering for Machine Learning" por Alice Zheng e Amanda Casari

### Cursos Online e MOOCs

1. Coursera: "Machine Learning with Python" pelo IBM
2. Udemy: "Python for Data Science and Machine Learning Bootcamp"
3. DataCamp: "Scikit-learn for Machine Learning in Python"
4. edX: "Machine Learning with Python: from Linear Models to Deep Learning"

### Comunidades e Fóruns

1. [Stack Overflow tags: scikit-learn](https://stackoverflow.com/questions/tagged/scikit-learn)
2. [Scikit-learn GitHub Issues](https://github.com/scikit-learn/scikit-learn/issues)
3. [Comunidade Data Science Stack Exchange](https://datascience.stackexchange.com/)
4. [Reddit r/MachineLearning](https://www.reddit.com/r/MachineLearning/)

### Extensões e Bibliotecas Complementares

1. **imbalanced-learn**: Extensão para tratar dados desbalanceados
2. **scikit-optimize**: Otimização bayesiana para hiperparâmetros
3. **yellowbrick**: Visualização de modelos de ML
4. **LIME e SHAP**: Interpretabilidade de modelos
5. **scikit-image**: Processamento de imagens
6. **scikit-multilearn**: Aprendizado multi-label
7. **scikit-survival**: Análise de sobrevivência
8. **bayesian-optimization**: Otimização bayesiana para hiperparâmetros

### Datasets para Prática

1. [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php)
2. [Kaggle Datasets](https://www.kaggle.com/datasets)
3. [Google Dataset Search](https://datasetsearch.research.google.com/)
4. [Awesome Public Datasets](https://github.com/awesomedata/awesome-public-datasets)
5. [Datasets integrados ao scikit-learn](https://scikit-learn.org/stable/datasets.html)

### Guias de Referência Rápida

1. [Scikit-learn Cheat Sheet](https://scikit-learn.org/stable/tutorial/machine_learning_map/index.html)
2. [Python Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/)
3. [Machine Learning Algorithm Cheat Sheet da Microsoft](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-cheat-sheet)

### Blogs e Newsletters

1. [Blog oficial do Scikit-learn](https://scikit-learn.org/stable/news.html)
2. [Towards Data Science](https://towardsdatascience.com/)
3. [KDnuggets](https://www.kdnuggets.com/)
4. [Data Science Weekly Newsletter](https://www.datascienceweekly.org/)
5. [Practical Business Python](https://pbpython.com/)

### Repositórios GitHub com Exemplos

1. [Scikit-learn Cookbook](https://github.com/PacktPublishing/scikit-learn-Cookbook)
2. [Machine Learning with Python Examples](https://github.com/tirthajyoti/Machine-Learning-with-Python)
3. [Scikit-learn Video Tutorial](https://github.com/justmarkham/scikit-learn-videos)
4. [scikit-learn Lightning Tutorial](https://github.com/scikit-learn-contrib/lightning)

### Ferramentas de Visualização

1. **Matplotlib**: Visualização básica
2. **Seaborn**: Visualização estatística
3. **Plotly**: Gráficos interativos
4. **Bokeh**: Visualizações interativas para web
5. **Altair**: Visualização declarativa
6. **Yellowbrick**: Visualização específica para ML
7. **ELI5**: Explicabilidade para modelos de ML
8. **SHAP**: Explicabilidade utilizando valores Shapley

## Conclusão

O scikit-learn é uma biblioteca robusta e versátil para aprendizado de máquina em Python, fornecendo implementações eficientes de uma ampla gama de algoritmos de ML. Com esta documentação completa, você tem as ferramentas necessárias para aplicar o scikit-learn em diversos problemas de análise de dados e aprendizado de máquina, desde a preparação dos dados até a implantação de modelos.

Lembre-se de que a prática constante e a experimentação são fundamentais para dominar o uso eficaz desta poderosa biblioteca. À medida que você ganha experiência, poderá combinar técnicas diferentes para criar soluções cada vez mais sofisticadas para seus desafios de dados.

Para ficar atualizado com as últimas funcionalidades e melhores práticas, recomendamos consultar regularmente a documentação oficial e participar das comunidades de usuários do scikit-learn e aprendizado de máquina em geral.
