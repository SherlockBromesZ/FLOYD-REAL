# Algoritmo de Bellman-Ford

Este repositório contém uma implementação do algoritmo de Bellman-Ford em C++. O algoritmo é utilizado para encontrar as menores distâncias de um vértice de origem para todos os outros vértices em um grafo ponderado, mesmo que contenha arestas com pesos negativos.

## Como funciona o algoritmo

O algoritmo de Bellman-Ford segue os seguintes passos:

1. **Inicialização**: 
   - Define a distância do vértice de origem como 0 e todas as outras distâncias como infinito (`inf`).
   
2. **Relaxamento**:
   - Para cada aresta `(u, v, p)`, se a distância até `u` somada ao peso `p` for menor que a distância atual até `v`, atualiza a distância de `v`.

3. **Repetição**:
   - Repete o processo de relaxamento para todas as arestas `n-1` vezes, onde `n` é o número de vértices.

4. **Detecção de Ciclos Negativos**:
   - Se após `n-1` iterações ainda for possível relaxar alguma aresta, então existe um ciclo negativo no grafo.

## Código

### Implementação

```cpp
#include <bits/stdc++.h>
using namespace std;

const int inf = INT_MAX / 2;

struct Aresta {
    int origem;
    int destino;
    int peso;
};

vector<int> ford(int n, vector<Aresta> &arestas, int origem) {
    vector<int> distancias(n, inf);
    distancias[origem] = 0;

    for (int i = 0; i < n; i++) {
        for (Aresta &aresta : arestas) {
            int u = aresta.origem;
            int v = aresta.destino;
            int p = aresta.peso;
            if (distancias[u] != inf && distancias[v] > distancias[u] + p) {
                distancias[v] = distancias[u] + p;
            }
        }
    }

    return distancias;
}

int main() {
    int n, m;
    cin >> n >> m;

    vector<Aresta> arestas(m);

    for (int i = 0; i < m; i++) {
        cin >> arestas[i].origem >> arestas[i].destino >> arestas[i].peso;
    }

    int origem;
    cin >> origem;
    vector<int> distancias = ford(n, arestas, origem);

    if (!distancias.empty()) {
        for (int i = 0; i < n; i++) {
            cout << distancias[i] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

### Entrada e Saída

#### Entrada

- A primeira linha contém dois inteiros `n` (número de vértices) e `m` (número de arestas).
- As próximas `m` linhas contêm três inteiros `origem`, `destino` e `peso` representando uma aresta do grafo.
- A última linha contém um inteiro `origem`, que é o vértice de origem para o cálculo das distâncias.

#### Saída

- Uma linha contendo `n` inteiros, onde o `i`-ésimo inteiro representa a menor distância do vértice de origem até o vértice `i`.

#### Exemplo

##### Entrada

```
5 8
0 1 -1
0 2 4
1 2 3
1 3 2
1 4 2
3 2 5
3 1 1
4 3 -3
0
```

##### Saída

```
0 -1 2 -2 1
```

### Explicação do Código

1. **Estrutura `Aresta`**:
   - Define uma estrutura para representar uma aresta com `origem`, `destino` e `peso`.

2. **Função `ford`**:
   - Recebe o número de vértices `n`, um vetor de arestas `arestas` e o vértice de origem `origem`.
   - Inicializa as distâncias com `inf` e define a distância do vértice de origem como 0.
   - Realiza `n-1` iterações para relaxar todas as arestas.
   - Retorna um vetor com as menores distâncias.

3. **Função `main`**:
   - Lê os valores de entrada.
   - Chama a função `ford` para calcular as menores distâncias.
   - Imprime as distâncias resultantes.

### Compilação e Execução

Para compilar e executar o código, siga os seguintes passos:

1. **Compilação**:
   - Utilize um compilador C++ como `g++`:
   ```bash
   g++ -std=c++17 -o bellman_ford bellman_ford.cpp
   ```

2. **Execução**:
   - Execute o programa:
   ```bash
   ./bellman_ford < input.txt
   ```

   Onde `input.txt` é um arquivo contendo os dados de entrada.

### Observações

- O algoritmo de Bellman-Ford tem complexidade de tempo `O(n * m)`, onde `n` é o número de vértices e `m` é o número de arestas.
- Ele pode detectar ciclos negativos, mas esta implementação não inclui explicitamente a detecção de ciclos negativos.
