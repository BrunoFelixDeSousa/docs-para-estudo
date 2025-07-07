# [⬅ Voltar para o índice principal](../../../README.md)

# Python Mini Doc

## Sumário

1. [Introdução ao Python](#introdução-ao-python)
2. [Instalação e Configuração](#instalação-e-configuração)
3. [Sintaxe Básica](#sintaxe-básica)
4. [Tipos de Dados](#tipos-de-dados)
5. [Estruturas de Controle](#estruturas-de-controle)
6. [Funções](#funções)
7. [Módulos e Pacotes](#módulos-e-pacotes)
8. [Manipulação de Arquivos](#manipulação-de-arquivos)
9. [Orientação a Objetos](#orientação-a-objetos)
10. [Tratamento de Exceções](#tratamento-de-exceções)
11. [Bibliotecas Populares](#bibliotecas-populares)
12. [Boas Práticas](#boas-práticas)

## Introdução ao Python

Python é uma linguagem de programação de alto nível, interpretada, de script, imperativa, orientada a objetos, funcional, de tipagem dinâmica e forte. Foi lançada por Guido van Rossum em 1991 e atualmente encontra-se na versão 3.x.

### Características Principais

- Sintaxe clara e legível
- Tipagem dinâmica
- Gerenciamento automático de memória
- Multiplataforma
- Grande biblioteca padrão
- Comunidade ativa

## Instalação e Configuração

### Windows

1. Acesse [python.org](https://www.python.org/downloads/)
2. Baixe a versão mais recente para Windows
3. Execute o instalador e marque a opção "Add Python to PATH"
4. Verifique a instalação abrindo o prompt de comando e digitando:

```python
python --version

```

```bash
# Resultado: Python 3.13.1 (ou a versão que você instalou)
```

### Linux

A maioria das distribuições Linux já vem com Python instalado. Caso contrário:

```bash
sudo apt update
sudo apt install python3 python3-pip
```

## Sintaxe Básica

### Primeiro Programa

```python
print("Olá, mundo!")

# Olá, mundo!
```

### Comentários

```python
# Isto é um comentário de uma linha

"""
Isto é um comentário
de múltiplas linhas
"""
```

### Indentação

Python usa indentação para delimitar blocos de código, geralmente 4 espaços.

```python
if True:
    print("Bloco indentado")
    if True:
        print("Outro nível de indentação")

# Bloco indentado
# Outro nível de indentação
```

## Tipos de Dados

### Numéricos

```python
# Inteiros
x = 10
print(x, type(x))

# Ponto flutuante
y = 10.5
print(y, type(y))

# Complexos
z = 1 + 2j
print(z, type(z))

# Resultado:
# 10 <class 'int'>
# 10.5 <class 'float'>
# (1+2j) <class 'complex'>
```

### Strings

```python
# Strings básicas
nome = "Python"
print(nome)

# Concatenação
primeiro_nome = "Guido"
sobrenome = "van Rossum"
nome_completo = primeiro_nome + " " + sobrenome
print(nome_completo)

# Formatação
idade = 30
mensagem = f"Olá, eu tenho {idade} anos."
print(mensagem)

# Métodos de string
texto = "  python é incrível  "
print(texto.strip())
print(texto.upper())
print(texto.replace("incrível", "fantástico"))

# Resultado:
# Python
# Guido van Rossum
# Olá, eu tenho 30 anos.
# python é incrível
# PYTHON É INCRÍVEL
# python é fantástico
```

### Booleanos

```python
verdadeiro = True
falso = False
print(verdadeiro, type(verdadeiro))
print(falso, type(falso))

# Resultado:
# True <class 'bool'>
# False <class 'bool'>
```

# Slicing

O slicing (fatiamento) é uma das características mais poderosas e elegantes de Python para trabalhar com sequências como listas, strings, tuplas e outros objetos iteráveis. Este guia explora todos os aspectos do slicing, desde o básico até técnicas avançadas.

## Índice

- [Introdução ao Slicing](#introdução-ao-slicing)
- [Sintaxe Básica](#sintaxe-básica)
- [Slicing com Strings](#slicing-com-strings)
- [Slicing com Listas](#slicing-com-listas)
- [Slicing com Tuplas](#slicing-com-tuplas)
- [Índices Negativos](#índices-negativos)
- [Slicing com Passo](#slicing-com-passo)
- [Slicing Reverso](#slicing-reverso)
- [Casos Especiais de Slicing](#casos-especiais-de-slicing)
- [Modificando Sequências com Slicing](#modificando-sequências-com-slicing)
- [Slicing Multidimensional](#slicing-multidimensional)
- [Slicing com Objetos Personalizados](#slicing-com-objetos-personalizados)
- [Dicas e Boas Práticas](#dicas-e-boas-práticas)
- [Exercícios Práticos](#exercícios-práticos)

## Introdução ao Slicing

O slicing é uma técnica que permite extrair subconjuntos de sequências em Python. É uma operação extremamente comum e útil que funciona de maneira consistente em vários tipos de dados.

### O que é Slicing?

Slicing é a operação de extrair uma parte (ou "fatia") de uma sequência. Esta operação cria uma nova sequência com os elementos selecionados, sem modificar a sequência original.

### Tipos de Dados que Suportam Slicing

- Strings
- Listas
- Tuplas
- Range
- Bytes e Bytearray
- Arrays do módulo array
- Objetos personalizados que implementam o protocolo de sequência

## Sintaxe Básica

A sintaxe básica do slicing em Python é:

```python
sequencia[inicio:fim:passo]
```

Onde:

- `inicio`: o índice do primeiro elemento a ser incluído na fatia (inclusivo)
- `fim`: o índice do primeiro elemento a ser excluído da fatia (exclusivo)
- `passo`: o incremento entre os elementos (opcional, padrão é 1)

Qualquer um destes elementos pode ser omitido:

- `sequencia[inicio:fim]` - usa o passo padrão (1)
- `sequencia[inicio:]` - do índice início até o final
- `sequencia[:fim]` - do início até o índice fim (exclusivo)
- `sequencia[:]` - cópia completa da sequência
- `sequencia[::passo]` - toda a sequência com o passo especificado

## Slicing com Strings

Strings são sequências de caracteres e o slicing funciona perfeitamente com elas.

### Exemplos Básicos

```python
mensagem = "Python é incrível!"

# Extrair os primeiros 6 caracteres
primeiros_seis = mensagem[:6]  # "Python"

# Extrair a palavra "incrível"
palavra = mensagem[9:]  # "incrível!"

# Extrair uma substring do meio
meio = mensagem[7:9]  # "é "

# Criar uma cópia da string
copia = mensagem[:]  # "Python é incrível!"
```

### Casos de Uso Comuns

```python
# Remover última exclamação
sem_exclamacao = mensagem[:-1]  # "Python é incrível"

# Obter extensão de um arquivo
arquivo = "documento.pdf"
extensao = arquivo[arquivo.rfind("."):]  # ".pdf"

# Verificar se começa com um prefixo (alternativa ao startswith())
comeca_com_py = mensagem[:2] == "Py"  # True
```

## Slicing com Listas

Listas são sequências mutáveis que podem conter elementos de qualquer tipo.

### Exemplos Básicos

```python
numeros = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Primeiros três elementos
inicio = numeros[:3]  # [0, 1, 2]

# Últimos três elementos
fim = numeros[-3:]  # [7, 8, 9]

# Elementos do meio
meio = numeros[3:7]  # [3, 4, 5, 6]

# Cópia da lista
copia = numeros[:]  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Casos de Uso Comuns

```python
# Removendo o primeiro elemento
sem_primeiro = numeros[1:]  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Removendo o último elemento
sem_ultimo = numeros[:-1]  # [0, 1, 2, 3, 4, 5, 6, 7, 8]

# Pegando uma porção do meio
parte_do_meio = numeros[3:7]  # [3, 4, 5, 6]

# Criando uma cópia superficial da lista
lista_copiada = numeros[:]
```

## Slicing com Tuplas

Tuplas são similares às listas, mas são imutáveis. O slicing funciona da mesma forma:

```python
coordenadas = (10, 20, 30, 40, 50, 60)

# Primeiros dois valores
x_y = coordenadas[:2]  # (10, 20)

# Últimos dois valores
ultimos = coordenadas[-2:]  # (50, 60)

# Valores do meio
meio = coordenadas[2:4]  # (30, 40)
```

## Índices Negativos

Python permite usar índices negativos, que contam a partir do final da sequência:

| Índice | -6  | -5  | -4  | -3  | -2  | -1  |
| ------ | --- | --- | --- | --- | --- | --- |
| Valor  | P   | y   | t   | h   | o   | n   |
| Índice | 0   | 1   | 2   | 3   | 4   | 5   |

```python
palavra = "Python"

# Último caractere
ultimo = palavra[-1]  # "n"

# Penúltimo caractere
penultimo = palavra[-2]  # "o"

# Últimos três caracteres
ultimos_tres = palavra[-3:]  # "hon"

# Todos exceto os últimos dois
exceto_ultimos_dois = palavra[:-2]  # "Pyth"

# Do terceiro até o penúltimo
do_terceiro_ate_penultimo = palavra[2:-1]  # "tho"
```

## Slicing com Passo

O parâmetro de passo permite selecionar elementos com um determinado intervalo.

```python
numeros = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Elementos em posições pares (passo 2)
pares = numeros[::2]  # [0, 2, 4, 6, 8]

# Elementos em posições ímpares (passo 2, começando de 1)
impares = numeros[1::2]  # [1, 3, 5, 7, 9]

# A cada três elementos
a_cada_tres = numeros[::3]  # [0, 3, 6, 9]

# A cada dois elementos, entre os índices 1 e 8
selecionados = numeros[1:8:2]  # [1, 3, 5, 7]
```

## Slicing Reverso

Usando um passo negativo, podemos inverter a ordem dos elementos:

```python
texto = "Python"

# Inverter completamente
invertido = texto[::-1]  # "nohtyP"

# Últimos três caracteres em ordem reversa
ultimos_invertidos = texto[-1:-4:-1]  # "noh"

# Elementos de 2 em 2, em ordem reversa
reverso_intercalado = texto[::-2]  # "nhy"
```

### Interpretação de Passo Negativo

Com um passo negativo, os valores padrão de início e fim são invertidos:

- `inicio` padrão se torna o último elemento (à direita)
- `fim` padrão se torna antes do primeiro elemento (à esquerda)

```python
numeros = [0, 1, 2, 3, 4]

# Equivalentes:
print(numeros[::-1])  # [4, 3, 2, 1, 0]
print(numeros[4::-1])  # [4, 3, 2, 1, 0]

# Do segundo até o início em ordem reversa
print(numeros[1::-1])  # [1, 0]

# Dos dois últimos até o início em ordem reversa
print(numeros[-2::-1])  # [3, 2, 1, 0]
```

## Casos Especiais de Slicing

### Índices Fora dos Limites

Python não gera erros se o índice estiver fora dos limites durante o slicing, ele simplesmente ajusta ao início ou fim da sequência:

```python
lista = [1, 2, 3, 4, 5]

# Índice de fim maior que o comprimento da lista
print(lista[:10])  # [1, 2, 3, 4, 5]

# Índice de início maior que o comprimento da lista
print(lista[10:])  # []

# Ambos os índices fora dos limites
print(lista[-10:10])  # [1, 2, 3, 4, 5]
```

### Slices Vazios

Se o índice inicial é maior ou igual ao índice final (considerando o passo), o resultado será uma sequência vazia:

```python
lista = [1, 2, 3, 4, 5]

# Início maior que fim com passo positivo
print(lista[3:2])  # []

# Início menor que fim com passo negativo
print(lista[2:3:-1])  # []
```

## Modificando Sequências com Slicing

O slicing também pode ser usado para modificar sequências mutáveis.

### Atribuição para Slices

```python
# Substituir uma parte da lista
numeros = [0, 1, 2, 3, 4, 5]
numeros[2:4] = [20, 30]  # Resultado: [0, 1, 20, 30, 4, 5]

# Inserir elementos sem remover existentes
numeros = [0, 1, 2, 3, 4, 5]
numeros[2:2] = [10, 11]  # Resultado: [0, 1, 10, 11, 2, 3, 4, 5]

# Remover elementos (atribuindo lista vazia)
numeros = [0, 1, 2, 3, 4, 5]
numeros[2:4] = []  # Resultado: [0, 1, 4, 5]

# Substituir toda a lista
numeros = [0, 1, 2, 3, 4, 5]
numeros[:] = [9, 8, 7]  # Resultado: [9, 8, 7]
```

### Deletando com Slices

```python
numeros = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Remover os três primeiros elementos
del numeros[:3]  # Resultado: [3, 4, 5, 6, 7, 8, 9]

# Remover elementos com passo
del numeros[::2]  # Remove elementos em índices pares
```

## Slicing Multidimensional

O slicing também funciona com estruturas multidimensionais, como listas de listas:

```python
matriz = [
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12]
]

# Obter as duas primeiras linhas
primeiras_linhas = matriz[:2]  # [[1, 2, 3, 4], [5, 6, 7, 8]]

# Obter as duas primeiras colunas de todas as linhas
primeiras_colunas = [linha[:2] for linha in matriz]  # [[1, 2], [5, 6], [9, 10]]

# Slicing em duas dimensões (requer biblioteca como NumPy)
import numpy as np
np_matriz = np.array(matriz)
submatriz = np_matriz[:2, 1:3]  # array([[2, 3], [6, 7]])
```

## Slicing com Objetos Personalizados

Para implementar o slicing em classes personalizadas, é necessário implementar o método `__getitem__`.

```python
class MinhaSequencia:
    def __init__(self, dados):
        self.dados = dados

    def __getitem__(self, key):
        # Se key for um slice
        if isinstance(key, slice):
            inicio, fim, passo = key.indices(len(self.dados))
            return [self.dados[i] for i in range(inicio, fim, passo)]
        # Se key for um índice inteiro
        else:
            return self.dados[key]

# Uso
seq = MinhaSequencia([1, 2, 3, 4, 5])
print(seq[1:4])  # [2, 3, 4]
print(seq[::2])  # [1, 3, 5]
```

## Dicas e Boas Práticas

### Clareza vs. Concisão

Equilibre a clareza com a concisão. Slicing muito complexo pode ser difícil de ler:

```python
# Menos legível
data = linha[2][5:15][::-1].replace("-", "")[::2]

# Mais legível
substring = linha[2][5:15]  # Extrai parte relevante
invertida = substring[::-1]  # Inverte a string
sem_hifens = invertida.replace("-", "")  # Remove hifens
resultado = sem_hifens[::2]  # Pega caracteres alternados
```

### Evite Índices Mágicos

Use variáveis ou constantes para índices importantes em vez de "números mágicos":

```python
# Não tão bom
nome_completo = "João Silva Oliveira"
sobrenome = nome_completo[5:10]

# Melhor
espaco1 = nome_completo.find(" ") + 1
espaco2 = nome_completo.find(" ", espaco1)
sobrenome = nome_completo[espaco1:espaco2]
```

### Use Slicing para Cópias Seguras

```python
# Modificar a original afeta a referência
original = [1, 2, 3]
referencia = original
referencia.append(4)  # original também é modificada

# Usar slicing para criar uma cópia
original = [1, 2, 3]
copia = original[:]
copia.append(4)  # original não é modificada
```

### Técnica do Slider

Pensar no slice como um "slider" ajuda a visualizar o comportamento:

```
  +---+---+---+---+---+---+
  | P | y | t | h | o | n |
  +---+---+---+---+---+---+
    0   1   2   3   4   5
   -6  -5  -4  -3  -2  -1
```

Os índices apontam para os espaços _entre_ os elementos. `palavra[2:4]` pega os elementos entre as posições 2 e 4, ou seja, "th".

### Economize Memória

Cuidado com slices grandes em objetos grandes. Para obter apenas o valor de um único elemento, use indexação normal em vez de slicing:

```python
# Cria uma nova lista com um elemento
primeiro = lista_grande[:1]  # Menos eficiente

# Obtém apenas a referência ao elemento
primeiro = lista_grande[0]  # Mais eficiente
```

## Exercícios Práticos

### Exercício 1: Inverter Palavras

Escreva uma função que inverta cada palavra em uma frase, mas mantenha a ordem das palavras.

```python
def inverter_palavras(frase):
    palavras = frase.split()
    palavras_invertidas = [palavra[::-1] for palavra in palavras]
    return " ".join(palavras_invertidas)

# Teste
texto = "Python é divertido"
print(inverter_palavras(texto))  # "nohtyP é oditrevid"
```

### Exercício 2: Extrair Domínio de Email

Crie uma função que extraia o domínio de um endereço de email.

```python
def extrair_dominio(email):
    arroba_pos = email.find("@")
    if arroba_pos == -1:
        return "Email inválido"
    return email[arroba_pos+1:]

# Teste
print(extrair_dominio("usuario@exemplo.com"))  # "exemplo.com"
```

### Exercício 3: Rotação de Lista

Implemente uma função que rotacione uma lista k posições para a direita.

```python
def rotacionar_direita(lista, k):
    if not lista:
        return lista
    k = k % len(lista)  # Lidar com k maior que len(lista)
    return lista[-k:] + lista[:-k]

# Teste
numeros = [1, 2, 3, 4, 5]
print(rotacionar_direita(numeros, 2))  # [4, 5, 1, 2, 3]
```

### Exercício 4: Palíndromo por Slicing

Crie uma função que verifique se uma string é um palíndromo usando slicing.

```python
def eh_palindromo(texto):
    # Remove espaços e converte para minúsculas
    texto = texto.lower().replace(" ", "")
    return texto == texto[::-1]

# Teste
print(eh_palindromo("Ana"))  # True
print(eh_palindromo("radar"))  # True
print(eh_palindromo("Python"))  # False
```

### Exercício 5: Matriz Transposta

Escreva uma função que calcule a transposta de uma matriz usando slicing.

```python
def transpor_matriz(matriz):
    # Verifica se a matriz está vazia
    if not matriz:
        return []

    # Cria a transposta usando slicing e compreensão de lista
    return [[linha[i] for linha in matriz] for i in range(len(matriz[0]))]

# Teste
matriz_original = [
    [1, 2, 3],
    [4, 5, 6]
]
transposta = transpor_matriz(matriz_original)
# Resultado: [[1, 4], [2, 5], [3, 6]]
```

### Listas

## Sumário

- [Introdução às Listas](#introdução-às-listas)
- [Criando Listas](#criando-listas)
- [Acessando Elementos](#acessando-elementos)
- [Fatiamento de Listas](#fatiamento-de-listas)
- [Métodos de Listas](#métodos-de-listas)
- [Listas por Compreensão](#listas-por-compreensão)
- [Ordenação de Listas](#ordenação-de-listas)
- [Copiando Listas](#copiando-listas)
- [Matrizes com Listas](#matrizes-com-listas)
- [Funções Úteis para Listas](#funções-úteis-para-listas)
- [Técnicas Avançadas](#técnicas-avançadas)
- [Problemas Comuns e Soluções](#problemas-comuns-e-soluções)
- [Exercícios Práticos](#exercícios-práticos)

## Introdução às Listas

As listas são uma das estruturas de dados mais versáteis e frequentemente utilizadas em Python. Elas permitem armazenar múltiplos itens em uma única variável, e são mutáveis, o que significa que podemos alterar seu conteúdo após a criação.

### Características das Listas em Python

- **Ordenadas**: Mantêm a ordem de inserção dos elementos
- **Mutáveis**: Podem ser modificadas após a criação
- **Indexadas**: Os elementos podem ser acessados por índices
- **Heterogêneas**: Podem conter elementos de diferentes tipos de dados
- **Permitem duplicatas**: Podem conter valores repetidos

## Criando Listas

### Sintaxe Básica

```python
# Lista vazia
lista_vazia = []

# Lista com elementos
numeros = [1, 2, 3, 4, 5]
nomes = ["Ana", "Bruno", "Carlos", "Daniela"]
mista = [1, "texto", 3.14, True, [1, 2]]
```

### Usando a Função list()

```python
# Convertendo outros tipos em listas
lista_de_string = list("Python")  # ['P', 'y', 't', 'h', 'o', 'n']
lista_de_tupla = list((1, 2, 3))  # [1, 2, 3]

# Lista com elementos repetidos
lista_repetida = [0] * 5  # [0, 0, 0, 0, 0]
```

### Usando range()

```python
numeros = list(range(5))  # [0, 1, 2, 3, 4]
pares = list(range(0, 10, 2))  # [0, 2, 4, 6, 8]
```

## Acessando Elementos

### Índices Positivos e Negativos

```python
frutas = ["maçã", "banana", "laranja", "uva", "manga"]

# Índices positivos (começam em 0)
primeira_fruta = frutas[0]  # "maçã"
terceira_fruta = frutas[2]  # "laranja"

# Índices negativos (começam em -1)
ultima_fruta = frutas[-1]  # "manga"
penultima_fruta = frutas[-2]  # "uva"
```

### Verificando a Existência de Elementos

```python
frutas = ["maçã", "banana", "laranja", "uva", "manga"]

# Usando o operador 'in'
tem_banana = "banana" in frutas  # True
tem_abacaxi = "abacaxi" in frutas  # False

# Verificando se não existe
nao_tem_abacaxi = "abacaxi" not in frutas  # True
```

## Fatiamento de Listas

O fatiamento (slicing) permite extrair parte de uma lista.

### Sintaxe de Fatiamento

```python
lista[inicio:fim:passo]
```

- **inicio**: Índice onde a fatia começa (inclusivo)
- **fim**: Índice onde a fatia termina (exclusivo)
- **passo**: Valor do incremento entre elementos

### Exemplos

```python
numeros = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Fatiamento básico
tres_primeiros = numeros[0:3]  # [0, 1, 2]
do_terceiro_ao_quinto = numeros[2:5]  # [2, 3, 4]

# Omitindo índices
do_inicio_ao_quarto = numeros[:4]  # [0, 1, 2, 3]
do_sexto_ate_o_fim = numeros[5:]  # [5, 6, 7, 8, 9]
copia_completa = numeros[:]  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Usando passo
pares = numeros[0:10:2]  # [0, 2, 4, 6, 8]
impares = numeros[1:10:2]  # [1, 3, 5, 7, 9]

# Passo negativo (inverso)
inverso = numeros[::-1]  # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

## Métodos de Listas

Python oferece diversos métodos para manipular listas.

### Adicionando Elementos

```python
frutas = ["maçã", "banana"]

# Adicionar elemento ao final
frutas.append("laranja")  # ["maçã", "banana", "laranja"]

# Inserir em posição específica
frutas.insert(1, "morango")  # ["maçã", "morango", "banana", "laranja"]

# Estender com outra lista
mais_frutas = ["uva", "manga"]
frutas.extend(mais_frutas)  # ["maçã", "morango", "banana", "laranja", "uva", "manga"]

# Alternativa para estender
frutas += ["abacaxi"]  # ["maçã", "morango", "banana", "laranja", "uva", "manga", "abacaxi"]
```

### Removendo Elementos

```python
frutas = ["maçã", "banana", "laranja", "uva", "banana"]

# Remover por valor (primeira ocorrência)
frutas.remove("banana")  # ["maçã", "laranja", "uva", "banana"]

# Remover por índice e retornar o valor
fruta_removida = frutas.pop(1)  # fruta_removida = "laranja", frutas = ["maçã", "uva", "banana"]

# Pop sem índice remove o último elemento
ultima = frutas.pop()  # ultima = "banana", frutas = ["maçã", "uva"]

# Limpar a lista completamente
frutas.clear()  # []
```

### Encontrando Elementos

```python
numeros = [10, 20, 30, 40, 20, 50]

# Encontrar índice da primeira ocorrência
indice = numeros.index(20)  # 1

# Encontrar a partir de um índice específico
indice = numeros.index(20, 2)  # 4

# Contar ocorrências
quantidade = numeros.count(20)  # 2
```

### Outros Métodos Úteis

```python
numeros = [3, 1, 4, 2, 5]

# Inverter a lista in-place
numeros.reverse()  # [5, 2, 4, 1, 3]

# Ordenar a lista in-place
numeros.sort()  # [1, 2, 3, 4, 5]

# Ordenar em ordem decrescente
numeros.sort(reverse=True)  # [5, 4, 3, 2, 1]
```

## Listas por Compreensão

As listas por compreensão (list comprehensions) são uma forma concisa e elegante de criar listas.

### Sintaxe Básica

```python
[expressão for item in iterável]
```

### Exemplos Simples

```python
# Criar lista de quadrados
quadrados = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Converter strings para maiúsculas
nomes = ["ana", "bruno", "carlos"]
maiusculos = [nome.upper() for nome in nomes]  # ["ANA", "BRUNO", "CARLOS"]
```

### Com Condicionais

```python
# Filtrar apenas números pares
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
pares = [n for n in numeros if n % 2 == 0]  # [2, 4, 6, 8, 10]

# Com if-else
status = ["par" if n % 2 == 0 else "ímpar" for n in numeros]
# ["ímpar", "par", "ímpar", "par", "ímpar", "par", "ímpar", "par", "ímpar", "par"]
```

### Compreensões Aninhadas

```python
# Matriz 3x3
matriz = [[i * 3 + j + 1 for j in range(3)] for i in range(3)]
# [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Aplainar uma matriz
matriz = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
valores = [valor for linha in matriz for valor in linha]
# [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## Ordenação de Listas

Python oferece diferentes formas de ordenar listas.

### Método sort()

O método `sort()` ordena a lista original "in-place", ou seja, modifica a lista diretamente sem criar uma nova.

```python
numeros = [3, 1, 4, 1, 5, 9, 2, 6]

# Ordenação simples
numeros.sort()  # [1, 1, 2, 3, 4, 5, 6, 9]

# Ordenação decrescente
numeros.sort(reverse=True)  # [9, 6, 5, 4, 3, 2, 1, 1]
```

### Função sorted()

A função `sorted()` retorna uma nova lista ordenada, sem modificar a original.

```python
numeros = [3, 1, 4, 1, 5, 9, 2, 6]

# Criar nova lista ordenada
ordenados = sorted(numeros)  # ordenados = [1, 1, 2, 3, 4, 5, 6, 9], numeros permanece inalterado

# Ordem decrescente
decrescente = sorted(numeros, reverse=True)  # [9, 6, 5, 4, 3, 2, 1, 1]
```

### Ordenação Personalizada

```python
# Ordenar por comprimento da string
palavras = ["banana", "maçã", "laranja", "uva", "abacaxi"]
palavras.sort(key=len)  # ["uva", "maçã", "banana", "laranja", "abacaxi"]

# Ordenar ignorando maiúsculas/minúsculas
nomes = ["Ana", "carlos", "Bruno", "daniela"]
nomes.sort(key=str.lower)  # ["Ana", "Bruno", "carlos", "daniela"]

# Usando lambdas para ordenações complexas
alunos = [
    {"nome": "Ana", "nota": 9.5},
    {"nome": "Bruno", "nota": 8.2},
    {"nome": "Carlos", "nota": 9.8}
]
alunos.sort(key=lambda aluno: aluno["nota"], reverse=True)
# Ordenado por nota decrescente: Carlos (9.8), Ana (9.5), Bruno (8.2)
```

## Copiando Listas

Em Python, a atribuição simples não cria uma cópia da lista, apenas uma referência.

### Referência vs Cópia

```python
# Referência (não é uma cópia)
lista1 = [1, 2, 3]
lista2 = lista1  # Cria uma referência, não uma cópia

lista1.append(4)
print(lista2)  # [1, 2, 3, 4] - lista2 também foi modificada
```

### Métodos para Criar Cópias

```python
original = [1, 2, 3]

# Método 1: Fatiamento
copia1 = original[:]

# Método 2: Função list()
copia2 = list(original)

# Método 3: Método copy()
copia3 = original.copy()

# Testando
original.append(4)
print(original)  # [1, 2, 3, 4]
print(copia1)    # [1, 2, 3]
print(copia2)    # [1, 2, 3]
print(copia3)    # [1, 2, 3]
```

### Cópia Profunda (Deep Copy)

Para listas que contêm outras listas ou objetos mutáveis, é necessário usar cópia profunda.

```python
import copy

# Lista com sublista
original = [1, 2, [3, 4]]

# Cópia rasa
copia_rasa = original.copy()

# Cópia profunda
copia_profunda = copy.deepcopy(original)

# Modificando a sublista
original[2].append(5)

print(original)       # [1, 2, [3, 4, 5]]
print(copia_rasa)     # [1, 2, [3, 4, 5]] - a sublista foi modificada!
print(copia_profunda) # [1, 2, [3, 4]] - a sublista permanece inalterada
```

## Matrizes com Listas

As listas podem ser aninhadas para criar estruturas multidimensionais como matrizes.

### Criando Matrizes

```python
# Matriz 3x3
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Criando matriz com list comprehension
matriz_zeros = [[0 for _ in range(3)] for _ in range(3)]
# [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
```

### Acessando Elementos da Matriz

```python
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Acessando elemento da linha 1, coluna 2
elemento = matriz[1][2]  # 6

# Modificando um elemento
matriz[0][1] = 10  # A matriz se torna [[1, 10, 3], [4, 5, 6], [7, 8, 9]]
```

### Operações com Matrizes

```python
# Percorrer uma matriz
matriz = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

for i in range(len(matriz)):
    for j in range(len(matriz[i])):
        print(f"matriz[{i}][{j}] = {matriz[i][j]}")

# Transpor uma matriz
transposta = [[matriz[j][i] for j in range(len(matriz))] for i in range(len(matriz[0]))]
# [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

# Somar matrizes
matriz_a = [[1, 2], [3, 4]]
matriz_b = [[5, 6], [7, 8]]
soma = [[matriz_a[i][j] + matriz_b[i][j] for j in range(len(matriz_a[0]))] for i in range(len(matriz_a))]
# [[6, 8], [10, 12]]
```

## Funções Úteis para Listas

Python oferece diversas funções integradas para trabalhar com listas.

### Funções Matemáticas

```python
numeros = [3, 1, 4, 1, 5, 9, 2]

# Soma de todos os elementos
total = sum(numeros)  # 25

# Valor mínimo
minimo = min(numeros)  # 1

# Valor máximo
maximo = max(numeros)  # 9

# Média (não é função integrada)
media = sum(numeros) / len(numeros)  # 3.5714...
```

### Funções de Utilidade

```python
# Comprimento da lista
tamanho = len([1, 2, 3, 4])  # 4

# Enumerar elementos com índices
for indice, valor in enumerate(["a", "b", "c"]):
    print(f"Índice {indice}: {valor}")
# Índice 0: a
# Índice 1: b
# Índice 2: c

# Enumerar com início personalizado
for indice, valor in enumerate(["a", "b", "c"], start=1):
    print(f"Item {indice}: {valor}")
# Item 1: a
# Item 2: b
# Item 3: c

# Combinando duas listas
for nome, idade in zip(["Ana", "Bruno", "Carlos"], [25, 30, 35]):
    print(f"{nome} tem {idade} anos")
# Ana tem 25 anos
# Bruno tem 30 anos
# Carlos tem 35 anos
```

### Aplicando Funções em Todos os Elementos

```python
# Usando map (retorna um iterador, converter para lista)
numeros = [1, 2, 3, 4, 5]
quadrados = list(map(lambda x: x**2, numeros))  # [1, 4, 9, 16, 25]

# Filtrando elementos
pares = list(filter(lambda x: x % 2 == 0, numeros))  # [2, 4]

# Alternativa com list comprehension
quadrados = [x**2 for x in numeros]  # [1, 4, 9, 16, 25]
pares = [x for x in numeros if x % 2 == 0]  # [2, 4]
```

## Técnicas Avançadas

### Desempacotamento de Listas

```python
# Atribuição múltipla
primeiro, segundo, terceiro = [1, 2, 3]

# Com o operador *
primeiro, *meio, ultimo = [1, 2, 3, 4, 5]
# primeiro = 1, meio = [2, 3, 4], ultimo = 5

# Desempacotamento em funções
numeros = [1, 2, 3, 4, 5]
maximo = max(*numeros)  # Equivalente a max(1, 2, 3, 4, 5)
```

### Listas como Pilhas e Filas

```python
# Usando lista como pilha (LIFO: Last In, First Out)
pilha = []
pilha.append(1)  # Adiciona ao topo
pilha.append(2)
pilha.append(3)
item = pilha.pop()  # Remove do topo, item = 3, pilha = [1, 2]

# Usando lista como fila (FIFO: First In, First Out)
# Obs: Para filas, é mais eficiente usar collections.deque
from collections import deque
fila = deque([1, 2, 3])
fila.append(4)  # Adiciona ao final
item = fila.popleft()  # Remove do início, item = 1, fila = deque([2, 3, 4])
```

### Comparação de Listas

```python
# Verificar igualdade
lista1 = [1, 2, 3]
lista2 = [1, 2, 3]
sao_iguais = lista1 == lista2  # True

# Comparação lexicográfica
a = [1, 2, 3]
b = [1, 2, 4]
resultado = a < b  # True (compara elemento por elemento)
```

## Problemas Comuns e Soluções

### Problema 1: Referência Compartilhada

```python
# PROBLEMA
lista_de_listas = [[0]] * 3  # [[0], [0], [0]]
lista_de_listas[0].append(1)  # Modifica todas as sublistas: [[0, 1], [0, 1], [0, 1]]

# SOLUÇÃO
lista_de_listas = [[] for _ in range(3)]  # Cria três listas independentes
lista_de_listas[0].append(1)  # Apenas a primeira é modificada: [[1], [], []]
```

### Problema 2: Modificar Durante Iteração

```python
# PROBLEMA - Não funciona corretamente
numeros = [1, 2, 3, 4, 5]
for numero in numeros:
    if numero % 2 == 0:
        numeros.remove(numero)  # Causa comportamento inesperado!

# SOLUÇÃO 1 - Criar nova lista
numeros = [1, 2, 3, 4, 5]
impares = [num for num in numeros if num % 2 != 0]  # [1, 3, 5]

# SOLUÇÃO 2 - Iterar pela cópia
numeros = [1, 2, 3, 4, 5]
for numero in numeros[:]:  # Cria uma cópia temporária para iteração
    if numero % 2 == 0:
        numeros.remove(numero)
```

### Problema 3: Duplicatas

```python
# Remover duplicatas mantendo a ordem
numeros = [1, 2, 3, 1, 2, 4, 5]

# Solução 1: set e list comprehension
unicos = []
for num in numeros:
    if num not in unicos:
        unicos.append(num)
# unicos = [1, 2, 3, 4, 5]

# Solução 2: usando dict.fromkeys() (mais eficiente)
unicos = list(dict.fromkeys(numeros))
# unicos = [1, 2, 3, 4, 5]
```

## Exercícios Práticos

### Exercício 1: Soma de Elementos

Crie uma função que calcule a soma de todos os elementos em uma lista, sem usar a função `sum()`.

```python
def soma_lista(lista):
    total = 0
    for elemento in lista:
        total += elemento
    return total

# Teste
numeros = [1, 2, 3, 4, 5]
resultado = soma_lista(numeros)  # 15
```

### Exercício 2: Maior e Menor Valores

Encontre o maior e o menor valor de uma lista sem usar as funções `min()` ou `max()`.

```python
def encontra_maior_menor(lista):
    if not lista:
        return None, None

    maior = menor = lista[0]

    for valor in lista[1:]:
        if valor > maior:
            maior = valor
        if valor < menor:
            menor = valor

    return maior, menor

# Teste
numeros = [3, 1, 8, 2, 5]
maior, menor = encontra_maior_menor(numeros)  # maior = 8, menor = 1
```

### Exercício 3: Inverter uma Lista

Escreva uma função para inverter uma lista sem usar as funções `reverse()` ou o fatiamento `[::-1]`.

```python
def inverter_lista(lista):
    resultado = []
    for i in range(len(lista) - 1, -1, -1):
        resultado.append(lista[i])
    return resultado

# Teste
original = [1, 2, 3, 4, 5]
invertida = inverter_lista(original)  # [5, 4, 3, 2, 1]
```

### Exercício 4: Contar Ocorrências

Crie uma função que conte as ocorrências de cada elemento em uma lista e retorne um dicionário.

```python
def contar_ocorrencias(lista):
    resultado = {}
    for item in lista:
        if item in resultado:
            resultado[item] += 1
        else:
            resultado[item] = 1
    return resultado

# Teste
itens = ["a", "b", "a", "c", "b", "a"]
contagem = contar_ocorrencias(itens)  # {"a": 3, "b": 2, "c": 1}
```

### Exercício 5: Filtrar por Critério

Implemente uma função que filtre elementos de uma lista com base em um critério.

```python
def filtrar_por_criterio(lista, criterio):
    resultado = []
    for item in lista:
        if criterio(item):
            resultado.append(item)
    return resultado

# Teste - filtrar números maiores que 3
numeros = [1, 5, 2, 8, 3, 9]
filtrados = filtrar_por_criterio(numeros, lambda x: x > 3)  # [5, 8, 9]
```

### Tuplas

Tuplas são como listas, mas imutáveis.

```python
coordenadas = (10, 20)
print(coordenadas)
print(coordenadas[0])

# Não é possível modificar uma tupla
# coordenadas[0] = 15  # Isso causaria um erro

# Resultado:
# (10, 20)
# 10
```

### Dicionários

```python
# Criação de dicionários
pessoa = {
    "nome": "João",
    "idade": 30,
    "cidade": "São Paulo"
}
print(pessoa)

# Acesso a elementos
print(pessoa["nome"])

# Adição/modificação de elementos
pessoa["profissao"] = "Programador"
print(pessoa)

# Métodos de dicionários
print(pessoa.keys())
print(pessoa.values())
print(pessoa.items())

# Verificação de chaves
if "idade" in pessoa:
    print(f"A idade é {pessoa['idade']}")

# Resultado:
# {'nome': 'João', 'idade': 30, 'cidade': 'São Paulo'}
# João
# {'nome': 'João', 'idade': 30, 'cidade': 'São Paulo', 'profissao':'Programador'}
# dict_keys(['nome', 'idade', 'cidade', 'profissao'])
# dict_values(['João', 30, 'São Paulo', 'Programador'])
# dict_items([('nome', 'João'), ('idade', 30), ('cidade', 'São Paulo'),('profissao', 'Programador')])*
# A idade é 30
```

### Conjuntos (Sets)

```python
# Criação de conjuntos
numeros = {1, 2, 3, 4, 5, 1, 2}  # Elementos duplicados são removidos
print(numeros)

# Operações com conjuntos
conjunto1 = {1, 2, 3}
conjunto2 = {3, 4, 5}

print(conjunto1.union(conjunto2))  # União
print(conjunto1.intersection(conjunto2))  # Interseção
print(conjunto1.difference(conjunto2))  # Diferença

# Resultado:
# {1, 2, 3, 4, 5}
# {1, 2, 3, 4, 5}
# {3}
# {1, 2}
```

## Estruturas de Controle

### Condicionais

```python
# If, elif, else
idade = 18

if idade < 18:
    print("Menor de idade")
elif idade == 18:
    print("Acabou de atingir a maioridade")
else:
    print("Maior de idade")

# Operador ternário
status = "Aprovado" if idade >= 18 else "Reprovado"
print(status)
```

_# Resultado:_
_# Acabou de atingir a maioridade_
_# Aprovado_

### Loops

#### Loop For

```python
# Iterando sobre uma lista
frutas = ["maçã", "banana", "laranja"]
for fruta in frutas:
    print(fruta)

# Iterando com range
for i in range(3):
    print(i)

# Enumerate para obter índice e valor
for indice, fruta in enumerate(frutas):
    print(f"Índice {indice}: {fruta}")
```

_# Resultado:_
_# maçã_
_# banana_
_# laranja_
_# 0_
_# 1_
_# 2_
_# Índice 0: maçã_
_# Índice 1: banana_
_# Índice 2: laranja_

#### Loop While

```python
contador = 0
while contador < 5:
    print(contador)
    contador += 1

# Break e continue
contador = 0
while True:
    contador += 1
    if contador == 3:
        continue  # Pula esta iteração
    print(contador)
    if contador == 5:
        break  # Sai do loop
```

_# Resultado:_
_# 0_
_# 1_
_# 2_
_# 3_
_# 4_
_# 1_
_# 2_
_# 4_
_# 5_

### Compreensões

```python
# Compreensão de lista
quadrados = [x**2 for x in range(5)]
print(quadrados)

# Compreensão de lista com condição
pares = [x for x in range(10) if x % 2 == 0]
print(pares)

# Compreensão de dicionário
quadrados_dict = {x: x**2 for x in range(5)}
print(quadrados_dict)

# Compreensão de conjunto
conjunto_pares = {x for x in range(10) if x % 2 == 0}
print(conjunto_pares)
```

_# Resultado:_
_# [0, 1, 4, 9, 16]_
_# [0, 2, 4, 6, 8]_
_# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}_
_# {0, 2, 4, 6, 8}_

## Funções

### Definição e Chamada

```python
# Função simples
def saudacao():
    print("Olá, mundo!")

saudacao()

# Função com parâmetros
def saudar(nome):
    print(f"Olá, {nome}!")

saudar("Maria")

# Função com valor de retorno
def soma(a, b):
    return a + b

resultado = soma(5, 3)
print(resultado)
```

_# Resultado:_
_# Olá, mundo!_
_# Olá, Maria!_
_# 8_

### Parâmetros com Valores Padrão

```python
def saudar(nome, mensagem="Olá"):
    print(f"{mensagem}, {nome}!")

saudar("João")
saudar("Maria", "Bem-vindo")
```

_# Resultado:_
_# Olá, João!_
_# Bem-vindo, Maria!_

### Args e Kwargs

```python
# *args para número variável de argumentos posicionais
def soma_varios(*args):
    return sum(args)

print(soma_varios(1, 2, 3, 4))

# **kwargs para número variável de argumentos nomeados
def exibir_info(**kwargs):
    for chave, valor in kwargs.items():
        print(f"{chave}: {valor}")

exibir_info(nome="João", idade=30, cidade="São Paulo")
```

_# Resultado:_
_# 10_
_# nome: João_
_# idade: 30_
_# cidade: São Paulo_

### Funções Lambda

```python
# Função lambda simples
quadrado = lambda x: x**2
print(quadrado(5))

# Usando lambda com funções de ordem superior
numeros = [1, 5, 4, 3, 2]
numeros_ordenados = sorted(numeros, key=lambda x: x)
print(numeros_ordenados)

# Usando lambda com filter
numeros_pares = list(filter(lambda x: x % 2 == 0, numeros))
print(numeros_pares)

# Usando lambda com map
numeros_ao_quadrado = list(map(lambda x: x**2, numeros))
print(numeros_ao_quadrado)
```

_# Resultado:_
_# 25_
_# [1, 2, 3, 4, 5]_
_# [4, 2]_
_# [1, 25, 16, 9, 4]_

## Módulos e Pacotes

### Importação de Módulos

```python
# Importando módulo completo
import math
print(math.sqrt(16))

# Importando funções específicas
from math import cos, sin
print(cos(0))

# Importando com alias
import datetime as dt
print(dt.datetime.now())

# Importando tudo (não recomendado)
from math import *
print(pi)
```

_# Resultado:_
_# 4.0_
_# 1.0_
_# 2025-04-01 12:34:56.789012_
_# 3.141592653589793_

### Criando Módulos

Arquivo `meu_modulo.py`:

```python
def saudacao(nome):
    return f"Olá, {nome}!"

PI = 3.14159
```

Arquivo principal:

```python
# Importando nosso módulo
import meu_modulo

print(meu_modulo.saudacao("João"))
print(meu_modulo.PI)
```

_# Resultado:_
_# Olá, João!_
_# 3.14159_

### Pacotes

Estrutura:

```
meu_pacote/
├── __init__.py
├── modulo1.py
└── modulo2.py
```

`__init__.py`:

```python
# Pode estar vazio ou conter código de inicialização
```

`modulo1.py`:

```python
def funcao1():
    return "Função 1"
```

`modulo2.py`:

```python
def funcao2():
    return "Função 2"
```

Arquivo principal:

```python
# Importando de pacotes
from meu_pacote import modulo1
print(modulo1.funcao1())

# Ou
import meu_pacote.modulo2
print(meu_pacote.modulo2.funcao2())
```

_# Resultado:_
_# Função 1_
_# Função 2_

## Manipulação de Arquivos

### Leitura de Arquivos

```python
# Abrindo e lendo um arquivo inteiro
with open("arquivo.txt", "r") as arquivo:
    conteudo = arquivo.read()
    print(conteudo)

# Lendo linha por linha
with open("arquivo.txt", "r") as arquivo:
    for linha in arquivo:
        print(linha.strip())

# Lendo todas as linhas em uma lista
with open("arquivo.txt", "r") as arquivo:
    linhas = arquivo.readlines()
    print(linhas)
```

_# Resultado (assumindo que arquivo.txt contenha "Linha 1\nLinha 2\nLinha 3"):_
_# Linha 1_
_# Linha 2_
_# Linha 3_
_# Linha 1_
_# Linha 2_
_# Linha 3_
_# ['Linha 1\n', 'Linha 2\n', 'Linha 3']_

### Escrita em Arquivos

```python
# Escrevendo em um arquivo
with open("saida.txt", "w") as arquivo:
    arquivo.write("Olá, mundo!\n")
    arquivo.write("Esta é a segunda linha.")

# Anexando conteúdo a um arquivo
with open("saida.txt", "a") as arquivo:
    arquivo.write("\nEsta é a terceira linha.")
```

_# Resultado (conteúdo de saida.txt):_
_# Olá, mundo!_
_# Esta é a segunda linha._
_# Esta é a terceira linha._

### Manipulação de Arquivos JSON

```python
import json

# Escrevendo JSON
dados = {
    "nome": "João",
    "idade": 30,
    "cidades_visitadas": ["São Paulo", "Rio de Janeiro", "Belo Horizonte"]
}

with open("dados.json", "w") as arquivo:
    json.dump(dados, arquivo, indent=4)

# Lendo JSON
with open("dados.json", "r") as arquivo:
    dados_lidos = json.load(arquivo)
    print(dados_lidos)
```

_# Resultado:_
_# {'nome': 'João', 'idade': 30, 'cidades_visitadas': ['São Paulo', 'Rio de Janeiro', 'Belo Horizonte']}_

### Manipulação de Arquivos CSV

```python
import csv

# Escrevendo CSV
dados = [
    ["Nome", "Idade", "Cidade"],
    ["João", 30, "São Paulo"],
    ["Maria", 25, "Rio de Janeiro"],
    ["Carlos", 35, "Belo Horizonte"]
]

with open("dados.csv", "w", newline="") as arquivo:
    escritor = csv.writer(arquivo)
    escritor.writerows(dados)

# Lendo CSV
with open("dados.csv", "r") as arquivo:
    leitor = csv.reader(arquivo)
    for linha in leitor:
        print(linha)
```

_# Resultado:_
_# ['Nome', 'Idade', 'Cidade']_
_# ['João', '30', 'São Paulo']_
_# ['Maria', '25', 'Rio de Janeiro']_
_# ['Carlos', '35', 'Belo Horizonte']_

## Orientação a Objetos

### Classes e Objetos

```python
# Definindo uma classe
class Pessoa:
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade

    def saudacao(self):
        return f"Olá, meu nome é {self.nome} e tenho {self.idade} anos."

# Criando objetos
pessoa1 = Pessoa("João", 30)
pessoa2 = Pessoa("Maria", 25)

print(pessoa1.nome)
print(pessoa1.saudacao())
print(pessoa2.saudacao())
```

_# Resultado:_
_# João_
_# Olá, meu nome é João e tenho 30 anos._
_# Olá, meu nome é Maria e tenho 25 anos._

### Herança

```python
# Classe base
class Animal:
    def __init__(self, nome):
        self.nome = nome

    def fazer_som(self):
        return "Som genérico de animal"

# Classe derivada
class Cachorro(Animal):
    def fazer_som(self):
        return "Au au!"

class Gato(Animal):
    def fazer_som(self):
        return "Miau!"

# Usando as classes
animal = Animal("Animal Genérico")
cachorro = Cachorro("Rex")
gato = Gato("Felix")

print(animal.nome, animal.fazer_som())
print(cachorro.nome, cachorro.fazer_som())
print(gato.nome, gato.fazer_som())
```

_# Resultado:_
_# Animal Genérico Som genérico de animal_
_# Rex Au au!_
_# Felix Miau!_

### Encapsulamento

```python
class ContaBancaria:
    def __init__(self, titular, saldo_inicial=0):
        self.titular = titular
        self.__saldo = saldo_inicial  # Atributo privado

    def depositar(self, valor):
        if valor > 0:
            self.__saldo += valor
            return True
        return False

    def sacar(self, valor):
        if 0 < valor <= self.__saldo:
            self.__saldo -= valor
            return True
        return False

    def consultar_saldo(self):
        return self.__saldo

# Usando a classe
conta = ContaBancaria("João", 1000)
print(conta.titular)
print(conta.consultar_saldo())
conta.depositar(500)
print(conta.consultar_saldo())
conta.sacar(200)
print(conta.consultar_saldo())

# Tentando acessar o atributo privado diretamente causará erro
# print(conta.__saldo)  # AttributeError
```

_# Resultado:_
_# João_
_# 1000_
_# 1500_
_# 1300_

### Polimorfismo

```python
class Forma:
    def area(self):
        pass

class Retangulo(Forma):
    def __init__(self, largura, altura):
        self.largura = largura
        self.altura = altura

    def area(self):
        return self.largura * self.altura

class Circulo(Forma):
    def __init__(self, raio):
        self.raio = raio

    def area(self):
        return 3.14159 * self.raio ** 2

# Função que usa polimorfismo
def calcular_area(forma):
    return forma.area()

# Usando o polimorfismo
retangulo = Retangulo(5, 4)
circulo = Circulo(3)

print(calcular_area(retangulo))
print(calcular_area(circulo))
```

_# Resultado:_
_# 20_
_# 28.27431_

### Métodos Especiais

```python
class Ponto:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"({self.x}, {self.y})"

    def __add__(self, outro):
        return Ponto(self.x + outro.x, self.y + outro.y)

    def __eq__(self, outro):
        return self.x == outro.x and self.y == outro.y

# Usando métodos especiais
p1 = Ponto(1, 2)
p2 = Ponto(3, 4)
p3 = Ponto(1, 2)

print(p1)  # __str__
print(p1 + p2)  # __add__
print(p1 == p3)  # __eq__
```

_# Resultado:_
_# (1, 2)_
_# (4, 6)_
_# True_

## Tratamento de Exceções

### Try-Except

```python
# Tratamento básico de exceções
try:
    resultado = 10 / 0
except ZeroDivisionError:
    print("Erro: Divisão por zero!")

# Tratando múltiplas exceções
try:
    numero = int("abc")
except ValueError:
    print("Erro: Valor inválido!")
except ZeroDivisionError:
    print("Erro: Divisão por zero!")

# Capturando a exceção
try:
    resultado = 10 / 0
except ZeroDivisionError as erro:
    print(f"Ocorreu um erro: {erro}")
```

_# Resultado:_
_# Erro: Divisão por zero!_
_# Erro: Valor inválido!_
_# Ocorreu um erro: division by zero_

### Try-Except-Else-Finally

```python
try:
    numero = int("123")
except ValueError:
    print("Valor inválido!")
else:
    print(f"Conversão bem-sucedida: {numero}")
finally:
    print("Execução finalizada.")

# Exemplo mais completo
try:
    arquivo = open("arquivo.txt", "r")
    conteudo = arquivo.read()
except FileNotFoundError:
    print("Arquivo não encontrado!")
else:
    print(f"Conteúdo: {conteudo}")
finally:
    try:
        arquivo.close()
        print("Arquivo fechado.")
    except:
        pass
```

_# Resultado (assumindo que arquivo.txt exista e contenha "Teste"):_
_# Conversão bem-sucedida: 123_
_# Execução finalizada._
_# Conteúdo: Teste_
_# Arquivo fechado._

### Criando Exceções Personalizadas

```python
# Definindo uma exceção personalizada
class SaldoInsuficienteError(Exception):
    def __init__(self, saldo_atual, valor_saque, mensagem="Saldo insuficiente"):
        self.saldo_atual = saldo_atual
        self.valor_saque = valor_saque
        self.mensagem = mensagem
        super().__init__(self.mensagem)

    def __str__(self):
        return f"{self.mensagem}: Saldo atual R${self.saldo_atual}, tentativa de saque R${self.valor_saque}"

# Usando a exceção personalizada
def sacar(saldo, valor):
    if valor > saldo:
        raise SaldoInsuficienteError(saldo, valor)
    return saldo - valor

# Tratando a exceção personalizada
try:
    novo_saldo = sacar(100, 150)
except SaldoInsuficienteError as erro:
    print(erro)
```

_# Resultado:_
_# Saldo insuficiente: Saldo atual R$100, tentativa de saque R$150_

## Bibliotecas Populares

### Manipulação de Dados com Pandas

```python
import pandas as pd

# Criando um DataFrame
dados = {
    'Nome': ['João', 'Maria', 'Carlos', 'Ana'],
    'Idade': [30, 25, 35, 28],
    'Cidade': ['São Paulo', 'Rio de Janeiro', 'Belo Horizonte', 'Brasília']
}

df = pd.DataFrame(dados)
print(df)

# Selecionando colunas
print(df['Nome'])

# Filtrando dados
print(df[df['Idade'] > 30])

# Estatísticas descritivas
print(df.describe())

# Agrupamento
print(df.groupby('Cidade').mean())
```

_# Resultado:_
_# Nome Idade Cidade_
_# 0 João 30 São Paulo_
_# 1 Maria 25 Rio de Janeiro_
_# 2 Carlos 35 Belo Horizonte_
_# 3 Ana 28 Brasília_

_# 0 João_
_# 1 Maria_
_# 2 Carlos_
_# 3 Ana_

_# Name: Nome, dtype: object_
_# Nome Idade Cidade_
_# 2 Carlos 35 Belo Horizonte_
_# Idade_
_# count 4.000000_
_# mean 29.500000_
_# std 4.203173_
_# min 25.000000_
_# 25% 27.250000_
_# 50% 29.000000_
_# 75% 31.250000_
_# max 35.000000_
_# Idade_
_# Cidade _
_# Belo Horizonte 35.000000_
_# Brasília 28.000000_
_# Rio de Janeiro 25.000000_
_# São Paulo 30.000000_

### Visualização de Dados com Matplotlib

```python
import matplotlib.pyplot as plt
import numpy as np

# Dados
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# Criando o gráfico
plt.figure(figsize=(10, 6))
plt.plot(x, y1, label='sen(x)')
plt.plot(x, y2, label='cos(x)')
plt.title('Funções Seno e Cosseno')
plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.grid(True)
plt.savefig('grafico.png')
plt.show()
```

_# Resultado: Um gráfico com as curvas de seno e cosseno é exibido_

### Computação Científica com NumPy

```python
import numpy as np

# Criando arrays
arr1 = np.array([1, 2, 3, 4, 5])
arr2 = np.array([6, 7, 8, 9, 10])

print(arr1)
print(arr2)

# Operações com arrays
print(arr1 + arr2)
print(arr1 * arr2)

# Funções estatísticas
print(np.mean(arr1))
print(np.std(arr1))
print(np.max(arr1))

# Reshape
matriz = np.array([1, 2, 3, 4, 5, 6]).reshape(2, 3)
print(matriz)

# Random
random_arr = np.random.rand(3, 3)
print(random_arr)
```

_# Resultado:_
_# [1 2 3 4 5]_
_# [ 6 7 8 9 10]_
_# [ 7 9 11 13 15]_
_# [ 6 14 24 36 50]_
_# 3.0_
_# 1.4142135623730951_
_# 5_
_# [[1 2 3]_
_# [4 5 6]]_
_# [[0.34521351 0.12451235 0.98452145]_
_# [0.76512354 0.23541213 0.56214523]_
_# [0.12354125 0.89754213 0.45612354]]_

### Aprendizado de Máquina com Scikit-Learn

```python
from sklearn import datasets
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Carregando o dataset iris
iris = datasets.load_iris()
X = iris.data
y = iris.target

# Dividindo em treino e teste
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Criando e treinando o modelo
modelo = RandomForestClassifier(n_estimators=100, random_state=42)
modelo.fit(X_train, y_train)

# Fazendo previsões
previsoes = modelo.predict(X_test)

# Avaliando o modelo
acuracia = accuracy_score(y_test, previsoes)
print(f"Acurácia: {acuracia:.2f}")

# Exemplo de previsão para uma nova amostra
nova_amostra = [[5.1, 3.5, 1.4, 0.2]]  # Setosa
predicao = modelo.predict(nova_amostra)
print(f"Classe prevista: {iris.target_names[predicao[0]]}")
```

_# Resultado:_
_# Acurácia: 0.96_
_# Classe prevista: setosa_

### Web Scraping com BeautifulSoup

```python
import requests
from bs4 import BeautifulSoup

# Fazendo a requisição
url = "https://example.com"
resposta = requests.get(url)

# Parseando o HTML
soup = BeautifulSoup(resposta.text, 'html.parser')

# Extraindo informações
titulo = soup.title.text
print(f"Título da página: {titulo}")

# Encontrando elementos
paragrafos = soup.find_all('p')
for i, p in enumerate(paragrafos):
    print(f"Parágrafo {i+1}: {p.text.strip()}")

# Encontrando elementos por classe
elementos_div = soup.find_all('div', class_='content')
for div in elementos_div:
    print(div.text.strip())
```

_# Resultado (assumindo que o site example.com está acessível):_
_# Título da página: Example Domain_
_# Parágrafo 1: This domain is for use in illustrative examples in documents. You may use this domain in literature without prior coordination or asking for permission._
_# Parágrafo 2: More information..._

### Desenvolvimento Web com Flask

```python
from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

# Rota para página inicial
@app.route('/')
def home():
    return render_template('index.html', titulo='Página Inicial')

# Rota com parâmetros na URL
@app.route('/usuario/<nome>')
def usuario(nome):
    return f"Olá, {nome}!"

# Rota com métodos HTTP
@app.route('/api/dados', methods=['GET', 'POST'])
def api_dados():
    if request.method == 'POST':
        dados = request.json
        # Processar dados...
        return jsonify({"status": "sucesso", "mensagem": "Dados recebidos"})
    else:
        # Retornar dados
        return jsonify({"dados": [1, 2, 3, 4, 5]})

# Iniciar o servidor
if __name__ == '__main__':
    app.run(debug=True)
```

_# Resultado: Um servidor web Flask é iniciado na porta 5000_

## Boas Práticas

### Docstrings e Documentação

```python
def calcular_media(numeros):
    """
    Calcula a média aritmética de uma lista de números.

    Args:
        numeros (list): Lista de números.

    Returns:
        float: A média aritmética dos números.

    Raises:
        ValueError: Se a lista estiver vazia.
        TypeError: Se algum elemento não for um número.

    Exemplos:
        >>> calcular_media([1, 2, 3, 4, 5])
        3.0
        >>> calcular_media([])
        Traceback (most recent call last):
            ...
        ValueError: Lista vazia
    """
    if not numeros:
        raise ValueError("Lista vazia")

    for num in numeros:
        if not isinstance(num, (int, float)):
            raise TypeError(f"Valor '{num}' não é um número")

    return sum(numeros) / len(numeros)

# Acessando a documentação
help(calcular_media)
```

_# Resultado: A documentação da função é exibida_

### Testes Unitários

```python
import unittest

def soma(a, b):
    return a + b

# Classe de teste
class TestSoma(unittest.TestCase):
    def test_soma_positivos(self):
        self.assertEqual(soma(1, 2), 3)

    def test_soma_negativos(self):
        self.assertEqual(soma(-1, -2), -3)

    def test_soma_misto(self):
        self.assertEqual(soma(-1, 2), 1)

# Executando os testes
if __name__ == '__main__':
    unittest.main()
```

_# Resultado:_
_# ..._
_# ----------------------------------------------------------------------_
_# Ran 3 tests in 0.001s_
_# OK_

### PEP 8 - Guia de Estilo

```python
# Exemplos de código seguindo as recomendações do PEP 8

# Indentação: 4 espaços
def funcao():
    if True:
        print("Indentado corretamente")

# Tamanho máximo da linha: 79 caracteres
muito_longo = (
    "Esta é uma string muito longa que seria quebrada "
    "para manter o código dentro dos limites recomendados."
)

# Imports
import os
import sys
from datetime import datetime

# Constantes
PI = 3.14159
GRAVITY = 9.8

# Espaçamento
def funcao_com_espacamento(param1, param2):
    # Espaço após o #
    print(param1)  # Comentário alinhado

    # Duas linhas em branco antes de classes ou funções de nível superior
    return param1 + param2
```

_# Resultado: Código mais limpo e de fácil leitura_

### Ambientes Virtuais

```bash
# Criando um ambiente virtual
python -m venv meu_ambiente

# Ativando o ambiente virtual
# No Windows:
meu_ambiente\Scripts\activate

# No Linux/Mac:
source meu_ambiente/bin/activate

# Instalando pacotes
pip install requests numpy pandas

# Listando pacotes instalados
pip list

# Criando um arquivo de requisitos
pip freeze > requirements.txt

# Instalando a partir de um arquivo de requisitos
pip install -r requirements.txt

# Desativando o ambiente virtual
deactivate
```

### Gerenciamento de Dependências com Poetry

```bash
# Instalando Poetry
pip install poetry

# Iniciando um novo projeto
poetry new meu_projeto

# Adicionando dependências
poetry add pandas matplotlib requests

# Adicionando dependências de desenvolvimento
poetry add --dev pytest black

# Instalando dependências
poetry install

# Executando scripts dentro do ambiente virtual
poetry run python meu_script.py

# Atualizando dependências
poetry update
```

### Logging

```python
import logging

# Configuração básica
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    filename='app.log'
)

# Usando o logger
logger = logging.getLogger(__name__)

def main():
    logger.debug("Mensagem de debug")
    logger.info("Programa iniciado")
    try:
        resultado = 10 / 0
    except Exception as e:
        logger.error(f"Erro: {e}", exc_info=True)
    logger.info("Programa finalizado")

if __name__ == "__main__":
    main()
```

_# Resultado: Logs são escritos no arquivo app.log_

### Profiling e Otimização

```python
import time
import cProfile

# Medindo tempo de execução
def funcao_lenta():
    time.sleep(1)
    return "Concluído"

# Usando decorador para medir tempo
def medir_tempo(func):
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        fim = time.time()
        print(f"Tempo de execução: {fim - inicio:.4f} segundos")
        return resultado
    return wrapper

@medir_tempo
def funcao_com_medicao():
    return funcao_lenta()

# Usando cProfile
def funcao_para_profile():
    soma = 0
    for i in range(1000000):
        soma += i
    return soma

cProfile.run('funcao_para_profile()')
```

_# Resultado:_
_# Tempo de execução: 1.0012 segundos_
_# Estatísticas de profile da função_

## Exemplos de Aplicações Práticas

### Programa de Gerenciamento de Tarefas

```python
import json
import os
from datetime import datetime

class GerenciadorTarefas:
    def __init__(self, arquivo='tarefas.json'):
        self.arquivo = arquivo
        self.tarefas = self._carregar_tarefas()

    def _carregar_tarefas(self):
        if os.path.exists(self.arquivo):
            with open(self.arquivo, 'r') as f:
                try:
                    return json.load(f)
                except json.JSONDecodeError:
                    return []
        return []

    def _salvar_tarefas(self):
        with open(self.arquivo, 'w') as f:
            json.dump(self.tarefas, f, indent=4)

    def adicionar_tarefa(self, titulo, descricao="", prioridade="média"):
        tarefa = {
            "id": len(self.tarefas) + 1,
            "titulo": titulo,
            "descricao": descricao,
            "prioridade": prioridade,
            "concluida": False,
            "data_criacao": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "data_conclusao": None
        }
        self.tarefas.append(tarefa)
        self._salvar_tarefas()
        return tarefa["id"]

    def concluir_tarefa(self, id_tarefa):
        for tarefa in self.tarefas:
            if tarefa["id"] == id_tarefa:
                tarefa["concluida"] = True
                tarefa["data_conclusao"] = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
                self._salvar_tarefas()
                return True
        return False

    def excluir_tarefa(self, id_tarefa):
        for i, tarefa in enumerate(self.tarefas):
            if tarefa["id"] == id_tarefa:
                del self.tarefas[i]
                self._salvar_tarefas()
                return True
        return False

    def listar_tarefas(self, apenas_pendentes=False):
        if apenas_pendentes:
            return [t for t in self.tarefas if not t["concluida"]]
        return self.tarefas

    def buscar_tarefa(self, termo):
        resultados = []
        for tarefa in self.tarefas:
            if (termo.lower() in tarefa["titulo"].lower() or
                termo.lower() in tarefa["descricao"].lower()):
                resultados.append(tarefa)
        return resultados

# Exemplo de uso
if __name__ == "__main__":
    gerenciador = GerenciadorTarefas()

    # Adicionar tarefas
    gerenciador.adicionar_tarefa("Estudar Python", "Revisar orientação a objetos", "alta")
    gerenciador.adicionar_tarefa("Fazer compras", "Comprar frutas e vegetais", "média")

    # Listar tarefas
    print("Tarefas:")
    for tarefa in gerenciador.listar_tarefas():
        status = "✓" if tarefa["concluida"] else "✗"
        print(f"{tarefa['id']}. [{status}] {tarefa['titulo']} ({tarefa['prioridade']})")

    # Concluir uma tarefa
    gerenciador.concluir_tarefa(1)

    # Listar apenas pendentes
    print("\nTarefas pendentes:")
    for tarefa in gerenciador.listar_tarefas(apenas_pendentes=True):
        print(f"{tarefa['id']}. {tarefa['titulo']}")

    # Buscar tarefas
    print("\nBusca por 'compras':")
    for tarefa in gerenciador.buscar_tarefa("compras"):
        print(f"{tarefa['id']}. {tarefa['titulo']}")
```

_# Resultado:_
_# Tarefas:_
_# 1. [✗] Estudar Python (alta)_
_# 2. [✗] Fazer compras (média)_
_# _
_# Tarefas pendentes:_
_# 2. Fazer compras_
_# _
_# Busca por 'compras':_
_# 2. Fazer compras_

### Web Scraper de Notícias

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time
from datetime import datetime

class ScraperNoticias:
    def __init__(self, url_base):
        self.url_base = url_base
        self.noticias = []

    def obter_noticias(self, max_paginas=1):
        for pagina in range(1, max_paginas + 1):
            url = f"{self.url_base}/page/{pagina}" if pagina > 1 else self.url_base
            self._scrapear_pagina(url)
            time.sleep(1)  # Pausa para não sobrecarregar o servidor

    def _scrapear_pagina(self, url):
        try:
            headers = {
                'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
            }
            resposta = requests.get(url, headers=headers)
            resposta.raise_for_status()

            soup = BeautifulSoup(resposta.text, 'html.parser')
            artigos = soup.find_all('article', class_='post')

            for artigo in artigos:
                titulo_elem = artigo.find('h2', class_='entry-title')
                data_elem = artigo.find('time', class_='entry-date')
                resumo_elem = artigo.find('div', class_='entry-content')

                if titulo_elem and data_elem:
                    titulo = titulo_elem.text.strip()
                    link = titulo_elem.find('a')['href'] if titulo_elem.find('a') else ""
                    data = data_elem.text.strip()
                    resumo = resumo_elem.text.strip() if resumo_elem else ""

                    self.noticias.append({
                        'titulo': titulo,
                        'data': data,
                        'resumo': resumo,
                        'link': link,
                        'data_coleta': datetime.now().strftime("%Y-%m-%d %H:%M:%S")
                    })
        except Exception as e:
            print(f"Erro ao acessar {url}: {e}")

    def salvar_csv(self, nome_arquivo="noticias.csv"):
        if self.noticias:
            df = pd.DataFrame(self.noticias)
            df.to_csv(nome_arquivo, index=False, encoding='utf-8')
            print(f"Dados salvos em {nome_arquivo}")
        else:
            print("Nenhuma notícia coletada para salvar.")

    def analisar_dados(self):
        if not self.noticias:
            return "Nenhuma notícia para analisar."

        df = pd.DataFrame(self.noticias)

        # Contagem por dia
        if 'data' in df.columns:
            try:
                df['data_formatada'] = pd.to_datetime(df['data'])
                contagem_por_dia = df['data_formatada'].dt.date.value_counts().sort_index()
                print("Contagem de notícias por dia:")
                print(contagem_por_dia)
            except:
                pass

        # Palavras mais comuns nos títulos
        palavras = " ".join(df['titulo']).lower().split()
        palavras = [p for p in palavras if len(p) > 3]  # Ignorar palavras pequenas
        contagem_palavras = pd.Series(palavras).value_counts().head(10)

        print("\nPalavras mais comuns nos títulos:")
        print(contagem_palavras)

        return {
            'total_noticias': len(df),
            'palavras_comuns': contagem_palavras.to_dict()
        }

# Exemplo de uso (note que o URL é fictício)
if __name__ == "__main__":
    scraper = ScraperNoticias("https://exemplo-noticias.com/tecnologia")
    scraper.obter_noticias(max_paginas=2)
    print(f"Total de notícias coletadas: {len(scraper.noticias)}")

    if scraper.noticias:
        # Exibir algumas notícias
        for i, noticia in enumerate(scraper.noticias[:3]):
            print(f"\nNotícia {i+1}:")
            print(f"Título: {noticia['titulo']}")
            print(f"Data: {noticia['data']}")
            print(f"Resumo: {noticia['resumo'][:100]}...")

        # Salvar e analisar
        scraper.salvar_csv()
        scraper.analisar_dados()
```

_# Resultado (assumindo que o site exemplo-noticias.com esteja acessível):_
_# Total de notícias coletadas: 20_
_# _
_# Notícia 1:_
_# Título: Nova versão do Python lançada com recursos inovadores_
_# Data: 01/04/2023_
_# Resumo: A linguagem de programação Python acaba de receber uma atualização importante com recursos..._
_# _
_# Dados salvos em noticias.csv_
_# Contagem de notícias por dia:_
_# 2023-04-01 10_
_# 2023-03-31 8_
_# 2023-03-30 2_
_# _
_# Palavras mais comuns nos títulos:_
_# python 5_
_# inteligência 3_
_# artificial 3_
_# tecnologia 2_

### API REST com Flask

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime
import os

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///produtos.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

# Modelo
class Produto(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    nome = db.Column(db.String(100), nullable=False)
    descricao = db.Column(db.String(200))
    preco = db.Column(db.Float, nullable=False)
    estoque = db.Column(db.Integer, default=0)
    data_cadastro = db.Column(db.DateTime, default=datetime.now)

    def to_dict(self):
        return {
            'id': self.id,
            'nome': self.nome,
            'descricao': self.descricao,
            'preco': self.preco,
            'estoque': self.estoque,
            'data_cadastro': self.data_cadastro.strftime('%Y-%m-%d %H:%M:%S')
        }

# Criar o banco de dados
with app.app_context():
    db.create_all()

# Rotas
@app.route('/api/produtos', methods=['GET'])
def listar_produtos():
    produtos = Produto.query.all()
    return jsonify([p.to_dict() for p in produtos])

@app.route('/api/produtos/<int:id>', methods=['GET'])
def obter_produto(id):
    produto = Produto.query.get_or_404(id)
    return jsonify(produto.to_dict())

@app.route('/api/produtos', methods=['POST'])
def criar_produto():
    dados = request.json

    if not dados or not 'nome' in dados or not 'preco' in dados:
        return jsonify({'erro': 'Dados incompletos'}), 400

    produto = Produto(
        nome=dados['nome'],
        descricao=dados.get('descricao', ''),
        preco=dados['preco'],
        estoque=dados.get('estoque', 0)
    )

    db.session.add(produto)
    db.session.commit()

    return jsonify(produto.to_dict()), 201

@app.route('/api/produtos/<int:id>', methods=['PUT'])
def atualizar_produto(id):
    produto = Produto.query.get_or_404(id)
    dados = request.json

    if 'nome' in dados:
        produto.nome = dados['nome']
    if 'descricao' in dados:
        produto.descricao = dados['descricao']
    if 'preco' in dados:
        produto.preco = dados['preco']
    if 'estoque' in dados:
        produto.estoque = dados['estoque']

    db.session.commit()

    return jsonify(produto.to_dict())

@app.route('/api/produtos/<int:id>', methods=['DELETE'])
def excluir_produto(id):
    produto = Produto.query.get_or_404(id)
    db.session.delete(produto)
    db.session.commit()

    return jsonify({'mensagem': f'Produto {id} excluído com sucesso'})

# Executar a aplicação
if __name__ == '__main__':
    app.run(debug=True)
```

_# Resultado: Uma API REST é iniciada com endpoints para gerenciar produtos_

### Análise de Dados e Visualização

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

# Carregar dados (criando um DataFrame de exemplo)
np.random.seed(42)
dados = {
    'idade': np.random.randint(18, 65, 100),
    'salario': np.random.normal(5000, 1500, 100),
    'experiencia': np.random.randint(0, 30, 100),
    'setor': np.random.choice(['Tecnologia', 'Finanças', 'Saúde', 'Educação'], 100),
    'satisfacao': np.random.randint(1, 11, 100)
}

df = pd.DataFrame(dados)

# Análise exploratória
print("Primeiras linhas:")
print(df.head())

print("\nInformações do DataFrame:")
print(df.info())

print("\nEstatísticas descritivas:")
print(df.describe())

# Análise por setor
print("\nMédia salarial por setor:")
media_salarial = df.groupby('setor')['salario'].mean().sort_values(ascending=False)
print(media_salarial)

print("\nContagem por setor:")
contagem_setor = df['setor'].value_counts()
print(contagem_setor)

# Correlação entre variáveis
print("\nMatriz de correlação:")
correlacao = df[['idade', 'salario', 'experiencia', 'satisfacao']].corr()
print(correlacao)

# Visualizações
plt.figure(figsize=(12, 10))

# Histograma de salários
plt.subplot(2, 2, 1)
sns.histplot(df['salario'], kde=True)
plt.title('Distribuição de Salários')

# Boxplot de salários por setor
plt.subplot(2, 2, 2)
sns.boxplot(x='setor', y='salario', data=df)
plt.title('Salário por Setor')
plt.xticks(rotation=45)

# Gráfico de dispersão: Experiência vs. Salário
plt.subplot(2, 2, 3)
sns.scatterplot(x='experiencia', y='salario', hue='setor', data=df)
plt.title('Experiência vs. Salário')

# Mapa de calor da matriz de correlação
plt.subplot(2, 2, 4)
sns.heatmap(correlacao, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
plt.title('Matriz de Correlação')

plt.tight_layout()
plt.savefig('analise_dados.png')
plt.show()

# Análise mais avançada
# Regressão linear simples
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score

X = df[['experiencia', 'idade']]
y = df['salario']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

modelo = LinearRegression()
modelo.fit(X_train, y_train)

y_pred = modelo.predict(X_test)

print("\nResultados da Regressão Linear:")
print(f"Coeficientes: {modelo.coef_}")
print(f"Intercepto: {modelo.intercept_}")
print(f"R² Score: {r2_score(y_test, y_pred):.4f}")
print(f"Erro Quadrático Médio: {mean_squared_error(y_test, y_pred):.2f}")

# Gráfico de previsões vs. valores reais
plt.figure(figsize=(10, 6))
plt.scatter(y_test, y_pred, alpha=0.5)
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--')
plt.xlabel('Salário Real')
plt.ylabel('Salário Previsto')
plt.title('Valores Reais vs. Previstos')
plt.savefig('regressao.png')
plt.show()

# Exportar resultados processados
df['salario_normalizado'] = (df['salario'] - df['salario'].mean()) / df['salario'].std()
df['faixa_salarial'] = pd.cut(df['salario'], bins=5, labels=['Muito Baixo', 'Baixo', 'Médio', 'Alto', 'Muito Alto'])

df.to_csv('dados_processados.csv', index=False)
print("\nDados processados exportados para 'dados_processados.csv'")
```

_# Resultado: Uma análise completa dos dados com visualizações é realizada e salva_

## Conclusão

Esta documentação oferece uma visão abrangente da linguagem Python, desde conceitos básicos até tópicos avançados e aplicações práticas. Python é uma linguagem versátil, poderosa e acessível, adequada para diversos domínios como desenvolvimento web, ciência de dados, automação, inteligência artificial e muito mais.
