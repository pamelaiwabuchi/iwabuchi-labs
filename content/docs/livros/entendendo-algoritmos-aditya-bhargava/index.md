---
title: "Entendendo Algoritmos - Aditya Y. Bhargava"
description: "Resumos e anotações sobre o livro entendendo algoritmos de Aditya Bhargava"
date: 2026-08-11T17:00:00-03:00
draft: false
categories: ["Livros"]
tags: ["Algoritmos", "Otimização", "Estruturas de dados"]
weight: 2
contributors: ["Pamela Iwabuchi"]
---

### Pilha de Chamada - Call stack

É uma estrutura de dados do tipo LIFO (Last In, First Out - o último a entrar é o primeiro a sair) - usada pelo interpretador para gerenciar a execução de funções em um programa. 

``` python
def funcao_c():
    print("Executando C")

def funcao_b():
    funcao_c()
    print("Executando B")

def funcao_a():
    funcao_b()
    print("Executando A")

funcao_a()

```

``` saída
Executando C
Executando B
Executando A
```

Cada elemento inserido na call stack é chamado de `Stack Frame` (ou quadro de pilha). Cada frame aloca memória para guardar:
- as variáveis locais da função;
- os parâmetros passados para a função
- o endereço de retorno (a instrução de onde a CPU deve ir apos terminar a função)

A call stack possui um limite fixo de memoria alocado pelo ambiente de execução. Se as funções forem empilhadas indefinidamente sem retornar, a memoria da pilha é esgotada, gerando o erro de `Stack Overflow`.

``` python
def recursao_infinita():
	recursao_infinita() # Nunca tem um retorno, empilha infinitamente

recursao_infinita()
#Erro em python: RecursionError: maximum recursion depth exceeded
```

O custo de usar uma pilha é o espaço que é utilizado na memória. Cada função ocupa um espaço e quando a pilha esta muito cheia temos duas opções:

- Reescrever o codigo usando loops.
- Utilizar tail recursion (recursao de cauda) - topico mais avancado que nao é suportada em todas as linguagens de programacao.

Resumo:
- Recursão é quando uma função chama a si mesma;
- Toda função recursiva tem dois casos: o caso-base e o caso recursivo;
- Uma pilha tem duas operações: push e pop;
- Todas as chamadas de função vão para a pilha de chamada
- A pilha de chamada pode ficar muito grande e ocupar muita memória.

## Quicksort - Capítulo 4


O algoritmo de quicksort é um algoritmo de ordenação que utiliza uma técnica chamada "Dividir para conquistar" - isso significa que dividimos o problema em partes menores usando recursão. Ele é muito utilizado e é mais rápido que a ordenação por seleção.

Para resolver um problema usando DC (dividir para conquistar), devemos seguir dois passos:
1. Descobrir o caso-base, que deve ser o caso mais simples possível;
2. Dividir ou diminuir o problema até que ele se torne o caso-base.

- O array mais simples que um algoritmo de ordenação pode ordenar é um array vazio ou um array com apenas 1 elemento, pois não há necessidade de ordená-los. Sendo assim, eles são o caso-base.

### Merge sort versus quicksort



## Hash

Pode ser feita uma tabela hash ao combinar uma função hash com um array.
Colisões são problemas, é necessário haver uma função hash que minimize colisões. 
Sao extremamente rápidas para pesquisar, inserir e remover itens.
Se seu fator de carga for maior que 0,7 será necessário redimensionar a hash.
são utilizadas como cache de dados, como no caso do Facebook.
## Desempenho

#### Tempo constante
Tempo de execução O(1). Refere-se a um tempo que continuará sempre o mesmo, independentemente de quão grande a tabela hash possa ficar.
Ou seja, não importa se a tabela hash tem 1 ou 1 milhão de elementos, o tempo de execução será o mesmo. 

## Pesquisa em largura - Capítulo 6

A pesquisa em largura é um tipo de algoritmo que utiliza grafos, permitindo encontrar o menor caminho entre dois objetos. 
Pode ser usada para:
- escrever um algoritmo de Inteligencia Artificial que calcula o menor numero de movimentos necessários para a vitória em uma partida de damas.
Os grafos são uma maneira de modelar como eventos diferentes estão conectados entre si.
Ajuda a responder 2 tipos de perguntas:
1. Existe algum caminho do vértice A até o vértice B?
2. Qual o caminho mínimo do vértice A até o vértice B?

Se houver um problema do tipo "encontre o menor X", tente modelar o seu problema utilizando grafos e use a pesquisa em largura para resolvê-lo.

-- Cada vez que você verificar alguém, procure não verificá-lo novamente, isso pode acabar em loop infinito.
## Filas

Estrutura de dados FIFO (First In First Out)
push = enqueue 
pop = dequeue

## Algoritmo de Dijkstra - Capítulo 7

Diferente da pesquisa em largura, que retorna o melhor caminho, com menos segmentos, o Algoritmo de Djikstra atribui um peso a cada segmento. Logo, o algoritmo encontra o caminho com o menor peso total. 

Nela, cada aresta tem um peso e um grafo com pesos é chamado de grafo ponderado (ou grafo valorado). Um grafo sem pesos é chamado de grafo não ponderado (ou grafo não valorado).

Você não pode usar o algoritmo de Dijkstra se tiver arestas com pesos negativos. 

## Algoritmos gulosos - Capítulo 8 

Trata-se de um algoritmo simples! A cada etapa, escolhe-se a solução ideal, e no fim você tem uma solução global ideal. 
- Algoritmos gulosos otimizam localmente na esperança de acabar em uma otimização global
- problemas NP-completo não tem uma solução rápida
- se você estiver tentando resolver um problema NP-completo, o melhor a fazer é usar um algoritmo de aproximação.
- algoritmos gulosos são fáceis de escrever e tem tempo de execução baixo, portanto eles são bons algoritmos de aproximação. 

## Programação Dinâmica - Capítulo 9

Técnica de resolução de problemas complexos que se baseia na divisão de um problema em subproblemas, os quais são resolvidos separadamente.

- útil quando voce tenta otimizar algo em relação a um limite.
- Voce pode usar a programacao dinamica quando o problema puder ser dividido em subproblemas discretos.
- Todas as solucoes em programacao dinamica envolvem uma tabela.

## K-vizinhos mais próximos - Capítulo 10

ë utilizado na classificacao e tambem na regressao. Ele envolve observar os K-vizinhos mais próximos.
Classificacao = classificar em grupos
Regressao = adivinhar uma resposta (como um número)
Extrair caracteristicas significa converter um item como uma fruta o uusuario, em uma lista de numeros que podem ser comparados. 
escolher boas caracteristicas é uma parte importante para que um algoritmo dos k-vizinhos mais proximos opere corretamente.

## algoritmo DIFIIE-Hellman
Programacao Linear

