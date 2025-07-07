# [⬅ Voltar para o índice principal](../../../README.md)

# Guia Completo de Pandas em Python

## Sumário

1. [Introdução ao Pandas](#introdução-ao-pandas)
2. [Instalação](#instalação)
3. [Estruturas de Dados Fundamentais](#estruturas-de-dados-fundamentais)
   - [Series](#series)
   - [DataFrame](#dataframe)
4. [Importando Dados](#importando-dados)
5. [Explorando Dados](#explorando-dados)
6. [Seleção e Indexação de Dados](#seleção-e-indexação-de-dados)
7. [Filtragem de Dados](#filtragem-de-dados)
8. [Manipulação de Dados](#manipulação-de-dados)
9. [Tratamento de Dados Ausentes](#tratamento-de-dados-ausentes)
10. [Operações de Grupo e Agregação](#operações-de-grupo-e-agregação)
11. [Mesclagem e Junção de DataFrames](#mesclagem-e-junção-de-dataframes)
12. [Pivotagem e Reformatação de Dados](#pivotagem-e-reformatação-de-dados)
13. [Visualização de Dados com Pandas](#visualização-de-dados-com-pandas)
14. [Funções de Data e Hora](#funções-de-data-e-hora)
15. [Otimização de Performance](#otimização-de-performance)
16. [Casos de Uso Avançados](#casos-de-uso-avançados)
17. [Dicas e Boas Práticas](#dicas-e-boas-práticas)
18. [Conclusão](#conclusão)

## Introdução ao Pandas

Pandas é uma biblioteca Python de código aberto que fornece estruturas de dados de alto desempenho e ferramentas de análise de dados. Foi desenvolvida por Wes McKinney em 2008 e se tornou fundamental para cientistas de dados, analistas e desenvolvedores que trabalham com dados.

### Por que usar Pandas?

- Manipulação intuitiva de dados tabulares e séries temporais
- Alto desempenho com processamento rápido
- Operações complexas com sintaxe simples e expressiva
- Integração com outras bibliotecas do ecossistema Python

```python
# Importando a biblioteca Pandas
import pandas as pd
import numpy as np  # NumPy geralmente é usado junto com Pandas

# Verificando a versão instalada
print(f"Versão do Pandas: {pd.__version__}")
```

## Instalação

A instalação do Pandas é simples usando o gerenciador de pacotes pip:

```python
# Usando pip
pip install pandas

# Usando conda
conda install pandas
```

## Estruturas de Dados Fundamentais

O Pandas possui duas estruturas de dados principais: Series e DataFrame.

### Series

Uma Series é uma matriz unidimensional rotulada capaz de armazenar qualquer tipo de dado.

```python
# Criando uma Series a partir de uma lista
numeros = pd.Series([10, 20, 30, 40, 50])
print("Series simples:")
print(numeros)
print("\n")

# Criando uma Series com índices personalizados
frutas = pd.Series([3, 5, 7, 2], index=['Maçãs', 'Bananas', 'Laranjas', 'Peras'])
print("Series com índices personalizados:")
print(frutas)
print("\n")

# Criando uma Series a partir de um dicionário
populacao = pd.Series({'São Paulo': 12.33, 'Rio de Janeiro': 6.75, 'Belo Horizonte': 2.72})
print("Series a partir de um dicionário:")
print(populacao)
print("\n")

# Acessando elementos de uma Series
print(f"Valor na posição 0 da Series numeros: {numeros[0]}")
print(f"Valor para 'Bananas' na Series frutas: {frutas['Bananas']}")

# ----------------------------------------------

# Criando uma Series simples
s = pd.Series([1, 3, 5, np.nan, 6, 8])
print(s)
# Saída:
# 0    1.0
# 1    3.0
# 2    5.0
# 3    NaN
# 4    6.0
# 5    8.0
# dtype: float64

# Series com índices personalizados
s = pd.Series([10, 20, 30, 40], index=['a', 'b', 'c', 'd'])
print(s)
# Saída:
# a    10
# b    20
# c    30
# d    40
# dtype: int64

# Criando uma Series a partir de um dicionário
d = {'a': 100, 'b': 200, 'c': 300}
s = pd.Series(d)
print(s)
# Saída:
# a    100
# b    200
# c    300
# dtype: int64
```

### DataFrame

Um DataFrame é uma estrutura de dados bidimensional semelhante a uma tabela, com linhas e colunas rotuladas.

```python
# Criando um DataFrame a partir de um dicionário
dados = {
    'Nome': ['Ana', 'Bruno', 'Carolina', 'Daniel', 'Elena'],
    'Idade': [25, 32, 19, 45, 37],
    'Cidade': ['São Paulo', 'Rio de Janeiro', 'Belo Horizonte', 'Curitiba', 'Salvador'],
    'Salário': [3500, 4200, 2800, 5100, 4800]
}

df = pd.DataFrame(dados)
print("DataFrame básico:")
print(df)
print("\n")

# Criando um DataFrame a partir de uma lista de dicionários
lista_dados = [
    {'nome': 'Ana', 'nota': 8.5},
    {'nome': 'Bruno', 'nota': 7.8},
    {'nome': 'Carolina', 'nota': 9.2}
]

df_notas = pd.DataFrame(lista_dados)
print("DataFrame a partir de lista de dicionários:")
print(df_notas)
print("\n")

# Criando um DataFrame a partir de um array NumPy
array = np.random.randint(0, 100, size=(5, 4))
df_array = pd.DataFrame(array, columns=['A', 'B', 'C', 'D'])
print("DataFrame a partir de array NumPy:")
print(df_array)
print("\n")

# Informações sobre o DataFrame
print("Informações do DataFrame 'df':")
print(f"Forma (linhas, colunas): {df.shape}")
print(f"Dimensões: {df.ndim}")
print(f"Tamanho: {df.size}")
print(f"Colunas: {df.columns.tolist()}")
print(f"Tipos de dados:\n{df.dtypes}")

# ----------------------------------------------

# Criando um DataFrame a partir de um dicionário
data = {
    'Nome': ['João', 'Maria', 'Pedro', 'Ana'],
    'Idade': [25, 30, 22, 28],
    'Cidade': ['São Paulo', 'Rio de Janeiro', 'Belo Horizonte', 'Curitiba']
}

df = pd.DataFrame(data)
print(df)
# Saída:
#     Nome  Idade         Cidade
# 0   João     25      São Paulo
# 1  Maria     30  Rio de Janeiro
# 2  Pedro     22  Belo Horizonte
# 3    Ana     28        Curitiba

# DataFrame com índices personalizados
df = pd.DataFrame(data, index=['p1', 'p2', 'p3', 'p4'])
print(df)
# Saída:
#      Nome  Idade         Cidade
# p1   João     25      São Paulo
# p2  Maria     30  Rio de Janeiro
# p3  Pedro     22  Belo Horizonte
# p4    Ana     28        Curitiba

# Criando um DataFrame a partir de um array NumPy
array = np.random.randn(3, 4)  # Cria matriz 3x4 de números aleatórios
df = pd.DataFrame(array, columns=list('ABCD'))
print(df)
# Saída:
#           A         B         C         D
# 0 -0.321384  0.804380 -0.178798 -0.694934
# 1 -0.528698 -0.421384  0.276145  0.266262
# 2 -0.021497  0.227031 -1.085756  1.098184
```

## Importando Dados

Pandas facilita a importação de dados de vários formatos:

```python
# Importando de CSV (comentado pois o arquivo não existe neste exemplo)
# df_csv = pd.read_csv('dados.csv')

# Criando um arquivo CSV de exemplo
df.to_csv('exemplo.csv', index=False)
df_csv = pd.read_csv('exemplo.csv')
print("DataFrame lido de CSV:")
print(df_csv)
print("\n")

# Importando de Excel (comentado pois o arquivo não existe neste exemplo)
# df_excel = pd.read_excel('dados.xlsx', sheet_name='Sheet1')

# Importando de JSON (criando um exemplo)
df.to_json('exemplo.json', orient='records')
df_json = pd.read_json('exemplo.json')
print("DataFrame lido de JSON:")
print(df_json)
print("\n")

# Importando de SQL (requer conexão com banco de dados)
# from sqlalchemy import create_engine
# engine = create_engine('sqlite:///banco_de_dados.db')
# df_sql = pd.read_sql('SELECT * FROM tabela', engine)

# Importando de HTML (exemplo com tabela da Wikipedia)
# url = 'https://pt.wikipedia.org/wiki/Lista_de_pa%C3%ADses_por_popula%C3%A7%C3%A3o'
# tabelas_html = pd.read_html(url)
# df_html = tabelas_html[0]  # Primeira tabela encontrada na página
```

## Explorando Dados

Pandas oferece métodos para explorar e entender rapidamente seus dados:

```python
# Criando um DataFrame maior para exemplo
np.random.seed(42)  # Para reprodutibilidade
dados_grandes = {
    'ID': range(1, 1001),
    'Valor': np.random.normal(100, 25, 1000),
    'Categoria': np.random.choice(['A', 'B', 'C', 'D'], 1000),
    'Data': pd.date_range(start='2023-01-01', periods=1000),
    'Status': np.random.choice([True, False], 1000)
}

df_grande = pd.DataFrame(dados_grandes)

# Visualizando as primeiras linhas
print("Primeiras 5 linhas:")
print(df_grande.head())
# Saída:
#    ID      Valor Categoria       Data  Status
# 0   1  112.450712         A 2023-01-01    True
# 1   2   97.926035         D 2023-01-02    True
# 2   3  118.377125         A 2023-01-03   False
# 3   4  143.491440         A 2023-01-04    True
# 4   5   85.384625         B 2023-01-05   False


# Visualizando as últimas linhas
print("Últimas 3 linhas:")
print(df_grande.tail(3))
# Saída:
#        ID      Valor Categoria       Data  Status
# 997   998  112.626085         A 2025-09-24    True
# 998   999   88.417091         C 2025-09-25    True
# 999  1000  104.297722         B 2025-09-26    True


# Amostra aleatória de linhas
print("Amostra aleatória de 5 linhas:")
print(df_grande.sample(5))
# Saída (exemplo, pode variar):
#        ID      Valor Categoria       Data  Status
# 963   964   95.233430         A 2025-08-25   False
# 292   293  127.552538         A 2023-10-21    True
# 14     15  100.429017         A 2023-01-15   False
# 156   157  102.264296         C 2023-06-07    True
# 90     91   88.446323         A 2023-04-02    True


# Estatísticas descritivas
print("Estatísticas descritivas:")
print(df_grande.describe())
# Saída:
#                ID       Valor
# count  1000.000000  1000.000000
# mean    500.500000   99.647226
# std     288.819436   24.823286
# min       1.000000   25.239902
# 25%     250.750000   82.507421
# 50%     500.500000   99.799139
# 75%     750.250000  116.693168
# max    1000.000000  169.100735


# Informações sobre o DataFrame
print("Informações do DataFrame:")
print(df_grande.info())
# Saída:
# <class 'pandas.core.frame.DataFrame'>
# RangeIndex: 1000 entries, 0 to 999
# Data columns (total 5 columns):
#  #   Column     Non-Null Count  Dtype
# ---  ------     --------------  -----
#  0   ID         1000 non-null   int64
#  1   Valor      1000 non-null   float64
#  2   Categoria  1000 non-null   object
#  3   Data       1000 non-null   datetime64[ns]
#  4   Status     1000 non-null   bool
# dtypes: bool(1), datetime64 , float64(1), int64(1), object(1)
# memory usage: 33.0+ KB


# Verificando valores únicos em uma coluna
print("Valores únicos na coluna 'Categoria':")
print(df_grande['Categoria'].unique())
# Saída:
# ['A' 'D' 'B' 'C']

print(f"Contagem de valores únicos: {df_grande['Categoria'].nunique()}")
# Saída:
# Contagem de valores únicos: 4


# Contagem de valores
print("Contagem de valores na coluna 'Categoria':")
print(df_grande['Categoria'].value_counts())
# Saída (pode variar, exemplo):
# A    253
# D    251
# C    248
# B    248
# Name: Categoria, dtype: int64


# Verificando correlações
print("Matriz de correlação:")
print(df_grande.corr())
# Saída:
#             ID     Valor
# ID     1.000000  0.024922
# Valor  0.024922  1.000000

```

## Seleção e Indexação de Dados

Há várias maneiras de selecionar dados específicos em um DataFrame:

```python
# Criando um DataFrame de exemplo
dados = {
    'Nome': ['Ana', 'Bruno', 'Carolina', 'Daniel', 'Elena'],
    'Idade': [25, 32, 19, 45, 37],
    'Cidade': ['São Paulo', 'Rio de Janeiro', 'Belo Horizonte', 'Curitiba', 'Salvador'],
    'Salário': [3500, 4200, 2800, 5100, 4800]
}
df = pd.DataFrame(dados)

# Selecionando uma coluna (retorna uma Series)
print("Coluna 'Nome':")
print(df['Nome'])
# Saída:
# 0        Ana
# 1      Bruno
# 2    Carolina
# 3     Daniel
# 4      Elena
# Name: Nome, dtype: object

# Selecionando múltiplas colunas (retorna um DataFrame)
print("Colunas 'Nome' e 'Idade':")
print(df[['Nome', 'Idade']])
# Saída:
#       Nome  Idade
# 0      Ana     25
# 1    Bruno     32
# 2  Carolina     19
# 3   Daniel     45
# 4    Elena     37

# Selecionando linhas por posição com loc (baseado em rótulos)
print("Linhas 1 a 3 (inclusivo) com .loc:")
print(df.loc[1:3])
# Saída:
#      Nome  Idade         Cidade  Salário
# 1   Bruno     32  Rio de Janeiro     4200
# 2 Carolina     19  Belo Horizonte     2800
# 3  Daniel     45         Curitiba     5100

# Selecionando linhas por posição com iloc (baseado em índices inteiros)
print("Linhas 1 a 3 (exclusivo para o último) com .iloc:")
print(df.iloc[1:3])
# Saída:
#      Nome  Idade         Cidade  Salário
# 1   Bruno     32  Rio de Janeiro     4200
# 2 Carolina     19  Belo Horizonte     2800

# Selecionando linhas e colunas específicas com loc
print("Selecionando linhas 1 a 3 e colunas 'Nome' e 'Salário' com .loc:")
print(df.loc[1:3, ['Nome', 'Salário']])
# Saída:
#       Nome  Salário
# 1    Bruno     4200
# 2  Carolina     2800
# 3   Daniel     5100

# Selecionando linhas e colunas específicas com iloc
print("Selecionando linhas 1 a 3 e colunas 0 e 3 com .iloc:")
print(df.iloc[1:3, [0, 3]])
# Saída:
#       Nome  Salário
# 1    Bruno     4200
# 2  Carolina     2800

# Usando o método .at para acessar um valor específico
print(f"Valor na linha 2, coluna 'Idade': {df.at[2, 'Idade']}")
# Saída:
# Valor na linha 2, coluna 'Idade': 19

# Usando o método .iat para acessar um valor por posição
print(f"Valor na terceira linha, segunda coluna: {df.iat[2, 1]}")
# Saída:
# Valor na terceira linha, segunda coluna: 19

```

## Filtragem de Dados

Podemos filtrar dados com base em condições:

```python
# Criando um DataFrame de exemplo
dados = {
    'Nome': ['Ana', 'Bruno', 'Carolina', 'Daniel', 'Elena', 'Fernando'],
    'Idade': [25, 32, 19, 45, 37, 29],
    'Cidade': ['São Paulo', 'Rio de Janeiro', 'São Paulo', 'Curitiba', 'Salvador', 'Rio de Janeiro'],
    'Salário': [3500, 4200, 2800, 5100, 4800, 3900],
    'Departamento': ['Vendas', 'TI', 'Marketing', 'TI', 'Vendas', 'Marketing']
}
df = pd.DataFrame(dados)

# Filtrando por uma condição
print("Pessoas com mais de 30 anos:")
print(df[df['Idade'] > 30])
# Saída:
#      Nome  Idade         Cidade  Salário Departamento
# 1   Bruno     32  Rio de Janeiro     4200           TI
# 3  Daniel     45         Curitiba     5100           TI
# 4   Elena     37         Salvador     4800       Vendas

# Filtrando por múltiplas condições com operador &
print("Pessoas com mais de 30 anos E que ganham mais de 4000:")
print(df[(df['Idade'] > 30) & (df['Salário'] > 4000)])
# Saída:
#     Nome  Idade         Cidade  Salário Departamento
# 3  Daniel     45         Curitiba     5100           TI
# 4   Elena     37         Salvador     4800       Vendas

# Filtrando por múltiplas condições com operador |
print("Pessoas que moram em São Paulo OU trabalham em Vendas:")
print(df[(df['Cidade'] == 'São Paulo') | (df['Departamento'] == 'Vendas')])
# Saída:
#       Nome  Idade       Cidade  Salário Departamento
# 0      Ana     25    São Paulo     3500       Vendas
# 2  Carolina     19    São Paulo     2800    Marketing
# 4     Elena     37     Salvador     4800       Vendas

# Usando o método .query() para filtrar
print("Pessoas de TI com salário acima de 4000 usando .query():")
print(df.query("Departamento == 'TI' and Salário > 4000"))
# Saída:
#     Nome  Idade   Cidade  Salário Departamento
# 3  Daniel     45  Curitiba     5100           TI

# Filtrando com .isin()
cidades_interesse = ['São Paulo', 'Rio de Janeiro']
print(f"Pessoas que moram em {cidades_interesse}:")
print(df[df['Cidade'].isin(cidades_interesse)])
# Saída:
#       Nome  Idade         Cidade  Salário Departamento
# 0      Ana     25      São Paulo     3500       Vendas
# 1    Bruno     32  Rio de Janeiro     4200           TI
# 2  Carolina     19      São Paulo     2800    Marketing
# 5  Fernando     29  Rio de Janeiro     3900    Marketing

# Filtrando com .str.contains() para buscar substring
print("Pessoas cujo nome contém 'ana':")
print(df[df['Nome'].str.contains('ana')])
# Saída (case sensitive):
#       Nome  Idade      Cidade  Salário Departamento
# 0     Ana     25    São Paulo     3500       Vendas
# 5  Fernando     29  Rio de Janeiro     3900    Marketing

# Usando .between() para verificar se está dentro de um intervalo
print("Pessoas com idade entre 25 e 35 anos:")
print(df[df['Idade'].between(25, 35)])
# Saída:
#       Nome  Idade         Cidade  Salário Departamento
# 0      Ana     25      São Paulo     3500       Vendas
# 1    Bruno     32  Rio de Janeiro     4200           TI
# 5  Fernando     29  Rio de Janeiro     3900    Marketing

```

## Manipulação de Dados

Pandas oferece muitas funções para manipular e transformar dados:

```python
# Criando um DataFrame de exemplo
dados = {
    'Nome': ['Ana Silva', 'Bruno Oliveira', 'Carolina Santos', 'Daniel Lima', 'Elena Costa'],
    'Idade': [25, 32, 19, 45, 37],
    'Email': ['ana.silva@exemplo.com', 'bruno.o@exemplo.com', 'carol@exemplo.com', 'daniel@exemplo.com', 'elena@exemplo.com'],
    'Salário': [3500, 4200, 2800, 5100, 4800]
}
df = pd.DataFrame(dados)

# Adicionando uma nova coluna baseada em colunas existentes
df['Salário Anual'] = df['Salário'] * 13  # Considerando o 13º salário
print("DataFrame com coluna calculada:")
print(df)
# Saída:
#              Nome  Idade                Email  Salário  Salário Anual
# 0        Ana Silva     25  ana.silva@exemplo.com     3500           45500
# 1   Bruno Oliveira     32   bruno.o@exemplo.com     4200           54600
# 2  Carolina Santos     19     carol@exemplo.com     2800           36400
# 3      Daniel Lima     45    daniel@exemplo.com     5100           66300
# 4      Elena Costa     37     elena@exemplo.com     4800           62400

# Usando apply() para aplicar uma função a cada elemento
df['Nome Maiúsculo'] = df['Nome'].apply(lambda x: x.upper())
print("Aplicando função com apply():")
print(df[['Nome', 'Nome Maiúsculo']])
# Saída:
#              Nome     Nome Maiúsculo
# 0        Ana Silva         ANA SILVA
# 1   Bruno Oliveira   BRUNO OLIVEIRA
# 2  Carolina Santos  CAROLINA SANTOS
# 3      Daniel Lima      DANIEL LIMA
# 4      Elena Costa      ELENA COSTA

# Aplicando função em várias colunas com applymap()
df_sub = df[['Nome', 'Email']]
df_sub = df_sub.applymap(lambda x: x[:5] + '...' if isinstance(x, str) else x)
print("Usando applymap() para truncar strings:")
print(df_sub)
# Saída:
#     Nome    Email
# 0  Ana S...  ana.s...
# 1  Bruno...  bruno...
# 2  Carol...  carol...
# 3  Danie...  danie...
# 4  Elena...  elena...

# Criando colunas a partir da divisão de uma coluna
df[['Primeiro Nome', 'Sobrenome']] = df['Nome'].str.split(' ', expand=True)
print("Dividindo a coluna 'Nome':")
print(df[['Nome', 'Primeiro Nome', 'Sobrenome']])
# Saída:
#              Nome Primeiro Nome  Sobrenome
# 0        Ana Silva          Ana      Silva
# 1   Bruno Oliveira        Bruno   Oliveira
# 2  Carolina Santos     Carolina     Santos
# 3      Daniel Lima        Daniel      Lima
# 4      Elena Costa         Elena     Costa

# Extraindo informações com métodos string
df['Domínio Email'] = df['Email'].str.split('@').str[1]
print("Extraindo domínio do email:")
print(df[['Email', 'Domínio Email']])
# Saída:
#                  Email      Domínio Email
# 0  ana.silva@exemplo.com     exemplo.com
# 1   bruno.o@exemplo.com     exemplo.com
# 2     carol@exemplo.com     exemplo.com
# 3    daniel@exemplo.com     exemplo.com
# 4     elena@exemplo.com     exemplo.com

# Transformando com replace (usando pd.cut para categorizar)
df['Faixa Etária'] = pd.cut(df['Idade'],
                          bins=[0, 25, 35, 50],
                          labels=['Jovem', 'Adulto', 'Sênior'])
print("Categorizando idades:")
print(df[['Nome', 'Idade', 'Faixa Etária']])
# Saída:
#              Nome  Idade Faixa Etária
# 0        Ana Silva     25       Jovem
# 1   Bruno Oliveira     32      Adulto
# 2  Carolina Santos     19       Jovem
# 3      Daniel Lima     45      Sênior
# 4      Elena Costa     37      Sênior

# Renomeando colunas
df = df.rename(columns={'Salário': 'Salário Mensal', 'Idade': 'Idade (anos)'})
print("Colunas renomeadas:")
print(df.columns.tolist())
# Saída:
# ['Nome', 'Idade (anos)', 'Email', 'Salário Mensal', 'Salário Anual', 'Nome Maiúsculo', 'Primeiro Nome', 'Sobrenome', 'Domínio Email', 'Faixa Etária']

# Reordenando colunas
colunas_ordenadas = ['Nome', 'Primeiro Nome', 'Sobrenome', 'Idade (anos)', 'Salário Mensal', 'Salário Anual']
df_ordenado = df[colunas_ordenadas]
print("DataFrame com colunas reordenadas:")
print(df_ordenado.head())
# Saída:
#              Nome Primeiro Nome  Sobrenome  Idade (anos)  Salário Mensal  Salário Anual
# 0        Ana Silva          Ana      Silva            25             3500          45500
# 1   Bruno Oliveira        Bruno   Oliveira            32             4200          54600
# 2  Carolina Santos     Carolina     Santos            19             2800          36400
# 3      Daniel Lima        Daniel      Lima            45             5100          66300
# 4      Elena Costa         Elena     Costa            37             4800          62400

```

## Tratamento de Dados Ausentes

O tratamento adequado de valores ausentes (NaN) é crucial na análise de dados:

```python
# Criando um DataFrame com valores ausentes (NaN)
dados = {
    'Nome': ['Ana', 'Bruno', 'Carolina', None, 'Elena'],
    'Idade': [25, 32, None, 45, 37],
    'Cidade': ['São Paulo', None, 'Belo Horizonte', 'Curitiba', None],
    'Salário': [3500, 4200, 2800, None, 4800]
}
df = pd.DataFrame(dados)

# Exibindo o DataFrame original com valores ausentes
print("DataFrame com valores ausentes:")
print(df)
# Saída: DataFrame com alguns campos como None (equivalente a NaN no pandas)

# Verificando quantos valores ausentes existem por coluna
# .isnull() retorna um DataFrame booleano e .sum() conta os True (NaN)
print("Valores ausentes por coluna:")
print(df.isnull().sum())
# Saída:
# Nome       1
# Idade      1
# Cidade     2
# Salário    1

# Visualizando a presença de valores ausentes (True indica NaN)
# Útil para entender onde estão os dados faltantes
print("Mapa de valores ausentes (True significa ausente):")
print(df.isnull())
# Saída: DataFrame booleano indicando a posição dos valores ausentes

# Filtrando as linhas que têm pelo menos um valor ausente
# .any(axis=1) verifica por linha se há algum NaN
print("Linhas com pelo menos um valor ausente:")
print(df[df.isnull().any(axis=1)])
# Saída: apenas as linhas que têm ao menos um campo com NaN

# Filtrando apenas as linhas completas (sem nenhum NaN)
# .dropna() remove linhas com qualquer valor ausente
print("Linhas sem valores ausentes:")
print(df.dropna())
# Saída: apenas as linhas que possuem todos os campos preenchidos

# Removendo colunas com mais de um valor ausente
# .dropna(axis=1, thresh=N) mantém colunas com ao menos N valores não-nulos
limite = 1
print(f"DataFrame após remover colunas com mais de {limite} valores ausentes:")
print(df.dropna(axis=1, thresh=len(df) - limite))
# Saída: colunas onde o número de valores não-nulos ≥ len(df) - limite

# Preenchendo valores ausentes com dados definidos para cada coluna
# Exemplo: média para idade, mediana para salário, etc.
print("Preenchendo valores ausentes por coluna:")
print(df.fillna({
    'Nome': 'Desconhecido',               # Nome padrão
    'Idade': df['Idade'].mean(),          # Média: (25 + 32 + 45 + 37) / 4 = 34.75
    'Cidade': 'Não informada',            # Cidade genérica
    'Salário': df['Salário'].median()     # Mediana: 4200
}))
# Saída: DataFrame com todos os NaNs substituídos por valores definidos

# Preenchendo valores ausentes com o valor anterior na linha (forward fill)
# Útil em séries temporais, onde o dado anterior pode ser um bom substituto
print("Preenchendo com o valor anterior (forward fill):")
print(df.fillna(method='ffill'))
# Saída: NaNs substituídos com o valor da linha anterior (acima)

# Preenchendo com o valor seguinte (backward fill)
# Útil quando o próximo valor faz mais sentido
print("Preenchendo com o valor posterior (backward fill):")
print(df.fillna(method='bfill'))
# Saída: NaNs substituídos com o valor da linha seguinte (abaixo)


# Interpolando valores numéricos ausentes (como em gráficos)
# O Pandas calcula valores intermediários automaticamente
print("Interpolando valores numéricos:")
print(df.interpolate())
# Saída: apenas os valores numéricos são preenchidos com base na interpolação linear

```

## Operações de Grupo e Agregação

As operações de grupo são muito poderosas para análise de dados:

```python
# Criando um DataFrame com informações de funcionários
dados = {
    'Nome': ['Ana', 'Bruno', 'Carolina', 'Daniel', 'Elena', 'Fernando', 'Gabriela', 'Henrique'],
    'Departamento': ['Vendas', 'TI', 'Marketing', 'TI', 'Vendas', 'Marketing', 'Vendas', 'TI'],
    'Idade': [28, 34, 25, 42, 31, 29, 35, 27],
    'Salário': [3800, 5200, 3200, 6100, 4200, 3900, 4100, 4800],
    'Anos_Empresa': [3, 7, 1, 10, 5, 2, 6, 3]
}

df = pd.DataFrame(dados)

# Visualizando o DataFrame completo
print("DataFrame de funcionários:")
print(df)
# Saída:
#       Nome Departamento  Idade  Salário  Anos_Empresa
# 0      Ana       Vendas     28     3800             3
# 1    Bruno           TI     34     5200             7
# ...

# Agrupando por 'Departamento' e aplicando múltiplas estatísticas
print("Estatísticas por departamento:")
print(df.groupby('Departamento').agg({
    'Salário': ['mean', 'min', 'max', 'sum'],   # Salário médio, mínimo, máximo e soma
    'Idade': ['mean', 'min', 'max'],            # Idade média, mínima e máxima
    'Anos_Empresa': 'mean'                      # Tempo médio de empresa
}))
# Saída (exemplo):
#                 Salário                   Idade               Anos_Empresa
#                    mean   min   max    sum   mean min max         mean
# Departamento
# Marketing          3550  3200  3900  7100    27.0  25  29           1.5
# TI                 5366  4800  6100 16100    34.3  27  42           6.7
# Vendas             4033  3800  4200 12100    31.3  28  35           4.7

# Criando faixas etárias para análise cruzada
# pd.cut divide os valores de 'Idade' em intervalos (bins)
categorias_idade = pd.cut(df['Idade'], bins=[20, 30, 40, 50], labels=['20-30', '30-40', '40-50'])
df['Faixa_Etária'] = categorias_idade

# Agrupando por Departamento e Faixa Etária, calculando salário médio
print("Salário médio por departamento e faixa etária:")
print(df.groupby(['Departamento', 'Faixa_Etária'])['Salário'].mean())
# Saída (exemplo):
# Departamento  Faixa_Etária
# Marketing     20-30            3550.0
# TI            20-30            4800.0
#               30-40            5200.0
#               40-50            6100.0
# Vendas        20-30            3800.0
#               30-40            4150.0

# Definindo função personalizada para calcular amplitude (diferença entre maior e menor valor)
def amplitude(x):
    return x.max() - x.min()

# Usando funções personalizadas no groupby
print("Agregações personalizadas:")
print(df.groupby('Departamento').agg({
    'Salário': [
        amplitude,                                  # Amplitude de salário
        lambda x: x.nlargest(2).mean()              # Média dos 2 maiores salários
    ],
    'Idade': 'nunique'                              # Quantas idades distintas
}))
# Saída (exemplo):
#              Salário                    Idade
#             amplitude top2_media_maior  nunique
# Departamento
# Marketing      700.0        3550.0          2
# TI            1300.0        5650.0          3
# Vendas         400.0        4150.0          3

# Usando transform para aplicar uma agregação mas manter o mesmo formato do DataFrame
df['Salário_Médio_Depto'] = df.groupby('Departamento')['Salário'].transform('mean')

print("DataFrame com coluna de salário médio por departamento:")
print(df[['Nome', 'Departamento', 'Salário', 'Salário_Médio_Depto']])
# Saída:
#       Nome Departamento  Salário  Salário_Médio_Depto
# 0      Ana       Vendas     3800               4033.33
# 1    Bruno           TI     5200               5366.66
# ...

# Usando filter para retornar apenas os grupos que satisfazem uma condição
# Neste caso, departamentos com salário médio acima de 4500
def tem_salario_alto(grupo):
    return grupo['Salário'].mean() > 4500

print("Departamentos com salário médio acima de 4500:")
print(df.groupby('Departamento').filter(tem_salario_alto))
# Saída (linhas dos departamentos TI e talvez Vendas, dependendo dos dados)

# Usando pivot_table para criar uma tabela de resumo (similar a tabela dinâmica do Excel)
print("Tabela pivô de salário médio por departamento e faixa etária:")
pivot = pd.pivot_table(
    df,
    values='Salário',
    index='Departamento',
    columns='Faixa_Etária',
    aggfunc='mean',
    fill_value=0  # Substitui NaN por zero para faixas sem dados
)
print(pivot)
# Saída (exemplo):
# Faixa_Etária   20-30   30-40   40-50
# Departamento
# Marketing       3550       0       0
# TI              4800    5200    6100
# Vendas          3800    4150       0

```

## Mesclagem e Junção de DataFrames

Pandas permite combinar DataFrames de diferentes maneiras:

```python
import pandas as pd

# Criando DataFrames para demonstração
funcionarios = pd.DataFrame({
    'ID': [1, 2, 3, 4, 5],
    'Nome': ['Ana', 'Bruno', 'Carolina', 'Daniel', 'Elena'],
    'Departamento_ID': [101, 103, 102, 103, 101]
})

departamentos = pd.DataFrame({
    'Departamento_ID': [101, 102, 103, 104],
    'Nome_Departamento': ['Vendas', 'Marketing', 'TI', 'RH'],
    'Localização': ['Andar 1', 'Andar 2', 'Andar 3', 'Andar 1']
})

salarios = pd.DataFrame({
    'ID': [1, 2, 3, 5, 6],  # Note que não há ID 4 e há ID 6 que não existe em funcionarios
    'Salário_Base': [3500, 4200, 2800, 4800, 3600],
    'Bônus': [500, 1000, 300, 1200, 600]
})

print("DataFrame de funcionários:")
print(funcionarios)
# DataFrame de funcionários:
#    ID      Nome  Departamento_ID
# 0   1       Ana              101
# 1   2     Bruno              103
# 2   3  Carolina              102
# 3   4    Daniel              103
# 4   5     Elena              101

print("\nDataFrame de departamentos:")
print(departamentos)
# DataFrame de departamentos:
#    Departamento_ID Nome_Departamento Localização
# 0              101            Vendas     Andar 1
# 1              102         Marketing     Andar 2
# 2              103                TI     Andar 3
# 3              104                RH     Andar 1

print("\nDataFrame de salários:")
print(salarios)
# DataFrame de salários:
#    ID  Salário_Base  Bônus
# 0   1          3500    500
# 1   2          4200   1000
# 2   3          2800    300
# 3   5          4800   1200
# 4   6          3600    600

# Inner join: apenas registros com correspondência em ambos os DataFrames
inner_join = pd.merge(funcionarios, departamentos, on='Departamento_ID', how='inner')
print("Inner join de funcionários e departamentos:")
print(inner_join)
# Saída:
#    ID      Nome  Departamento_ID Nome_Departamento Localização
# 0   1       Ana              101           Vendas     Andar 1
# 1   2     Bruno              103               TI     Andar 3
# 2   3  Carolina              102        Marketing     Andar 2
# 3   4    Daniel              103               TI     Andar 3
# 4   5     Elena              101           Vendas     Andar 1

# Left join: mantém todos os funcionários, mesmo sem salário correspondente
left_join = pd.merge(funcionarios, salarios, on='ID', how='left')
print("\nLeft join de funcionários e salários:")
print(left_join)
# Saída:
#    ID      Nome  Departamento_ID  Salário_Base   Bônus
# 0   1       Ana              101        3500.0   500.0
# 1   2     Bruno              103        4200.0  1000.0
# 2   3  Carolina              102        2800.0   300.0
# 3   4    Daniel              103           NaN     NaN
# 4   5     Elena              101        4800.0  1200.0

# Right join: mantém todos os registros de salários, mesmo que sem funcionário correspondente
right_join = pd.merge(funcionarios, salarios, on='ID', how='right')
print("\nRight join de funcionários e salários:")
print(right_join)
# Saída:
#    ID      Nome  Departamento_ID  Salário_Base  Bônus
# 0   1       Ana            101.0        3500.0   500
# 1   2     Bruno            103.0        4200.0  1000
# 2   3  Carolina            102.0        2800.0   300
# 3   5     Elena            101.0        4800.0  1200
# 4   6       NaN              NaN        3600.0   600

# Outer join: união total das duas tabelas
outer_join = pd.merge(funcionarios, salarios, on='ID', how='outer')
print("\nOuter join de funcionários e salários:")
print(outer_join)
# Saída:
#    ID      Nome  Departamento_ID  Salário_Base  Bônus
# 0   1       Ana            101.0        3500.0   500.0
# 1   2     Bruno            103.0        4200.0  1000.0
# 2   3  Carolina            102.0        2800.0   300.0
# 3   4    Daniel            103.0           NaN     NaN
# 4   5     Elena            101.0        4800.0  1200.0
# 5   6       NaN              NaN        3600.0   600.0

# Merge com colunas de mesmo nome — usando sufixos para evitar conflito
funcionarios2 = pd.DataFrame({
    'ID': [1, 2, 3, 4, 5],
    'Nome': ['Ana Silva', 'Bruno Oliveira', 'Carolina Santos', 'Daniel Lima', 'Elena Costa'],
    'Departamento': ['Vendas', 'TI', 'Marketing', 'TI', 'Vendas']
})

avaliacao = pd.DataFrame({
    'ID': [1, 2, 3, 5, 6],
    'Nome': ['Ana S.', 'Bruno O.', 'Carolina S.', 'Elena C.', 'Fernando M.'],
    'Avaliação': [4.5, 4.2, 3.9, 4.8, 4.1]
})

merge_sufixos = pd.merge(funcionarios2, avaliacao, on='ID', suffixes=('_Completo', '_Abreviado'))
print("\nMerge com colunas de mesmo nome:")
print(merge_sufixos)
# Saída:
#    ID        Nome_Completo   Departamento    Nome_Abreviado  Avaliação
# 0   1           Ana Silva         Vendas            Ana S.        4.5
# 1   2     Bruno Oliveira             TI          Bruno O.        4.2
# 2   3    Carolina Santos       Marketing      Carolina S.        3.9
# 3   5        Elena Costa         Vendas          Elena C.        4.8

# Concatenação vertical: empilha DataFrames (mesmas colunas)
df1 = pd.DataFrame({
    'ID': [1, 2, 3],
    'Nome': ['Ana', 'Bruno', 'Carolina'],
    'Valor': [100, 200, 300]
})

df2 = pd.DataFrame({
    'ID': [4, 5, 6],
    'Nome': ['Daniel', 'Elena', 'Fernando'],
    'Valor': [400, 500, 600]
})

concatenado_v = pd.concat([df1, df2], axis=0)
print("\nConcatenação vertical (empilhamento):")
print(concatenado_v)
# Saída:
#    ID      Nome  Valor
# 0   1       Ana    100
# 1   2     Bruno    200
# 2   3  Carolina    300
# 0   4    Daniel    400
# 1   5     Elena    500
# 2   6  Fernando    600

# Concatenação horizontal: junta lado a lado (linhas devem ter o mesmo índice ou se alinham por posição)
df3 = pd.DataFrame({
    'ID': [1, 2, 3, 4],
    'Nome': ['Ana', 'Bruno', 'Carolina', 'Daniel']
})

df4 = pd.DataFrame({
    'Salário': [3500, 4200, 2800, 5100],
    'Departamento': ['Vendas', 'TI', 'Marketing', 'TI']
})

concatenado_h = pd.concat([df3, df4], axis=1)
print("\nConcatenação horizontal:")
print(concatenado_h)
# Saída:
#    ID      Nome  Salário Departamento
# 0   1       Ana     3500      Vendas
# 1   2     Bruno     4200          TI
# 2   3  Carolina     2800   Marketing
# 3   4    Daniel     5100          TI

```

## Pivotagem e Reformatação de Dados

Pandas facilita a reformatação de dados:

```python
# Criando um DataFrame de exemplo (dados de vendas por região, produto e trimestre)
dados = {
    'Região': ['Norte', 'Norte', 'Norte', 'Norte', 'Sul', 'Sul', 'Sul', 'Sul',
               'Leste', 'Leste', 'Leste', 'Leste', 'Oeste', 'Oeste', 'Oeste', 'Oeste'],
    'Trimestre': ['T1', 'T2', 'T3', 'T4', 'T1', 'T2', 'T3', 'T4',
                  'T1', 'T2', 'T3', 'T4', 'T1', 'T2', 'T3', 'T4'],
    'Produto': ['A', 'A', 'A', 'A', 'A', 'A', 'A', 'A',
                'B', 'B', 'B', 'B', 'B', 'B', 'B', 'B'],
    'Vendas': [100, 110, 120, 130, 200, 210, 220, 230,
               150, 160, 170, 180, 250, 260, 270, 280]
}

df = pd.DataFrame(dados)
print("DataFrame original:")
print(df)
print("\n")


# Tabela pivô básica
print("Tabela pivô de Vendas por Região e Trimestre:")
pivot = df.pivot_table(index='Região', columns='Trimestre', values='Vendas')
print(pivot)
print("\n")

# Tabela pivô com múltiplos índices e colunas
print("Tabela pivô com múltiplos índices e colunas:")
pivot_multi = df.pivot_table(index=['Região', 'Produto'], columns='Trimestre', values='Vendas')
print(pivot_multi)
print("\n")

# Tabela pivô com função de agregação personalizada
print("Tabela pivô com função personalizada:")
pivot_agg = df.pivot_table(index='Região', columns='Trimestre', values='Vendas',
                           aggfunc=[np.sum, np.mean, np.max])
print(pivot_agg)
print("\n")

# De formato "wide" para "long" (unpivot) com melt
df_wide = pd.DataFrame({
    'Nome': ['Ana', 'Bruno', 'Carolina'],
    'Matemática': [85, 78, 92],
    'Português': [90, 85, 88],
    'Ciências': [82, 90, 94]
})
print("DataFrame em formato 'wide':")
print(df_wide)
print("\n")

# Transformando para formato "long"
df_long = pd.melt(df_wide, id_vars=['Nome'], var_name='Disciplina', value_name='Nota')
print("DataFrame em formato 'long':")
print(df_long)
print("\n")

# De formato "long" para "wide" com pivot
df_wide_novamente = df_long.pivot(index='Nome', columns='Disciplina', values='Nota')
print("Voltando para formato 'wide':")
print(df_wide_novamente)
print("\n")

# Empilhando e desempilhando com stack e unstack
print("Empilhando com stack():")
stacked = df_wide_novamente.stack()
print(stacked)
print("\n")

print("Desempilhando com unstack():")
unstacked = stacked.unstack()
print(unstacked)
```

## Visualização de Dados com Pandas

Pandas oferece funcionalidades de visualização básicas baseadas em matplotlib:

```python
# Importando bibliotecas necessárias para visualização
import matplotlib.pyplot as plt

# Desativando avisos (em um notebook seria: %matplotlib inline)
plt.ion()

# Criando um DataFrame para exemplo
dados = {
    'Ano': [2019, 2020, 2021, 2022, 2023],
    'Vendas_Produto_A': [120, 150, 180, 210, 250],
    'Vendas_Produto_B': [90, 110, 140, 160, 190],
    'Vendas_Produto_C': [60, 80, 120, 150, 210]
}
df = pd.DataFrame(dados)
df = df.set_index('Ano')

# Gráfico de linhas
plt.figure(figsize=(10, 6))
df.plot(title='Vendas por Produto ao Longo dos Anos')
plt.xlabel('Ano')
plt.ylabel('Vendas (unidades)')
plt.grid(True, linestyle='--', alpha=0.7)
plt.legend(title='Produtos')
plt.tight_layout()
# plt.savefig('vendas_linhas.png')  # Para salvar o gráfico

# Gráfico de barras
plt.figure(figsize=(10, 6))
df.plot(kind='bar', title='Comparação de Vendas por Produto')
plt.xlabel('Ano')
plt.ylabel('Vendas (unidades)')
plt.grid(True, linestyle='--', alpha=0.7, axis='y')
plt.legend(title='Produtos')
plt.tight_layout()
# plt.savefig('vendas_barras.png')

# Gráfico de área
plt.figure(figsize=(10, 6))
df.plot(kind='area', alpha=0.5, title='Vendas Acumuladas por Produto')
plt.xlabel('Ano')
plt.ylabel('Vendas (unidades)')
plt.grid(True, linestyle='--', alpha=0.7)
plt.legend(title='Produtos', loc='upper left')
plt.tight_layout()
# plt.savefig('vendas_area.png')

# Gráfico de dispersão
plt.figure(figsize=(8, 6))
plt.scatter(df['Vendas_Produto_A'], df['Vendas_Produto_B'], c=df.index, cmap='viridis', s=100)
plt.colorbar(label='Ano')
plt.xlabel('Vendas do Produto A')
plt.ylabel('Vendas do Produto B')
plt.title('Correlação entre Vendas dos Produtos A e B')
plt.grid(True, linestyle='--', alpha=0.7)
plt.tight_layout()
# plt.savefig('vendas_dispersao.png')

# Histograma
plt.figure(figsize=(10, 6))
df.plot(kind='hist', bins=12, alpha=0.7, title='Distribuição de Vendas')
plt.xlabel('Vendas (unidades)')
plt.ylabel('Frequência')
plt.grid(True, linestyle='--', alpha=0.7, axis='y')
plt.legend(title='Produtos')
plt.tight_layout()
# plt.savefig('vendas_histograma.png')

# Box plot
plt.figure(figsize=(8, 6))
df.plot(kind='box', title='Estatísticas de Vendas por Produto')
plt.ylabel('Vendas (unidades)')
plt.grid(True, linestyle='--', alpha=0.7, axis='y')
plt.tight_layout()
# plt.savefig('vendas_boxplot.png')

# Heatmap (usando outra biblioteca além do pandas básico)
# import seaborn as sns
# plt.figure(figsize=(8, 6))
# sns.heatmap(df.corr(), annot=True, cmap='coolwarm', fmt='.2f')
# plt.title('Matriz de Correlação entre Produtos')
# plt.tight_layout()
# plt.savefig('vendas_heatmap.png')

plt.close('all')  # Fechando todas as figuras criadas (para não mostrar os gráficos aqui)
```

## Funções de Data e Hora

Pandas tem excelente suporte para manipulação de dados temporais:

```python
# Importando biblioteca
import pandas as pd
from datetime import datetime, timedelta

# Criando objetos de data/hora
# Timestamp (momento específico no tempo)
timestamp = pd.Timestamp('2023-07-15 14:30:00')
print(f"Timestamp: {timestamp}")
print(f"Ano: {timestamp.year}, Mês: {timestamp.month}, Dia: {timestamp.day}")
print(f"Hora: {timestamp.hour}, Minuto: {timestamp.minute}")
print(f"Dia da semana: {timestamp.day_name()}")
print("\n")

# Período (intervalo de tempo)
periodo = pd.Period('2023-07', freq='M')  # M = mês
print(f"Período: {periodo}")
print(f"Início do período: {periodo.start_time}")
print(f"Fim do período: {periodo.end_time}")
print("\n")

# Criando séries temporais
# DatetimeIndex
datas = pd.date_range(start='2023-01-01', end='2023-06-30', freq='MS')  # MS = início do mês
print("DatetimeIndex com frequência mensal:")
print(datas)
print("\n")

# Criando com número de períodos
datas_diarias = pd.date_range(start='2023-01-01', periods=10, freq='D')  # D = diário
print("DatetimeIndex com 10 dias:")
print(datas_diarias)
print("\n")

# Frequências personalizadas
print("Frequências personalizadas:")
print("Dias úteis (dias de semana):")
print(pd.date_range(start='2023-01-01', periods=10, freq='B'))  # B = dias úteis (business days)
print("\nQuinzenal:")
print(pd.date_range(start='2023-01-01', periods=6, freq='2W'))  # 2W = a cada 2 semanas
print("\nTrimestral:")
print(pd.date_range(start='2023-01-01', periods=4, freq='Q'))  # Q = trimestral
print("\n")

# Criando DataFrame com série temporal
dados_vendas = {
    'data': pd.date_range(start='2023-01-01', periods=12, freq='MS'),
    'vendas': [1200, 1300, 1450, 1600, 1750, 1900, 2050, 2200, 2100, 1950, 1800, 2000]
}
df_vendas = pd.DataFrame(dados_vendas)
print("DataFrame com série temporal:")
print(df_vendas)
print("\n")

# Definindo índice como data
df_vendas = df_vendas.set_index('data')
print("DataFrame com índice temporal:")
print(df_vendas)
print("\n")

# Extraindo componentes de data
df_vendas['ano'] = df_vendas.index.year
df_vendas['mês'] = df_vendas.index.month
df_vendas['nome_mês'] = df_vendas.index.month_name()
df_vendas['trimestre'] = df_vendas.index.quarter
print("DataFrame com componentes de data extraídos:")
print(df_vendas)
print("\n")

# Filtragem temporal
print("Vendas do primeiro trimestre:")
print(df_vendas.loc['2023-01-01':'2023-03-31'])
print("\n")

# Resample (agregação temporal)
print("Vendas agregadas por trimestre:")
print(df_vendas['vendas'].resample('Q').sum())
print("\n")

print("Média de vendas por bimestre:")
print(df_vendas['vendas'].resample('2M').mean())
print("\n")

# Deslocamento temporal
print("Vendas com deslocamento de 1 mês:")
print(pd.DataFrame({
    'Original': df_vendas['vendas'],
    'Deslocado_1_Mês': df_vendas['vendas'].shift(1),
    'Variação': df_vendas['vendas'] - df_vendas['vendas'].shift(1)
}))
print("\n")

# Calculando variação percentual
df_vendas['variação_pct'] = df_vendas['vendas'].pct_change() * 100
print("Variação percentual das vendas:")
print(df_vendas[['vendas', 'variação_pct']])
print("\n")

# Rolling windows (janelas móveis)
df_vendas['média_móvel_3m'] = df_vendas['vendas'].rolling(window=3).mean()
print("Média móvel de 3 meses:")
print(df_vendas[['vendas', 'média_móvel_3m']])
```

## Otimização de Performance

Dicas para melhorar a performance ao trabalhar com grandes conjuntos de dados:

```python
# Criando um DataFrame grande para exemplo
import numpy as np
import time

# Função para medir o tempo de execução
def medir_tempo(func):
    inicio = time.time()
    resultado = func()
    fim = time.time()
    return resultado, fim - inicio

# Criando um DataFrame relativamente grande
np.random.seed(42)
n_linhas = 1000000  # 1 milhão de linhas
df_grande = pd.DataFrame({
    'ID': np.arange(n_linhas),
    'A': np.random.randint(0, 100, n_linhas),
    'B': np.random.randint(0, 100, n_linhas),
    'C': np.random.choice(['X', 'Y', 'Z'], n_linhas),
    'D': np.random.normal(50, 10, n_linhas)
})

print(f"DataFrame criado com {len(df_grande)} linhas e {len(df_grande.columns)} colunas")
print("\n")

# 1. Usar tipos de dados otimizados
print("Uso de memória antes da otimização:")
print(df_grande.info(memory_usage='deep'))
print(f"Uso total de memória: {df_grande.memory_usage(deep=True).sum() / (1024*1024):.2f} MB")
print("\n")

# Convertendo tipos para mais eficientes
df_otimizado = df_grande.copy()
df_otimizado['ID'] = df_otimizado['ID'].astype('int32')
df_otimizado['A'] = df_otimizado['A'].astype('int8')
df_otimizado['B'] = df_otimizado['B'].astype('int8')
df_otimizado['C'] = df_otimizado['C'].astype('category')

print("Uso de memória após otimização:")
print(df_otimizado.info(memory_usage='deep'))
print(f"Uso total de memória: {df_otimizado.memory_usage(deep=True).sum() / (1024*1024):.2f} MB")
print("\n")

# 2. Comparação de performance: loc vs iloc vs at/iat
def acesso_loc():
    return df_grande.loc[500000, 'A']

def acesso_iloc():
    return df_grande.iloc[500000, 1]

def acesso_at():
    return df_grande.at[500000, 'A']

def acesso_iat():
    return df_grande.iat[500000, 1]

_, tempo_loc = medir_tempo(acesso_loc)
_, tempo_iloc = medir_tempo(acesso_iloc)
_, tempo_at = medir_tempo(acesso_at)
_, tempo_iat = medir_tempo(acesso_iat)

print("Tempo de acesso (microssegundos):")
print(f"loc: {tempo_loc*1000000:.2f} µs")
print(f"iloc: {tempo_iloc*1000000:.2f} µs")
print(f"at: {tempo_at*1000000:.2f} µs")
print(f"iat: {tempo_iat*1000000:.2f} µs")
print("\n")

# 3. Iteração eficiente
def iteracao_iterrows():
    total = 0
    for idx, row in df_grande.head(10000).iterrows():
        total += row['A']
    return total

def iteracao_itertuples():
    total = 0
    for row in df_grande.head(10000).itertuples():
        total += row.A
    return total

def iteracao_vetorizada():
    return df_grande.head(10000)['A'].sum()

_, tempo_iterrows = medir_tempo(iteracao_iterrows)
_, tempo_itertuples = medir_tempo(iteracao_itertuples)
_, tempo_vetorizada = medir_tempo(iteracao_vetorizada)

print("Tempo de iteração sobre 10.000 linhas (milissegundos):")
print(f"iterrows: {tempo_iterrows*1000:.2f} ms")
print(f"itertuples: {tempo_itertuples*1000:.2f} ms")
print(f"Operação vetorizada: {tempo_vetorizada*1000:.2f} ms")
print("\n")

# 4. Filtragem eficiente
def filtragem_where():
    return df_grande[df_grande['A'] > 50]

def filtragem_query():
    return df_grande.query('A > 50')

_, tempo_where = medir_tempo(filtragem_where)
_, tempo_query = medir_tempo(filtragem_query)

print("Tempo de filtragem (milissegundos):")
print(f"Usando where: {tempo_where*1000:.2f} ms")
print(f"Usando query: {tempo_query*1000:.2f} ms")
print("\n")

# 5. Apply vs. Vetorização
def usando_apply():
    return df_grande.head(100000)['A'].apply(lambda x: x * 2)

def usando_vetorizacao():
    return df_grande.head(100000)['A'] * 2

_, tempo_apply = medir_tempo(usando_apply)
_, tempo_vetorizacao = medir_tempo(usando_vetorizacao)

print("Tempo de transformação (milissegundos):")
print(f"Usando apply: {tempo_apply*1000:.2f} ms")
print(f"Usando vetorização: {tempo_vetorizacao*1000:.2f} ms")
```

## Casos de Uso Avançados

Vamos explorar alguns casos de uso mais avançados do Pandas:

```python
# 1. Manipulação de Time Series Financeiras

# Criando dados financeiros fictícios
dates = pd.date_range('2023-01-01', periods=252, freq='B')  # 252 dias úteis ~ 1 ano de trading
np.random.seed(42)

precos = 100 * (1 + np.random.normal(0, 0.01, len(dates)).cumsum())
volume = np.random.randint(100000, 1000000, len(dates))

df_finance = pd.DataFrame({
    'Close': precos,
    'Volume': volume
}, index=dates)

# Calculando retornos diários
df_finance['Returns'] = df_finance['Close'].pct_change() * 100

# Calculando média móvel
df_finance['SMA_20'] = df_finance['Close'].rolling(window=20).mean()
df_finance['SMA_50'] = df_finance['Close'].rolling(window=50).mean()

# Calculando volatilidade (desvio padrão móvel)
df_finance['Volatility_20'] = df_finance['Returns'].rolling(window=20).std()

# Primeiras linhas do DataFrame financeiro
print("DataFrame financeiro:")
print(df_finance.head())
print("\n")

# Estatísticas descritivas
print("Estatísticas descritivas dos dados financeiros:")
print(df_finance.describe())
print("\n")

# 2. Processamento de dados textuais

# Criando DataFrame com texto
dados_texto = {
    'ID': [1, 2, 3, 4, 5],
    'Texto': [
        'Esta é uma análise de sentimento usando pandas',
        'Pandas é uma ferramenta poderosa para data science',
        'Análise de dados com python é muito eficiente',
        'Data science e machine learning usam pandas frequentemente',
        'Python tem muitas bibliotecas para ciência de dados'
    ]
}

df_texto = pd.DataFrame(dados_texto)

# Contagem de palavras
df_texto['Contagem_Palavras'] = df_texto['Texto'].str.split().str.len()

# Verificando presença de palavras-chave
palavras_chave = ['pandas', 'python', 'data', 'análise']
for palavra in palavras_chave:
    df_texto[f'Contém_{palavra}'] = df_texto['Texto'].str.lower().str.contains(palavra)

# Transformando texto em minúsculas
df_texto['Texto_Lower'] = df_texto['Texto'].str.lower()

# Extraindo primeira palavra
df_texto['Primeira_Palavra'] = df_texto['Texto'].str.split().str[0]

print("Análise de texto com Pandas:")
print(df_texto)
print("\n")

# 3. Dados Multidimensionais com MultiIndex

# Criando um DataFrame com MultiIndex
indices = pd.MultiIndex.from_product([
    ['Brasil', 'EUA', 'Japão'],
    [2020, 2021, 2022],
    ['T1', 'T2', 'T3', 'T4']
], names=['País', 'Ano', 'Trimestre'])

# Gerando dados aleatórios
np.random.seed(42)
dados = np.round(np.random.normal(100, 25, len(indices)), 2)

# Criando DataFrame
df_multi = pd.DataFrame({'Vendas': dados}, index=indices)

print("DataFrame com MultiIndex:")
print(df_multi.head(10))
print("\n")

# Seleção por nível
print("Vendas no Brasil em 2021:")
print(df_multi.loc[('Brasil', 2021)])
print("\n")

# Seleção cruzada de níveis
print("Vendas do primeiro trimestre de todos os anos e países:")
idx = pd.IndexSlice
print(df_multi.loc[idx[:, :, 'T1'], :])
print("\n")

# Descompactando níveis em colunas
df_reset = df_multi.reset_index()
print("MultiIndex convertido para colunas:")
print(df_reset.head())
print("\n")

# Empilhando e desempilhando níveis
df_stack = df_reset.set_index(['País', 'Ano']).stack()
print("DataFrame após stack():")
print(df_stack.head())
print("\n")

df_unstack = df_multi.unstack(level='Trimestre')
print("DataFrame após unstack():")
print(df_unstack.head())

# 4. Aplicando funções em janelas móveis customizadas
def autocorrelacao(x, lag=1):
    """Calcula a autocorrelação com defasagem (lag) especificada"""
    x1 = x[lag:]
    x2 = x[:-lag]
    if len(x1) == 0 or len(x2) == 0:
        return np.nan
    return np.corrcoef(x1, x2)[0, 1]

# Aplicando função personalizada em janela móvel
df_finance['Autocorr_5'] = df_finance['Returns'].rolling(window=20).apply(lambda x: autocorrelacao(x, lag=5))

print("\nDataFrame com autocorrelação aplicada em janela móvel:")
print(df_finance[['Returns', 'Autocorr_5']].head(10))
```

## Dicas e Boas Práticas

Vamos finalizar com algumas dicas e boas práticas para usar o Pandas eficientemente:

```python
# Dicas e melhores práticas para usar Pandas

# 1. Use os tipos de dados corretos para economizar memória
# Exemplo: Converter colunas categóricas para o tipo 'category'
df = pd.DataFrame({
    'ID': range(1000),
    'Categoria': np.random.choice(['A', 'B', 'C', 'D', 'E'], 1000)
})

print("Memória antes da otimização:")
print(f"{df.memory_usage(deep=True).sum() / 1024:.2f} KB")

# Convertendo para tipo category
df['Categoria'] = df['Categoria'].astype('category')

print("Memória após otimização:")
print(f"{df.memory_usage(deep=True).sum() / 1024:.2f} KB")
print("\n")

# 2. Evite usar .apply() quando operações vetorizadas são possíveis
# Ruim:
# df['ao_quadrado'] = df['valor'].apply(lambda x: x**2)
# Bom:
# df['ao_quadrado'] = df['valor'] ** 2

# 3. Use consultas eficientes
df_grande = pd.DataFrame({
    'A': np.random.randn(100000),
    'B': np.random.choice(['X', 'Y', 'Z'], 100000),
    'C': np.random.randn(100000)
})

# Comparação entre diferentes métodos de filtro
def filtro_1():
    return df_grande[(df_grande['A'] > 0) & (df_grande['B'] == 'X')]

def filtro_2():
    return df_grande.query('A > 0 and B == "X"')

def filtro_3():
    mask1 = df_grande['A'] > 0
    mask2 = df_grande['B'] == 'X'
    return df_grande[mask1 & mask2]

# Medindo o tempo (exemplo omitido para brevidade)

# 4. Prefira métodos específicos em vez de loops
# Ruim:
# total = 0
# for i in range(len(df)):
#     total += df.iloc[i, 0]
# Bom:
# total = df.iloc[:, 0].sum()

# 5. Use loc e iloc para acessar linhas e colunas

# 6. Configure opções de exibição adequadas
pd.set_option('display.max_columns', 10)  # Mostrar até 10 colunas
pd.set_option('display.max_rows', 10)     # Mostrar até 10 linhas
pd.set_option('display.width', 80)        # Largura máxima da saída
pd.set_option('display.precision', 2)     # Precisão decimal

# 7. Use funções eficientes para agregar dados
df = pd.DataFrame({
    'grupo': ['A', 'A', 'B', 'B', 'B', 'C'],
    'valor': [1, 2, 3, 4, 5, 6]
})

# Agregação eficiente
result = df.groupby('grupo').agg({
    'valor': ['sum', 'mean', 'count']
})
print("Agregação eficiente:")
print(result)
print("\n")

# 8. Manipule dados ausentes corretamente
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, np.nan, 8],
    'C': [9, 10, 11, 12]
})

# Verificando valores ausentes
print("Valores ausentes por coluna:")
print(df.isna().sum())

# Preenchendo valores ausentes estrategicamente
# Preenchendo com média
df_mean = df.fillna(df.mean())
print("\nPreenchendo com média:")
print(df_mean)

# Preenchendo com método forward fill
df_ffill = df.fillna(method='ffill')
print("\nPreenchendo com forward fill:")
print(df_ffill)
print("\n")

# 9. Use chains de métodos para tornar o código mais legível
df = pd.DataFrame({
    'grupo': ['A', 'A', 'B', 'B', 'C', 'C', 'C'],
    'valor1': np.random.randn(7),
    'valor2': np.random.randn(7)
})

# Encadeamento de métodos para transformação de dados
result = (df
          .groupby('grupo')
          .agg({'valor1': 'mean', 'valor2': 'sum'})
          .reset_index()
          .rename(columns={'valor1': 'média', 'valor2': 'soma'})
          .sort_values('média', ascending=False)
         )

print("Resultado do encadeamento de métodos:")
print(result)
print("\n")

# 10. Use copy() quando necessário para evitar SettingWithCopyWarning
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
# Errado (pode causar SettingWithCopyWarning):
# subset = df[df['A'] > 1]
# subset['C'] = subset['B'] * 2

# Correto:
subset = df[df['A'] > 1].copy()
subset['C'] = subset['B'] * 2

print("DataFrame original:")
print(df)
print("\nSubset modificado corretamente:")
print(subset)
```

## Conclusão

O Pandas é uma ferramenta extremamente poderosa e versátil para análise e manipulação de dados em Python. Este guia cobriu desde os conceitos básicos até técnicas avançadas, mas ainda há muito mais para explorar.

As principais vantagens do Pandas incluem:

1. **Flexibilidade**: Trabalha com diferentes tipos de dados e formatos
2. **Performance**: Operações vetorizadas e otimizadas para grandes volumes de dados
3. **Integração**: Funciona perfeitamente com outras bibliotecas do ecossistema Python
4. **Facilidade de uso**: API intuitiva e bem documentada

Para continuar aprendendo sobre Pandas, recomendamos:

- Documentação oficial: https://pandas.pydata.org/docs/
- Livro "Python for Data Analysis" de Wes McKinney (criador do Pandas)
- Praticar com diferentes tipos de conjuntos de dados
- Participar da comunidade PyData
