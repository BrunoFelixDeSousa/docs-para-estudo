# [⬅ Voltar para o índice principal](../../../README.md)

# Guia Completo de NumPy

## Sumário

1. [Introdução ao NumPy](#introdução-ao-numpy)
2. [Instalação](#instalação)
3. [Criação de Arrays](#criação-de-arrays)
4. [Propriedades de Arrays](#propriedades-de-arrays)
5. [Indexação e Fatiamento](#indexação-e-fatiamento)
6. [Operações com Arrays](#operações-com-arrays)
7. [Broadcasting](#broadcasting)
8. [Funções Matemáticas](#funções-matemáticas)
9. [Funções Estatísticas](#funções-estatísticas)
10. [Manipulação de Arrays](#manipulação-de-arrays)
11. [Álgebra Linear](#álgebra-linear)
12. [Números Aleatórios](#números-aleatórios)
13. [Entrada e Saída de Dados](#entrada-e-saída-de-dados)
14. [Exemplos Práticos](#exemplos-práticos)
15. [Dicas e Truques](#dicas-e-truques)
16. [Comparação com Python Puro](#comparação-com-python-puro)
17. [Recursos Adicionais](#recursos-adicionais)

## Introdução ao NumPy

NumPy (Numerical Python) é uma biblioteca fundamental para computação científica em Python. Ela fornece suporte para arrays multidimensionais e matrizes, junto com uma grande coleção de funções matemáticas de alto nível para operar nessas estruturas de dados.

### Por que usar NumPy?

- **Eficiência**: NumPy é muito mais rápido que listas Python para operações numéricas
- **Menos código**: Implementa operações vetorizadas que substituem loops
- **Qualidade**: Código estável, testado e mantido por uma grande comunidade
- **Interoperabilidade**: Base para muitas outras bibliotecas científicas como Pandas, Matplotlib, SciPy

```python
# Comparação simples: Python vs NumPy para soma de arrays
import numpy as np
import time

# Criando dados
tamanho = 1000000
lista_python = list(range(tamanho))
array_numpy = np.array(range(tamanho))

# Somando elementos com Python puro
inicio = time.time()
soma_python = sum(lista_python)
tempo_python = time.time() - inicio

# Somando elementos com NumPy
inicio = time.time()
soma_numpy = np.sum(array_numpy)
tempo_numpy = time.time() - inicio

print(f"Python: {tempo_python:.6f} segundos")
print(f"NumPy: {tempo_numpy:.6f} segundos")
print(f"NumPy é {tempo_python/tempo_numpy:.1f}x mais rápido")
```

## Instalação

Instalar o NumPy é simples utilizando pip:

```bash
pip install numpy
```

Ou usando conda:

```bash
conda install numpy
```

Após a instalação, importe o NumPy no seu código:

```python
import numpy as np  # A convenção padrão é importar como 'np'
```

## Criação de Arrays

### Criando arrays a partir de listas Python

```python
# Array unidimensional a partir de uma lista
array_1d = np.array([1, 2, 3, 4, 5])
print(array_1d)  # [1 2 3 4 5]

# Array bidimensional a partir de uma lista de listas
array_2d = np.array([[1, 2, 3], [4, 5, 6]])
print(array_2d)
# [[1 2 3]
#  [4 5 6]]

# Array com tipo de dado específico
array_float = np.array([1, 2, 3], dtype=float)
print(array_float)  # [1. 2. 3.]
```

### Funções para criar arrays especiais

```python
# Array de zeros
zeros = np.zeros((3, 4))  # Array 3x4 de zeros
print(zeros)
# [[0. 0. 0. 0.]
#  [0. 0. 0. 0.]
#  [0. 0. 0. 0.]]

# Array de uns
ones = np.ones((2, 3))  # Array 2x3 de uns
print(ones)
# [[1. 1. 1.]
#  [1. 1. 1.]]

# Array com valor constante
full = np.full((2, 2), 7)  # Array 2x2 com valor 7
print(full)
# [[7 7]
#  [7 7]]

# Matriz identidade (uns na diagonal principal, zeros no resto)
identidade = np.eye(3)  # Matriz identidade 3x3
print(identidade)
# [[1. 0. 0.]
#  [0. 1. 0.]
#  [0. 0. 1.]]

# Array com valores aleatórios entre 0 e 1
aleatorio = np.random.random((2, 2))
print(aleatorio)
# [[0.12345678 0.98765432]
#  [0.56789012 0.34567890]] (valores serão diferentes em cada execução)
```

### Criando sequências

```python
# Sequência com intervalo específico
range_array = np.arange(0, 10, 2)  # Início, fim (exclusivo), passo
print(range_array)  # [0 2 4 6 8]

# Sequência com número específico de elementos
linspace = np.linspace(0, 1, 5)  # Início, fim (inclusivo), número de elementos
print(linspace)  # [0.   0.25 0.5  0.75 1.  ]

# Sequência logarítmica
logspace = np.logspace(0, 2, 5)  # Base-10, de 10^0 até 10^2, 5 elementos
print(logspace)  # [  1.          3.16227766  10.         31.6227766  100.        ]
```

## Propriedades de Arrays

Os arrays NumPy possuem vários atributos que fornecem informações sobre sua estrutura:

```python
# Criando um array para demonstração
arr = np.array([[1, 2, 3], [4, 5, 6]])

# Número de dimensões
print(f"Dimensões (ndim): {arr.ndim}")  # 2

# Forma do array (linhas, colunas)
print(f"Forma (shape): {arr.shape}")  # (2, 3)

# Tamanho total (número de elementos)
print(f"Tamanho (size): {arr.size}")  # 6

# Tipo de dados
print(f"Tipo de dados (dtype): {arr.dtype}")  # int64

# Tamanho de cada elemento em bytes
print(f"Tamanho de item (itemsize): {arr.itemsize}")  # 8 (para int64)

# Número total de bytes
print(f"Total de bytes (nbytes): {arr.nbytes}")  # 48 (6 elementos * 8 bytes)
```

## Indexação e Fatiamento

### Indexação Básica

```python
# Array unidimensional
arr_1d = np.array([10, 20, 30, 40, 50])

# Acessando elementos individuais (indexação começa em 0)
print(arr_1d[0])   # 10 (primeiro elemento)
print(arr_1d[2])   # 30 (terceiro elemento)
print(arr_1d[-1])  # 50 (último elemento)

# Array bidimensional
arr_2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Acessando elementos com coordenadas [linha, coluna]
print(arr_2d[0, 0])  # 1 (primeira linha, primeira coluna)
print(arr_2d[1, 2])  # 6 (segunda linha, terceira coluna)
print(arr_2d[2, -1])  # 9 (terceira linha, última coluna)
```

### Fatiamento

```python
# Array unidimensional
arr = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

# Sintaxe de fatiamento: array[início:fim:passo]
print(arr[1:6])     # [1 2 3 4 5] (do índice 1 até o 5)
print(arr[::2])     # [0 2 4 6 8] (do início ao fim, pulando de 2 em 2)
print(arr[5:])      # [5 6 7 8 9] (do índice 5 até o fim)
print(arr[:5])      # [0 1 2 3 4] (do início até o índice 4)
print(arr[::-1])    # [9 8 7 6 5 4 3 2 1 0] (array invertido)

# Array bidimensional
arr_2d = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])

# Selecionando submatrizes
print(arr_2d[0:2, 1:3])  # Linhas 0-1, colunas 1-2
# [[2 3]
#  [6 7]]

# Selecionando linhas inteiras
print(arr_2d[1])  # [5 6 7 8] (segunda linha)
print(arr_2d[0:2])  # Primeiras duas linhas
# [[1 2 3 4]
#  [5 6 7 8]]

# Selecionando colunas inteiras
print(arr_2d[:, 2])  # [3 7 11] (terceira coluna)
```

### Indexação Avançada

```python
# Indexação com arrays
arr = np.array([10, 20, 30, 40, 50])
indices = np.array([0, 2, 4])
print(arr[indices])  # [10 30 50]

# Indexação booleana
arr = np.array([1, 2, 3, 4, 5])
mascara = np.array([True, False, True, False, True])
print(arr[mascara])  # [1 3 5]

# Filtragem baseada em condição
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
filtro = arr > 5
print(filtro)         # [False False False False False  True  True  True  True]
print(arr[filtro])    # [6 7 8 9]
# Equivalente direto:
print(arr[arr > 5])   # [6 7 8 9]

# Múltiplas condições
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
# Elementos pares maiores que 5
print(arr[(arr > 5) & (arr % 2 == 0)])  # [6 8]
```

## Operações com Arrays

### Operações Aritméticas Básicas

```python
# Criando arrays para demonstração
a = np.array([10, 20, 30, 40])
b = np.array([1, 2, 3, 4])

# Adição
print(a + b)  # [11 22 33 44]
print(np.add(a, b))  # [11 22 33 44]

# Subtração
print(a - b)  # [9 18 27 36]
print(np.subtract(a, b))  # [9 18 27 36]

# Multiplicação
print(a * b)  # [10 40 90 160]
print(np.multiply(a, b))  # [10 40 90 160]

# Divisão
print(a / b)  # [10. 10. 10. 10.]
print(np.divide(a, b))  # [10. 10. 10. 10.]

# Elevado a potência
print(b ** 2)  # [1 4 9 16]
print(np.power(b, 2))  # [1 4 9 16]

# Módulo (resto da divisão)
print(a % b)  # [0 0 0 0]
print(np.mod(a, b))  # [0 0 0 0]
```

### Operações com Escalares

```python
# Array para demonstração
arr = np.array([1, 2, 3, 4])

# Adição com escalar
print(arr + 5)  # [6 7 8 9]

# Multiplicação com escalar
print(arr * 2)  # [2 4 6 8]

# Potência com escalar
print(arr ** 2)  # [1 4 9 16]
```

### Funções Universais (ufuncs)

```python
# Array para demonstração
arr = np.array([0, np.pi/4, np.pi/2, np.pi])

# Funções trigonométricas
print(np.sin(arr))  # [0.  0.70710678 1. 0.]
print(np.cos(arr))  # [1.  0.70710678 0. -1.]
print(np.tan(arr))  # [0. 1. inf 0.]

# Funções exponenciais e logarítmicas
arr = np.array([1, 2, 3, 4])
print(np.exp(arr))    # [2.71828183 7.3890561  20.08553692 54.59815003]
print(np.log(arr))    # [0. 0.69314718 1.09861229 1.38629436]
print(np.log10(arr))  # [0. 0.30103 0.47712125 0.60205999]
print(np.sqrt(arr))   # [1. 1.41421356 1.73205081 2.]

# Arredondamento
arr = np.array([1.1, 1.5, 1.9, 2.5])
print(np.floor(arr))  # [1. 1. 1. 2.]
print(np.ceil(arr))   # [2. 2. 2. 3.]
print(np.round(arr))  # [1. 2. 2. 2.]
```

## Broadcasting

Broadcasting é um mecanismo que permite ao NumPy trabalhar com arrays de diferentes formas durante operações aritméticas. As regras de broadcasting permitem que operações sejam executadas entre arrays com dimensões diferentes.

```python
# Exemplo 1: Array + escalar
arr = np.array([1, 2, 3, 4])
print(arr + 10)  # [11 12 13 14]
# O escalar 10 é "transmitido" para um array [10, 10, 10, 10]

# Exemplo 2: Arrays de diferentes formas
# Array de forma (3,) (vetor linha)
a = np.array([1, 2, 3])
# Array de forma (3, 1) (vetor coluna)
b = np.array([[10], [20], [30]])

# b é "transmitido" para forma (3, 3) e então acontece a soma
print(a + b)
# [[11 12 13]
#  [21 22 23]
#  [31 32 33]]

# Exemplo 3: Broadcasting mais complexo
# Array 4x3
a = np.ones((4, 3))
# Array 3
b = np.array([1, 2, 3])

# b é tratado como uma linha a ser adicionada em cada linha de a
#      a      +      b      =       c
# [[1. 1. 1.] +  [1. 2. 3.] =  [[2. 3. 4.]
#  [1. 1. 1.] +  [1. 2. 3.] =   [2. 3. 4.]
#  [1. 1. 1.] +  [1. 2. 3.] =   [2. 3. 4.]
#  [1. 1. 1.] +  [1. 2. 3.] =   [2. 3. 4.]]
c = a + b
print(c)
# [[2. 3. 4.]
#  [2. 3. 4.]
#  [2. 3. 4.]
#  [2. 3. 4.]]
```

### Regras de Broadcasting

1. Se dois arrays têm dimensões diferentes, a forma do array com menor dimensão é preenchida com 1's à esquerda.
2. Se a forma de dois arrays não corresponde em alguma dimensão, o array com tamanho 1 nessa dimensão é esticado para corresponder ao outro.
3. Se em alguma dimensão os tamanhos são diferentes e nenhum é igual a 1, ocorre um erro.

```python
# Demonstração completa das regras de broadcasting
# Array de forma (2, 3)
a = np.array([[1, 2, 3], [4, 5, 6]])

# Array de forma (3,) - se torna (1, 3) para broadcasting
b = np.array([10, 20, 30])

# Resultado: forma (2, 3)
print(a + b)
# [[11 22 33]
#  [14 25 36]]

# Array de forma (2, 1)
c = np.array([[100], [200]])

# Resultado: forma (2, 3)
print(a + c)
# [[101 102 103]
#  [204 205 206]]

# Combinando todos
# a: (2, 3), b: (1, 3), c: (2, 1)
# Resultado: forma (2, 3)
print(a + b + c)
# [[111 122 133]
#  [214 225 236]]
```

## Funções Matemáticas

### Funções Básicas

```python
# Array para demonstração
arr = np.array([-4, -2, 0, 2, 4])

# Valor absoluto
print(np.abs(arr))  # [4 2 0 2 4]

# Sinal (-1 para negativo, 0 para zero, 1 para positivo)
print(np.sign(arr))  # [-1 -1  0  1  1]

# Máximo e mínimo
print(np.max(arr))  # 4
print(np.min(arr))  # -4

# Valores máximos e mínimos elemento a elemento
a = np.array([1, 5, 3])
b = np.array([2, 3, 9])
print(np.maximum(a, b))  # [2 5 9] (máximo elemento a elemento)
print(np.minimum(a, b))  # [1 3 3] (mínimo elemento a elemento)
```

### Funções Trigonométricas

```python
# Ângulos em radianos
angles = np.array([0, np.pi/6, np.pi/4, np.pi/3, np.pi/2])

# Funções trigonométricas
print(np.sin(angles))
# [0.         0.5        0.70710678 0.8660254  1.        ]

print(np.cos(angles))
# [1.00000000e+00 8.66025404e-01 7.07106781e-01 5.00000000e-01 6.12323400e-17]

print(np.tan(angles))
# [0.00000000e+00 5.77350269e-01 1.00000000e+00 1.73205081e+00 1.63312394e+16]

# Funções trigonométricas inversas
valores = np.array([0, 0.5, 0.7071, 0.866, 1])
print(np.arcsin(valores))  # Arco seno (em radianos)
print(np.arccos(valores))  # Arco cosseno (em radianos)
print(np.arctan(valores))  # Arco tangente (em radianos)
```

### Funções Exponenciais e Logarítmicas

```python
# Array para demonstração
arr = np.array([1, 2, 3, 4])

# Exponencial (e^x)
print(np.exp(arr))
# [ 2.71828183  7.3890561  20.08553692 54.59815003]

# Exponencial (2^x)
print(np.exp2(arr))
# [ 2.  4.  8. 16.]

# Logaritmo natural (base e)
print(np.log(arr))
# [0.         0.69314718 1.09861229 1.38629436]

# Logaritmo base 2
print(np.log2(arr))
# [0.        1.        1.5849625 2.       ]

# Logaritmo base 10
print(np.log10(arr))
# [0.         0.30103    0.47712125 0.60205999]

# Logaritmo base arbitrária
print(np.log(arr) / np.log(5))  # Logaritmo base 5
```

## Funções Estatísticas

### Estatísticas Básicas

```python
# Array para demonstração
arr = np.array([1, 2, 3, 4, 5])

# Soma
print(np.sum(arr))  # 15

# Média
print(np.mean(arr))  # 3.0

# Mediana
print(np.median(arr))  # 3.0

# Desvio padrão
print(np.std(arr))  # 1.4142135623730951

# Variância
print(np.var(arr))  # 2.0

# Valor mínimo e máximo
print(np.min(arr))  # 1
print(np.max(arr))  # 5

# Índice do valor mínimo e máximo
print(np.argmin(arr))  # 0
print(np.argmax(arr))  # 4
```

### Estatísticas em Arrays Multidimensionais

```python
# Array bidimensional para demonstração
arr_2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Soma por eixo
print(np.sum(arr_2d))       # 45 (soma total)
print(np.sum(arr_2d, axis=0))  # [12 15 18] (soma de cada coluna)
print(np.sum(arr_2d, axis=1))  # [ 6 15 24] (soma de cada linha)

# Média por eixo
print(np.mean(arr_2d, axis=0))  # [4. 5. 6.] (média de cada coluna)
print(np.mean(arr_2d, axis=1))  # [2. 5. 8.] (média de cada linha)

# Valores máximos por eixo
print(np.max(arr_2d, axis=0))  # [7 8 9] (máximo de cada coluna)
print(np.max(arr_2d, axis=1))  # [3 6 9] (máximo de cada linha)
```

### Histogramas e Percentis

```python
# Criando dados aleatórios para demonstração
data = np.random.normal(0, 1, 1000)  # 1000 amostras da distribuição normal

# Calculando histograma
hist, bins = np.histogram(data, bins=10)
print("Contagens por bin:", hist)
print("Limites dos bins:", bins)

# Percentis
percentis = [0, 25, 50, 75, 100]
print(np.percentile(data, percentis))
# Exemplo de saída: [-3.10, -0.67, 0.03, 0.71, 3.24]

# Quartis (equivalente a percentis 25, 50, 75)
print(np.quantile(data, [0.25, 0.5, 0.75]))

# Cumulativo
print(np.cumsum(arr))  # [1 3 6 10 15]
print(np.cumprod(arr))  # [1 2 6 24 120]
```

## Manipulação de Arrays

### Mudança de Forma

```python
# Criando um array para demonstração
arr = np.arange(12)
print(arr)  # [ 0  1  2  3  4  5  6  7  8  9 10 11]

# Mudando para forma 3x4
arr_2d = arr.reshape(3, 4)
print(arr_2d)
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]

# Mudando para forma 2x2x3
arr_3d = arr.reshape(2, 2, 3)
print(arr_3d)
# [[[ 0  1  2]
#   [ 3  4  5]]
#  [[ 6  7  8]
#   [ 9 10 11]]]

# Usando -1 para inferir a dimensão
print(arr.reshape(4, -1))
# [[ 0  1  2]
#  [ 3  4  5]
#  [ 6  7  8]
#  [ 9 10 11]]

# Transformando em vetor unidimensional
print(arr_2d.ravel())
# [ 0  1  2  3  4  5  6  7  8  9 10 11]

# Outra forma de achatar o array
print(arr_2d.flatten())
# [ 0  1  2  3  4  5  6  7  8  9 10 11]
```

### Transposição e Mudanças de Eixo

```python
# Array 2D para demonstração
arr = np.array([[1, 2, 3], [4, 5, 6]])
print(arr)
# [[1 2 3]
#  [4 5 6]]

# Transposição (linhas viram colunas e vice-versa)
print(arr.T)
# [[1 4]
#  [2 5]
#  [3 6]]

# Array 3D para demonstração
arr_3d = np.array([[[1, 2], [3, 4]], [[5, 6], [7, 8]]])
print(arr_3d.shape)  # (2, 2, 2)

# Swapaxes - troca dois eixos específicos
print(np.swapaxes(arr_3d, 0, 2).shape)  # (2, 2, 2)

# Transpose - especifica a ordem dos eixos
print(np.transpose(arr_3d, (2, 0, 1)).shape)  # (2, 2, 2)
```

### Concatenação e Divisão

```python
# Arrays para demonstração
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

# Concatenação vertical (ao longo do eixo 0)
print(np.concatenate([a, b], axis=0))
# [[1 2]
#  [3 4]
#  [5 6]
#  [7 8]]

# Concatenação horizontal (ao longo do eixo 1)
print(np.concatenate([a, b], axis=1))
# [[1 2 5 6]
#  [3 4 7 8]]

# Alternativas para concatenação
print(np.vstack([a, b]))  # Vertical
# [[1 2]
#  [3 4]
#  [5 6]
#  [7 8]]

print(np.hstack([a, b]))  # Horizontal
# [[1 2 5 6]
#  [3 4 7 8]]

# Divisão de arrays
arr = np.arange(16).reshape(4, 4)
print(arr)
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]
#  [12 13 14 15]]

# Dividindo horizontalmente em 2 partes
print(np.hsplit(arr, 2))
# [array([[ 0,  1],
#        [ 4,  5],
#        [ 8,  9],
#        [12, 13]]), array([[ 2,  3],
#        [ 6,  7],
#        [10, 11],
#        [14, 15]])]

# Dividindo verticalmente em 2 partes
print(np.vsplit(arr, 2))
# [array([[0, 1, 2, 3],
#        [4, 5, 6, 7]]), array([[ 8,  9, 10, 11],
#        [12, 13, 14, 15]])]
```

### Empilhamento de Arrays

```python
# Arrays para demonstração
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

# Empilhamento vertical
print(np.vstack((a, b)))
# [[1 2 3]
#  [4 5 6]]

# Empilhamento horizontal
print(np.hstack((a, b)))
# [1 2 3 4 5 6]

# Empilhamento em profundidade
c = np.array([7, 8, 9])
print(np.dstack((a, b, c)))
# [[[1 4 7]
#   [2 5 8]
#   [3 6 9]]]

# Empilhamento de forma geral
print(np.stack((a, b, c), axis=0))  # Ao longo do primeiro eixo
# [[1 2 3]
#  [4 5 6]
#  [7 8 9]]

print(np.stack((a, b, c), axis=1))  # Ao longo do segundo eixo
# [[1 4 7]
#  [2 5 8]
#  [3 6 9]]
```

## Álgebra Linear

NumPy inclui várias operações de álgebra linear, mas para funcionalidades mais avançadas, é recomendado usar o módulo `numpy.linalg` ou a biblioteca SciPy.

### Operações Básicas

```python
# Produto escalar (dot product)
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
print(np.dot(a, b))  # 32 (= 1*4 + 2*5 + 3*6)
print(a @ b)         # 32 (sintaxe alternativa para produto escalar)

# Produto matricial
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
print(np.dot(A, B))
# [[19 22]
#  [43 50]]
print(A @ B)  # Mesmo resultado usando a sintaxe '@'
# [[19 22]
#  [43 50]]

# Determinante
print(np.linalg.det(A))  # -2.0000000000000004

# Inversa de uma matriz
print(np.linalg.inv(A))
# [[-2.   1. ]
#  [ 1.5 -0.5]]

# Verificando: A * A^-1 = I
print(A @ np.linalg.inv(A))
# [[1. 0.]
#  [0. 1.]]
```

### Decomposições Matriciais

```python
# Criando uma matriz para demonstração
A = np.array([[1, 2], [3, 4]])

# Autovalores e autovetores
eigenvalues, eigenvectors = np.linalg.eig(A)
print("Autovalores:", eigenvalues)
# Autovalores: [-0.37228132  5.37228132]
print("Autovetores:")
print(eigenvectors)
# [[-0.82456484 -0.41597356]
#  [ 0.56576746 -0.90937671]]

# Decomposição SVD (Singular Value Decomposition)
U, S, Vt = np.linalg.svd(A)
print("U (matriz ortogonal esquerda):")
print(U)
print("S (valores singulares):")
print(S)
print("Vt (matriz ortogonal direita transposta):")
print(Vt)

# Decomposição QR
Q, R = np.linalg.qr(A)
print("Q (matriz ortogonal):")
print(Q)
print("R (matriz triangular superior):")
print(R)
```

### Sistemas de Equações Lineares

```python
# Resolvendo sistema de equações lineares: Ax = b
# Exemplo: 2x + y = 5
#          3x + 2y = 8
A = np.array([[2, 1], [3, 2]])
b = np.array([5, 8])

# Solução
x = np.linalg.solve(A, b)
print("Solução x:", x)  # Deve ser [2. 1.]

# Verificando a solução
print("Verificação A @ x:", A @ x)  # Deve ser [5. 8.]
```

### Normas e Comparações

```python
# Array para demonstração
v = np.array([3, 4])

# Norma L2 (euclidiana)
print(np.linalg.norm(v))  # 5.0

# Normas Lp
print(np.linalg.norm(v, ord=1))  # 7.0 (norma L1 = soma dos valores absolutos)
print(np.linalg.norm(v, ord=np.inf))  # 4.0 (norma infinito = valor máximo absoluto)

# Norma de matrizes
A = np.array([[1, 2], [3, 4]])
print(np.linalg.norm(A))  # 5.477225575051661 (norma de Frobenius)
print(np.linalg.norm(A, ord=1))  # 6.0 (norma de coluna máxima)
print(np.linalg.norm(A, ord=np.inf))  # 7.0 (norma de linha máxima)
```

## Números Aleatórios

O NumPy fornece um gerador de números pseudoaleatórios poderoso através do módulo `numpy.random`.

### Geração Básica de Números Aleatórios

```python
# Definindo uma semente para reproducibilidade
np.random.seed(42)

# Números aleatórios uniformes entre 0 e 1
print(np.random.random(5))
# [0.37454012 0.95071431 0.73199394 0.59865848 0.15601864]

# Inteiros aleatórios
# Gera 5 inteiros aleatórios entre 1 e 10 (inclusive)
print(np.random.randint(1, 11, 5))
# [6 1 4 4 8]

# Amostragem de uma distribuição normal (média=0, desvio padrão=1)
print(np.random.normal(0, 1, 5))
# [ 0.24196227 -1.91328024 -1.72798696  0.30015937  0.30743432]

# Amostragem de uma distribuição uniforme
print(np.random.uniform(0, 10, 5))
# [7.20324493 8.17426154 4.48252056 5.2408159  3.57709929]
```

### Distribuições Estatísticas

```python
# Tamanho da amostra
size = 1000

# Distribuição normal (Gaussiana)
normal_samples = np.random.normal(loc=0, scale=1, size=size)
# loc = média, scale = desvio padrão

# Distribuição binomial
binomial_samples = np.random.binomial(n=10, p=0.5, size=size)
# n = número de tentativas, p = probabilidade de sucesso

# Distribuição de Poisson
poisson_samples = np.random.poisson(lam=5, size=size)
# lam = taxa média de ocorrência

# Distribuição exponencial
exponential_samples = np.random.exponential(scale=1, size=size)
# scale = 1/lambda (lambda = taxa)

# Distribuição gama
gamma_samples = np.random.gamma(shape=2, scale=2, size=size)
# shape = parâmetro de forma, scale = parâmetro de escala

# Visualizando as distribuições com histogramas (requer matplotlib)
# import matplotlib.pyplot as plt
# plt.figure(figsize=(10, 8))
# plt.subplot(231)
# plt.hist(normal_samples, bins=30)
# plt.title('Normal')
# plt.subplot(232)
# plt.hist(binomial_samples, bins=30)
# plt.title('Binomial')
# plt.subplot(233)
# plt.hist(poisson_samples, bins=30)
# plt.title('Poisson')
# plt.subplot(234)
# plt.hist(exponential_samples, bins=30)
# plt.title('Exponencial')
# plt.subplot(235)
# plt.hist(gamma_samples, bins=30)
# plt.title('Gama')
# plt.tight_layout()
# plt.show()
```

### Embaralhamento e Amostragem Aleatória

```python
# Criando um array para demonstração
arr = np.arange(10)
print("Array original:", arr)  # [0 1 2 3 4 5 6 7 8 9]

# Embaralhando o array
np.random.shuffle(arr)
print("Array embaralhado:", arr)  # [exemplo: 5 1 9 8 0 4 7 6 2 3]

# Recriando o array ordenado
arr = np.arange(10)

# Amostragem aleatória sem repetição
print("Amostra sem reposição:", np.random.choice(arr, size=5, replace=False))
# Exemplo: [1 3 5 7 9]

# Amostragem aleatória com repetição
print("Amostra com reposição:", np.random.choice(arr, size=5, replace=True))
# Exemplo: [5 0 5 1 8]

# Amostragem com probabilidades personalizadas
probabilidades = np.array([0.05, 0.05, 0.05, 0.05, 0.2, 0.2, 0.1, 0.1, 0.1, 0.1])
print("Amostra com probabilidades específicas:",
      np.random.choice(arr, size=5, p=probabilidades))
# Exemplo: [4 9 6 5 4]
```

### Geradores e Reprodutibilidade

```python
# Criando um gerador com semente específica
rng = np.random.RandomState(42)

# Gerando números aleatórios
print("Primeiro conjunto:", rng.rand(3))
# [0.37454012 0.95071431 0.73199394]

# Criando outro gerador com a mesma semente
rng2 = np.random.RandomState(42)
print("Confirmando reprodutibilidade:", rng2.rand(3))
# [0.37454012 0.95071431 0.73199394]

# API mais recente (NumPy >= 1.17)
# Generator é a implementação moderna recomendada
from numpy.random import Generator, PCG64
rng_new = Generator(PCG64(42))
print("Usando novo gerador:", rng_new.random(3))
# [exemplo: 0.63696169 0.26978671 0.04097352]
```

## Entrada e Saída de Dados

### Salvando e Carregando Arrays

```python
# Criando um array para salvar
arr = np.array([[1, 2, 3], [4, 5, 6]])

# Salvando no formato binário do NumPy (.npy)
np.save('meu_array.npy', arr)

# Carregando o array
arr_carregado = np.load('meu_array.npy')
print("Array carregado do arquivo .npy:")
print(arr_carregado)
# [[1 2 3]
#  [4 5 6]]

# Salvando múltiplos arrays em um único arquivo (.npz)
x = np.array([1, 2, 3])
y = np.array([4, 5, 6])
np.savez('multiplos_arrays.npz', x=x, y=y)

# Carregando múltiplos arrays
dados = np.load('multiplos_arrays.npz')
print("Array x:", dados['x'])  # [1 2 3]
print("Array y:", dados['y'])  # [4 5 6]

# Salvando em formato de texto
np.savetxt('meu_array.csv', arr, delimiter=',')

# Carregando dados de texto
arr_texto = np.loadtxt('meu_array.csv', delimiter=',')
print("Array carregado do arquivo .csv:")
print(arr_texto)
# [[1. 2. 3.]
#  [4. 5. 6.]]
```

### Manipulação de Dados CSV

```python
# Criando dados para demonstração
dados = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Salvando em CSV
np.savetxt('dados.csv', dados, delimiter=',', fmt='%d', header='col1,col2,col3')

# Carregando do CSV
dados_csv = np.loadtxt('dados.csv', delimiter=',', skiprows=1)  # skiprows=1 para pular o cabeçalho
print(dados_csv)
# [[1. 2. 3.]
#  [4. 5. 6.]
#  [7. 8. 9.]]

# Para dados com tipos mistos, use genfromtxt
dados_mistos = np.array([
    ('Alice', 25, 63.5),
    ('Bob', 32, 78.2),
    ('Charlie', 18, 71.0)
], dtype=[('nome', 'U10'), ('idade', 'i4'), ('peso', 'f4')])
np.savetxt('dados_mistos.csv', dados_mistos, delimiter=',', fmt=['%s', '%d', '%.1f'], header='nome,idade,peso')

# Carregando dados mistos
dados_mistos_lidos = np.genfromtxt('dados_mistos.csv', delimiter=',', names=True, dtype=None, encoding='utf-8')
print("Nomes:", dados_mistos_lidos['nome'])
print("Idades:", dados_mistos_lidos['idade'])
print("Pesos:", dados_mistos_lidos['peso'])
```

## Exemplos Práticos

### Processamento de Imagens Simples

```python
# Criando uma imagem simples (matriz 2D)
imagem = np.zeros((5, 5))
imagem[1:4, 1:4] = 1  # Quadrado branco no meio
print("Imagem original:")
print(imagem)
# [[0. 0. 0. 0. 0.]
#  [0. 1. 1. 1. 0.]
#  [0. 1. 1. 1. 0.]
#  [0. 1. 1. 1. 0.]
#  [0. 0. 0. 0. 0.]]

# Rotacionando a imagem 90 graus
imagem_rotacionada = np.rot90(imagem)
print("Imagem rotacionada:")
print(imagem_rotacionada)

# Espelhando a imagem horizontalmente
imagem_espelhada = np.fliplr(imagem)
print("Imagem espelhada horizontalmente:")
print(imagem_espelhada)

# Aplicando um filtro (kernel de convolução)
kernel = np.array([[0, 1, 0], [1, -4, 1], [0, 1, 0]])
from scipy import signal
imagem_filtrada = signal.convolve2d(imagem, kernel, mode='same', boundary='wrap')
print("Imagem após filtro (detector de bordas):")
print(imagem_filtrada)
```

### Análise de Dados Simples

```python
# Criando dados de exemplo (notas de alunos em 3 disciplinas)
notas = np.array([
    [8.5, 7.0, 9.5],  # Aluno 1
    [9.0, 8.5, 7.5],  # Aluno 2
    [6.5, 7.5, 8.0],  # Aluno 3
    [8.0, 5.0, 7.0],  # Aluno 4
    [7.5, 8.0, 8.5]   # Aluno 5
])

# Média de cada aluno
medias_alunos = np.mean(notas, axis=1)
print("Média de cada aluno:", medias_alunos)
# [8.33333333 8.33333333 7.33333333 6.66666667 8.        ]

# Média de cada disciplina
medias_disciplinas = np.mean(notas, axis=0)
print("Média de cada disciplina:", medias_disciplinas)
# [7.9 7.2 8.1]

# Nota mais alta e mais baixa
nota_mais_alta = np.max(notas)
nota_mais_baixa = np.min(notas)
print(f"Nota mais alta: {nota_mais_alta}, Nota mais baixa: {nota_mais_baixa}")
# Nota mais alta: 9.5, Nota mais baixa: 5.0

# Aluno com a maior média
indice_melhor_aluno = np.argmax(medias_alunos)
print(f"Índice do aluno com maior média: {indice_melhor_aluno}")
# Índice do aluno com maior média: 0 ou 1 (ambos têm média 8.33)

# Disciplina com melhor desempenho geral
indice_melhor_disciplina = np.argmax(medias_disciplinas)
print(f"Índice da disciplina com melhor desempenho: {indice_melhor_disciplina}")
# Índice da disciplina com melhor desempenho: 2

# Encontrando alunos com notas acima de 8.0
alunos_acima_oito = np.where(medias_alunos > 8.0)
print("Índices dos alunos com média acima de 8.0:", alunos_acima_oito[0])
# Índices dos alunos com média acima de 8.0: [0 1 4]

# Normalizando as notas (para escala de 0 a 1)
notas_normalizadas = (notas - nota_mais_baixa) / (nota_mais_alta - nota_mais_baixa)
print("Notas normalizadas:")
print(notas_normalizadas)
```

### Resolução de Sistemas de Equações

```python
# Exemplo: Sistema de equações
# 2x + y - z = 8
# -3x - y + 2z = -11
# -2x + y + 2z = -3

# Matriz de coeficientes
A = np.array([[2, 1, -1], [-3, -1, 2], [-2, 1, 2]])
# Vetor de constantes
b = np.array([8, -11, -3])

# Resolvendo o sistema
x = np.linalg.solve(A, b)
print("Solução do sistema (x, y, z):", x)
# [2. 3. -1.]

# Verificação
print("Verificação A @ x:", A @ x)
# [8. -11. -3.]

# Resolvendo também usando a matriz inversa
x_inv = np.linalg.inv(A) @ b
print("Solução usando matriz inversa:", x_inv)
# [2. 3. -1.]
```

### Ajuste de Curvas

```python
# Gerando dados sintéticos com ruído
np.random.seed(42)
x = np.linspace(0, 10, 20)
y_true = 3 * x + 2  # Função verdadeira: y = 3x + 2
ruido = np.random.normal(0, 1.5, 20)
y = y_true + ruido  # Dados observados com ruído

# Ajuste polinomial de grau 1 (regressão linear)
coeficientes = np.polyfit(x, y, 1)
print("Coeficientes do ajuste linear:", coeficientes)
# Exemplo: [2.97... 2.13...] (próximo de 3x + 2)

# Criando a função ajustada
y_ajustado = np.polyval(coeficientes, x)

# Calculando o erro quadrático médio
erro = np.mean((y - y_ajustado) ** 2)
print(f"Erro quadrático médio: {erro:.4f}")

# Visualização do ajuste (requer matplotlib)
# import matplotlib.pyplot as plt
# plt.figure(figsize=(10, 6))
# plt.scatter(x, y, label='Dados com ruído')
# plt.plot(x, y_true, 'r-', label='Função verdadeira (y = 3x + 2)')
# plt.plot(x, y_ajustado, 'g--', label='Função ajustada')
# plt.legend()
# plt.xlabel('x')
# plt.ylabel('y')
# plt.title('Ajuste de Curva Linear')
# plt.grid(True)
# plt.show()
```

## Dicas e Truques

### Otimização de Memória

```python
# Arrays grandes podem consumir muita memória
# Aqui estão algumas técnicas para otimizar o uso de memória

# 1. Escolha o tipo de dado adequado
# Criando um array com tipo int64 (padrão em muitos sistemas)
arr_int64 = np.arange(10, dtype=np.int64)
print(f"Tamanho com int64: {arr_int64.nbytes} bytes")

# Usando int8 para números pequenos
arr_int8 = np.arange(10, dtype=np.int8)
print(f"Tamanho com int8: {arr_int8.nbytes} bytes")

# 2. Usando views em vez de cópias
a = np.arange(1000000)
# View (não aloca nova memória)
b = a[::2]  # View de elementos pares
print("b é view de a:", b.base is a)

# 3. Usando strides para operações especializadas
# Strides representam o número de bytes para pular em cada dimensão
arr = np.ones((1000, 1000))
print(f"Forma: {arr.shape}, Strides: {arr.strides}")
```

### Performance e Vetorização

```python
# Vetorização é fundamental para performance no NumPy
# Compare operações vetorizadas vs loops:

# Criando arrays para teste
n = 1000000
a = np.random.random(n)
b = np.random.random(n)

# Multiplicação com loop Python
def multiplicacao_loop(a, b):
    resultado = np.zeros_like(a)
    for i in range(len(a)):
        resultado[i] = a[i] * b[i]
    return resultado

# Multiplicação vetorizada do NumPy
def multiplicacao_numpy(a, b):
    return a * b

# Comparando performance (usando funções mágicas do IPython)
# %timeit multiplicacao_loop(a, b)  # Geralmente muito lento
# %timeit multiplicacao_numpy(a, b)  # Geralmente muito rápido

# Dica: Sempre evite loops Python quando puder usar operações vetorizadas
```

### Técnicas Avançadas

```python
# 1. Broadcasting avançado
# Expandindo dimensões com np.newaxis (equivalente a None)
x = np.array([1, 2, 3])
# Adicionando uma dimensão para transformar vetor linha em coluna
x_col = x[:, np.newaxis]  # ou x[:, None]
print(x_col.shape)  # (3, 1)
print(x_col)
# [[1]
#  [2]
#  [3]]

# 2. Operações de máscara
dados = np.array([1, 2, -999, 4, -999, 6])  # -999 representa valores ausentes
mascara = dados != -999
dados_limpos = dados[mascara]
print("Dados sem valores ausentes:", dados_limpos)  # [1 2 4 6]

# Substituindo valores
dados_corrigidos = np.where(dados == -999, np.nan, dados)
print("Dados com NaN:", dados_corrigidos)  # [ 1.  2. nan  4. nan  6.]

# 3. Axis como parâmetro flexível
arr_3d = np.random.random((3, 4, 5))
# Soma em cada dimensão
print("Forma original:", arr_3d.shape)  # (3, 4, 5)
print("Soma no eixo 0:", np.sum(arr_3d, axis=0).shape)  # (4, 5)
print("Soma no eixo 1:", np.sum(arr_3d, axis=1).shape)  # (3, 5)
print("Soma no eixo 2:", np.sum(arr_3d, axis=2).shape)  # (3, 4)
print("Soma em múltiplos eixos:", np.sum(arr_3d, axis=(0, 2)).shape)  # (4,)
```

## Comparação com Python Puro

NumPy oferece muitas vantagens sobre as estruturas de dados padrão do Python para computação numérica:

### Exemplo Comparativo: Cálculo de Distância Euclidiana

```python
import time

# Criando dados
n = 1000000
lista_python1 = list(range(n))
lista_python2 = list(range(n, 2*n))
array_numpy1 = np.array(lista_python1)
array_numpy2 = np.array(lista_python2)

# Calculando distância euclidiana com Python puro
inicio = time.time()
soma = 0
for i in range(n):
    soma += (lista_python1[i] - lista_python2[i]) ** 2
distancia_python = soma ** 0.5
tempo_python = time.time() - inicio

# Calculando distância euclidiana com NumPy
inicio = time.time()
distancia_numpy = np.sqrt(np.sum((array_numpy1 - array_numpy2) ** 2))
tempo_numpy = time.time() - inicio

print(f"Python puro: {tempo_python:.6f} segundos")
print(f"NumPy: {tempo_numpy:.6f} segundos")
print(f"NumPy é {tempo_python/tempo_numpy:.1f}x mais rápido")
```

### Eficiência de Memória

```python
# Comparação do uso de memória
import sys

# Criando uma lista Python com 1 milhão de inteiros
lista_python = list(range(1000000))
# Criando um array NumPy com 1 milhão de inteiros
array_numpy = np.arange(1000000, dtype=np.int32)

# Tamanho em memória
tamanho_lista = sys.getsizeof(lista_python) + sum(sys.getsizeof(i) for i in lista_python[:5]) * len(lista_python) // 5
tamanho_array = array_numpy.nbytes

print(f"Tamanho estimado da lista Python: {tamanho_lista / 1024 / 1024:.2f} MB")
print(f"Tamanho do array NumPy: {tamanho_array / 1024 / 1024:.2f} MB")
print(f"Lista Python usa {tamanho_lista / tamanho_array:.1f}x mais memória")
```

### Expressividade de Código

```python
# Comparação de expressividade de código para operações comuns

# Exemplo: Calculando a média de valores acima de um limiar

# Dados de exemplo
n = 1000000
dados = np.random.normal(0, 1, n)
limiar = 0.5

# Python puro
inicio = time.time()
valores_filtrados = [x for x in dados if x > limiar]
media_python = sum(valores_filtrados) / len(valores_filtrados)
tempo_python = time.time() - inicio

# NumPy
inicio = time.time()
mascara = dados > limiar
media_numpy = np.mean(dados[mascara])
tempo_numpy = time.time() - inicio

print(f"Python puro: {tempo_python:.6f} segundos")
print(f"NumPy: {tempo_numpy:.6f} segundos")
print(f"NumPy é {tempo_python/tempo_numpy:.1f}x mais rápido")
```

## Recursos Adicionais

### Sites Oficiais e Documentação

- [Site oficial do NumPy](https://numpy.org/)
- [Documentação completa do NumPy](https://numpy.org/doc/stable/)
- [Tutorial NumPy para iniciantes](https://numpy.org/doc/stable/user/absolute_beginners.html)
- [Referência da API NumPy](https://numpy.org/doc/stable/reference/)

### Livros Recomendados

- "Python for Data Analysis" por Wes McKinney
- "Numerical Python" por Robert Johansson
- "Python Data Science Handbook" por Jake VanderPlas

### Tutoriais e Cursos Online

- [DataCamp: Intro to Python for Data Science](https://www.datacamp.com/)
- [Coursera: Python para Ciência de Dados](https://www.coursera.org/)
- [edX: Python para Ciência de Dados](https://www.edx.org/)

### Bibliotecas Relacionadas

- [SciPy](https://scipy.org/) - Estende NumPy com mais funcionalidades científicas
- [Pandas](https://pandas.pydata.org/) - Análise de dados com estruturas de dados baseadas em NumPy
- [Matplotlib](https://matplotlib.org/) - Visualização de dados, funciona bem com arrays NumPy
- [Scikit-learn](https://scikit-learn.org/) - Aprendizado de máquina baseado em NumPy

---

## Conclusão

NumPy é a base fundamental para computação científica e análise de dados em Python. Sua eficiência, combinada com a sintaxe simples e poderosa, torna-o indispensável para qualquer trabalho envolvendo dados numéricos. Este guia apresentou os conceitos principais, mas NumPy tem ainda mais recursos avançados a serem explorados.

À medida que você se familiariza com NumPy, verá como ele simplifica tarefas complexas e acelera drasticamente seus cálculos numéricos, permitindo que você foque nos problemas que está tentando resolver, em vez de se preocupar com detalhes de implementação de baixo nível.
