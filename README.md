# 8-puzzle-state-search

# Buscas em Espaço de Estados utilizando busca não informada: BFS, DFS e IDS no 8-Puzzle

Este projeto foi desenvolvido como atividade da disciplina de Inteligência Artificial, com o objetivo de implementar e comparar diferentes algoritmos de busca não informada aplicados ao problema clássico do **8-Puzzle**.

A atividade utiliza como base as implementações de **Busca em Largura (BFS)** e **Busca em Profundidade (DFS)** desenvolvidas em aula e adiciona a implementação do **Aprofundamento Iterativo (IDS — Iterative Deepening Search)**.

Além de encontrar uma solução para cada estado inicial, os algoritmos também coletam métricas de execução, permitindo analisar seus diferentes comportamentos em termos de profundidade da solução, quantidade de estados explorados, uso da fronteira e tempo de execução.

---

## Objetivos

Os principais objetivos da atividade são:

* Implementar o algoritmo **IDS (Iterative Deepening Search)** do zero;
* Reutilizar a estrutura desenvolvida anteriormente para BFS e DFS;
* Resolver diferentes instâncias do **8-Puzzle**;
* Retornar o caminho completo até a solução;
* Coletar métricas de execução;
* Comparar experimentalmente **BFS × DFS × IDS**;
* Compreender as vantagens e limitações de cada estratégia de busca.

---

# O problema do 8-Puzzle

O **8-Puzzle** é um problema clássico utilizado no estudo de algoritmos de busca em Inteligência Artificial.

O tabuleiro possui uma grade `3 × 3`, contendo oito peças numeradas e uma posição vazia, normalmente representada pelo valor `0`.

Um possível estado é:

```text
1 2 3
4 5 6
7 0 8
```

O objetivo é movimentar a posição vazia até alcançar uma configuração final previamente definida.

Neste projeto, os estados são representados utilizando uma **tupla de nove elementos**:

```python
(1, 2, 3,
 4, 5, 6,
 7, 0, 8)
```

Essa representação facilita a comparação entre estados e permite armazená-los em estruturas como `set`, utilizadas para controlar estados já visitados.

---

## Estado objetivo

O estado objetivo utilizado é:

```text
1 2 3
4 5 6
7 8 0
```

Representado em Python por:

```python
GOAL = (1, 2, 3, 4, 5, 6, 7, 8, 0)
```

Cada movimento consiste em deslocar o espaço vazio (`0`) para uma posição válida:

* cima;
* baixo;
* esquerda;
* direita.

A função responsável por gerar esses possíveis estados sucessores é utilizada pelos três algoritmos de busca.

---

# Algoritmos implementados

## BFS — Breadth-First Search

A **Busca em Largura** explora os estados por níveis.

Primeiro são analisados todos os estados que estão a uma distância de um movimento do estado inicial. Depois, aqueles que estão a dois movimentos, três movimentos e assim sucessivamente.

Sua fronteira normalmente é implementada utilizando uma **fila FIFO**:

```text
First In, First Out
```

Exemplo simplificado:

```text
          Inicial
        /    |    \
       A     B     C
      / \   / \
     D   E F   G
```

A BFS visita primeiro:

```text
Inicial → A → B → C → D → E → F → G
```

### Características

* completa;
* encontra a solução de menor profundidade quando todos os movimentos possuem o mesmo custo;
* pode consumir muita memória;
* tende a armazenar uma grande quantidade de estados na fronteira.

Para o 8-Puzzle, a BFS é particularmente interessante porque cada movimento possui o mesmo custo.

---

## DFS — Depth-First Search

A **Busca em Profundidade** explora um caminho o máximo possível antes de retornar e tentar alternativas.

Sua fronteira normalmente é implementada utilizando uma **pilha LIFO**:

```text
Last In, First Out
```

Considerando novamente:

```text
          Inicial
        /    |    \
       A     B     C
      / \
     D   E
```

A DFS pode seguir uma sequência semelhante a:

```text
Inicial → A → D → ...
```

antes de voltar e explorar os outros ramos.

### Características

* utiliza menos memória que a BFS em muitos casos;
* pode encontrar uma solução rapidamente se escolher um caminho favorável;
* não garante encontrar a solução mais curta;
* pode explorar caminhos muito profundos antes de chegar ao objetivo.

No 8-Puzzle, isso significa que a DFS pode encontrar uma solução com muito mais movimentos do que o necessário.

---

## IDS — Iterative Deepening Search

O **Aprofundamento Iterativo**, ou **IDS**, combina características importantes da BFS e da DFS.

A ideia é executar sucessivamente buscas em profundidade com um limite máximo de profundidade.

Por exemplo:

```text
Limite 0
↓
Limite 1
↓
Limite 2
↓
Limite 3
↓
...
```

A cada nova iteração, a profundidade máxima permitida aumenta.

Uma possível sequência seria:

```text
Busca até profundidade 0

Inicial
```

Depois:

```text
Busca até profundidade 1

        Inicial
       /   |   \
      A    B    C
```

Depois:

```text
Busca até profundidade 2

        Inicial
       /   |   \
      A    B    C
     / \
    D   E
```

Esse processo continua até que o estado objetivo seja encontrado.

### Características

O IDS busca combinar:

* a eficiência de memória da DFS;
* a capacidade da BFS de encontrar soluções em profundidades menores.

Apesar de visitar novamente alguns estados nas diferentes iterações, esse custo adicional normalmente é compensado pela redução no consumo de memória.

---

# Comparação conceitual

| Algoritmo | Estrutura principal | Encontra menor profundidade? | Uso de memória | Estratégia                       |
| --------- | ------------------- | ---------------------------: | -------------: | -------------------------------- |
| BFS       | Fila                |                          Sim |           Alto | Explora nível por nível          |
| DFS       | Pilha               |                          Não |          Baixo | Explora profundamente um caminho |
| IDS       | Pilha + limite      |                          Sim | Baixo/moderado | Repete DFS aumentando o limite   |

A comparação experimental permite observar que algoritmos capazes de resolver o mesmo problema podem apresentar comportamentos completamente diferentes.

---

# Instâncias utilizadas

Para os experimentos foram utilizados três estados iniciais, todos solúveis e suficientemente pequenos para permitir uma comparação rápida entre os algoritmos:

```python
casos = {
    "Fácil":      (1, 2, 3, 4, 5, 6, 7, 0, 8),
    "Médio":      (1, 2, 3, 4, 5, 6, 0, 7, 8),
    "Desafiador": (1, 2, 3, 5, 0, 6, 4, 7, 8),
}
```

Visualmente:

### Fácil

```text
1 2 3
4 5 6
7 0 8
```

### Médio

```text
1 2 3
4 5 6
0 7 8
```

### Desafiador

```text
1 2 3
5 0 6
4 7 8
```

---

# Métricas de execução

Além do caminho encontrado, cada algoritmo registra métricas para permitir uma comparação experimental.

Entre as métricas analisadas estão:

### Profundidade da solução

Número de movimentos necessários para sair do estado inicial e chegar ao objetivo.

```text
depth = quantidade de movimentos
```

Se o caminho possuir cinco estados:

```text
Estado 0
Estado 1
Estado 2
Estado 3
Estado 4
```

então:

```text
depth = 4
```

---

### Nós expandidos

Quantidade de estados retirados da fronteira e analisados durante a busca.

Essa métrica ajuda a estimar o esforço computacional realizado por cada algoritmo.

---

### Tamanho máximo da fronteira

Maior quantidade de caminhos ou estados armazenados simultaneamente durante a execução.

Essa métrica permite observar diferenças de consumo de memória entre BFS, DFS e IDS.

---

### Tempo de execução

Tempo necessário para encontrar a solução de cada instância.

O tempo é medido durante a execução do algoritmo e utilizado como uma métrica complementar de comparação.

---

# Caminho da solução

Os algoritmos devem retornar uma **lista de estados**, permitindo visualizar passo a passo como o tabuleiro foi transformado.

Exemplo:

```python
[
    (1,2,3,4,5,6,7,0,8),
    (1,2,3,4,5,6,7,8,0)
]
```

Visualmente:

```text
1 2 3
4 5 6
7 0 8
```

↓

```text
1 2 3
4 5 6
7 8 0
```

---

# Experimento

Cada um dos três algoritmos é executado sobre os três casos:

```text
Fácil
Médio
Desafiador
```

Os resultados podem então ser organizados em uma tabela semelhante a:

| Caso       | Algoritmo | Profundidade | Nós expandidos | Máx. fronteira | Tempo |
| ---------- | --------- | -----------: | -------------: | -------------: | ----: |
| Fácil      | BFS       |            — |              — |              — |     — |
| Fácil      | DFS       |            — |              — |              — |     — |
| Fácil      | IDS       |            — |              — |              — |     — |
| Médio      | BFS       |            — |              — |              — |     — |
| Médio      | DFS       |            — |              — |              — |     — |
| Médio      | IDS       |            — |              — |              — |     — |
| Desafiador | BFS       |            — |              — |              — |     — |
| Desafiador | DFS       |            — |              — |              — |     — |
| Desafiador | IDS       |            — |              — |              — |     — |

Os valores são preenchidos a partir das métricas obtidas durante a execução.

---

# Análise esperada

A atividade busca evidenciar que não existe apenas uma maneira de explorar um espaço de estados.

A **BFS** tende a encontrar soluções de menor profundidade, mas pode manter uma fronteira relativamente grande.

A **DFS** pode utilizar menos memória, porém a ordem em que os estados são explorados pode fazer com que ela encontre caminhos muito maiores do que o ótimo.

O **IDS** tenta combinar características das duas abordagens: executa buscas em profundidade, mas aumenta gradualmente o limite máximo permitido até encontrar a solução.

Dessa forma, a comparação entre os três algoritmos permite visualizar conceitos fundamentais de Inteligência Artificial, como:

* espaço de estados;
* fator de ramificação;
* profundidade da solução;
* fronteira;
* estados visitados;
* completude;
* optimalidade;
* custo computacional;
* consumo de memória.

---

# Estrutura geral da atividade

```text
Estado inicial
      │
      ▼
Geração de movimentos
      │
      ▼
 ┌────┴────┐
 │         │
 ▼         ▼
BFS       DFS
           │
           ▼
          IDS
      │
      ▼
Estado objetivo
      │
      ▼
Coleta de métricas
      │
      ▼
Comparação dos algoritmos
```

---

# Tecnologias utilizadas

* Python
* Jupyter Notebook
* Estruturas de dados: listas, tuplas, conjuntos, filas e pilhas
* Algoritmos de busca em espaço de estados

---

por Roberto Neto, 23/08/2026. Documentação feita com auxílio de ferramentas de Inteligência Artificial.
