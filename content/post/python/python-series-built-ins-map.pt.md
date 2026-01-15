+++
date = "2024-06-30"
title = "Experimentando Python built-ins: map()"
description = "Usando o map() é melhor quando processando iterables?"
slug = "python-map-transforming-values"
authors = "Diogo Pessoa"
tags = ["python", "functional-programming"]
categories = ["coding"]
series = ["python built-ins"]
toc = true
draft = true
+++

## Sumário

Em Python a função `map()`, é bem útil para objetos iterables. Aplicando uma função e retornando um
objeto Map. `(Geralmente transformado em uma lista)`

Vejo essa abordagem frequentemente ao ler código com uma abordagem de programação funcional.
Aproveitei para experimentar e escrever sobre e entender melhor os prós e os contras. Então, faça
uma rápida. Um resumo como referência quando/se usar o `mapa()`.

### Convenções & repositório do snippet do código

- [a implementação do código](https://github.com/diogo-pessoa/coding-exercises/blob/main/functional-programming/MapTransform.py)

## A função Map

A função Map aplica uma determinada função a todos os itens em uma lista de entrada. Ele retorna
um `Map Object`.

Achei a chamada de função extremamente útil: `.map(função, iterável).` Combine isso
com [expressão lambda](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions).
É possível escrever código sucinto e flexivel.

```python
    def test_mapping_built_in(self):
        numbers = [1, 2, 3, 4]
        squared = map(lambda x: x ** 2, numbers)
        print(type(squared))
        print(list(squared))
        print(squared.__doc__)
```

```python
    <class 'map'>
    [1, 4, 9, 16]
    map(func, *iterables) --> map object

    Make an iterator that computes the function using arguments from
    each of the iterables.  Stops when the shortest iterable is exhausted
```

## Exemplo prático

Cruzei por um caso no dia a dia, em que isso se mostrou útil (simplificou a legibilidade do código).

Em geral sou cético quanto a sacrificar legibilidade a favor de "smart code". Temendo que isso
dificulte a leitura e
entendimento da lógica. Mas neste caso simplificou o código. Ao extrair o valor transformado uma
chamada única usando o `map()` e retornando o resultado como uma lista.

```python
    def _reduce_key_to_pattern(self, key):
        return key.split("#")[1]

    def _build_tuple_from_item(self, item: Tuple[str, Any]) -> Tuple[str, Any]:
        return self._reduce_key_to_pattern(item[0]), item[1]

    def transform_keys(self, string_to_transform: dict) -> list[tuple]:
        """
        Given a dictionary, return a a list of tuples with the chars after "#".
        :param string_to_transform: dict
        :return: list[tuple]
        """
        return list(map(self._build_tuple_from_item, string_to_transform.items()))
``` 

Aqui está a ideia:

Neste cenário. Estamos salvando uma tupla com chave e valor. Onde a chave real é o que vem depois
o `#`.

```python

    def _reduce_key_to_pattern(self, key):
        return key.split("#")[1]

    def _build_tuple_from_item(self, item: Tuple[str, Any]) -> Tuple[str, Any]:
        return self._reduce_key_to_pattern(item[0]), item[1]

    def transform_keys(self, string_to_transform: dict) -> list[tuple]:
        """
        Given a dictionary, return a a list of tuples with the chars after "#".
        :param string_to_transform: dict
        :return: list[tuple]
        """
        return list(map(self._build_tuple_from_item, string_to_transform.items()))
```

* ao chamar o `map()` o `string_to_transform.items()` -> `<class 'dict_items'>`.

A `map()` chama a função (parametro: `self._build_tuple_from_item`) implicitamente. Passando cada
item
no iterable, aplicando a transformação e retornando a lista. Muita coisa acontece em apenas uma
linha. É fácil perder algum detalhe.

Algumas outras maneiras de escrever a mesma iteração em python. Agora, neste ponto, tudo soa muito
como syntax sugar. No entanto, ao usar o decorator registrando o tempo de execução, não vi um grande
diferença em tempo. `(na verdadeo map()  foi o mais lento)`

O **list comprehension** é provavelmente a maneira mais "pitônica" de abordar o iterable. No
entanto, já que estou a fã de uma boa sessão de depuração. O looping mais tradicional foi o mais
benéfico para mim.
Não perde muito em comparação, em relação a tempo de execução. O debugger é bem mais legível, sendo
possivel, explorar cada variavel e transformação aplicada. Para não mencionar, a estrutura é mais
familiar
para um programador vindo de uma linguagem diferente, é muito mais óbvio o que está acontecendo no
loop.
`(Poderíamos argumentar sobre a completitude do espaço, já que estou criando uma variável extra)`
**Warning:** O goal aqui não é o código mais eficiente `(não é uma análise Big O)`. A idéia é
explorar a função `map()`.

## Explorando outras formas de escrever a mesma iteração

### O "loop" tradicional

```python
...
result = []
for item in string_to_transform.items():
    result.append((self._reduce_key_to_pattern(item[0]), item[1]))
return result
```

### Usando "list comprehension":

```python
...
return [(self._reduce_key_to_pattern(item[0]), item[1]) for item in string_to_transform.items()]
...
```

## Testes com o @timeit_decorator e screenshots do  debugger

### Loop and Append

![for_loop_debugger.png](/images/map_python/for_loop_debugger.png)

* Test output

```python
Running Test: test_transform_keys_with_loop
transform_keys_with_loop_and_append took 0.000003 seconds to execute
```

### List comprehension

![List_comprehension_debugger.png](/images/map_python/List_comprehension_debugger.png)

* Test output

```python
Running Test: test_transform_keys_list_comprehension
transform_keys_with_comprehension took 0.000003 seconds to execute
```

### map()

![map_debugger.png](/images/map_python/map_debugger.png)

```python
Running Test: test_transform_keys_map
transform_keys took 0.000067 seconds to execute
```

## Conclusão

Wait for it… Depende. :unamused:

Eu definitivamente usaria a função `map()` em alguns casos. Em baixa complexidade. Onde a
transformação é simples, por
por exemplo. Transformando valores `map(lambda x: x ** 2, numbers)` para transformar o iterable.
Meu problema é. Não é imediatamente óbvio quando
escrevendo o método como tudo se comportará `(ou talvez, eu seja apenas inexperiente)`. Como eu
ainda uso muito o
Debugger `(e repetindo a execução dos testes)` para entender a lógica.

Outro ponto a se pensar é "esconder" a lógica. Eu me imagino lendo o código no futuro, e tentando
ler o
código sem documentação e falta de visibilidade do debugger. :cursing_face:

Resumindo, o `map()` é uma boa ferramenta para ter familiaridade. Imagino que acoplá-lo
com `filter()` e `reduce()`. Isso pode flexibilzar o processo de desenvolvimento. **Porém**, não é
para
iniciantes, achei dificil implementar e consigo imaginar várias formas de fazer errado.

## Referências

1. Map: Built-in Functions (no date) Python documentation. Available
   at: https://docs.python.org/3/library/functions.html#map (Accessed: 1 June 2024).
2. Lambda-expressions. Available
   at: https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions (Accessed:
   19 June 2024).
