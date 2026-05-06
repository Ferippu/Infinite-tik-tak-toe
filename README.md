# Jogo Sem Velha

O "Jogo Sem Velha" é uma reinterpretação do clássico Jogo da Velha, construído com a mecânica de tempo de vida para as peças. Este projeto foi desenvolvido inteiramente no navegador e tem como foco principal o estudo prático de inteligência artificial aplicada a jogos de tabuleiro, especificamente através da implementação do algoritmo Minimax com poda Alpha-Beta.

## Sobre o Jogo

As regras são semelhantes às do jogo tradicional, mas com uma limitação de peças: cada jogador pode ter no máximo três marcações no tabuleiro simultaneamente. Ao posicionar a quarta peça, a marcação mais antiga do jogador é removida do tabuleiro (seguindo uma lógica de fila FIFO - First In, First Out). Isso garante que o jogo nunca resulte em um empate convencional ("dar velha"), continuando indefinidamente até que um jogador consiga alinhar três peças.

O jogo conta com um modo contra o computador, oferecendo quatro níveis de dificuldade:
* **Fácil:** A inteligência artificial realiza movimentos completamente aleatórios entre os espaços disponíveis.
* **Médio:** O algoritmo identifica movimentos para vencer ou bloquear o adversário no estado atual, mas ignora a mecânica de desaparecimento das peças antigas.
* **Difícil:** Considera o desaparecimento iminente das peças ao tentar vencer ou bloquear, e utiliza uma preferência tática ao escolher espaços vazios (priorizando o centro, seguido das quinas e, por fim, as bordas).
* **Vingança da Veia (Impossível):** Utiliza o algoritmo Minimax para prever todos os cenários possíveis e tomar a decisão matematicamente perfeita.

## Tecnologias Utilizadas

O projeto foi construído utilizando as tecnologias base da web, sem o auxílio de bibliotecas ou frameworks externos:

* **HTML5:** Estruturação da interface.
* **CSS3:** Utilizado para a estilização da interface limpa. O tabuleiro responsivo foi construído utilizando CSS Grid , e animações nativas controlam o feedback visual, como o aviso de qual peça irá desaparecer na próxima rodada reduzindo a opacidade da peça em questão.
* **JavaScript (ES6+):** Responsável por toda a lógica de estado do jogo, manipulação do DOM e implementação dos algoritmos de tomada de decisão da IA. A arquitetura foi orientada a objetos através da classe `InfiniteTicTacToe`.

## Entendendo a Inteligência Artificial

O núcleo de estudo deste projeto reside na dificuldade "Vingança da Veia", que implementa um algoritmo de busca em árvores de decisão.

### O Algoritmo Minimax

O Minimax é um algoritmo recursivo utilizado em teoria dos jogos para encontrar o melhor movimento possível em jogos de soma zero (onde a vantagem de um jogador é exatamente a desvantagem do outro). O algoritmo simula todas as jogadas possíveis, alternando entre dois jogadores hipotéticos: o "Maximizador" (que busca a maior pontuação possível) e o "Minimizador" (que busca a menor pontuação possível).

No contexto do "Jogo Sem Velha", a IA simula as consequências de colocar sua peça em cada espaço vazio, preve as respostas do jogador humano e, em seguida, as respostas da IA a essas respostas, criando uma árvore de possibilidades. A pontuação é definida com base na vitória: a IA busca o caminho que lhe garante a vitória mais rápida (avaliada através da profundidade da jogada) ou que, no mínimo, prolongue o jogo forçando o jogador a não vencer.


### Poda Alpha-Beta

A árvore de possibilidades gerada pelo Minimax cresce exponencialmente, o que pode causar lentidão e vazamento de memória. Para otimizar esse processo, utilizou-se a Poda Alpha-Beta. 

A poda Alpha-Beta não altera a decisão final do Minimax, ela apenas otimiza a busca. Durante a simulação, o algoritmo mantém o controle de dois valores: Alpha (o melhor valor já garantido para o Maximizador) e Beta (o melhor valor já garantido para o Minimizador). Se o algoritmo perceber que uma determinada ramificação levará a um resultado pior do que uma alternativa já avaliada em outro ramo, ele "poda" (interrompe a avaliação) dessa ramificação, pois sabe que aquele caminho nunca será escolhido em uma partida perfeita. 

```
┌───────────────────────────────────────────────────────────────────────────────┐
│  α: Melhor opção de MAX no caminho para a raiz                                │
│  β: Melhor opção de MIN no caminho para a raiz                                │
└───────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────┐ ┌──────────────────────────────────────┐
│ def valor_max(estado, α, β):         │ │ def valor_min(estado, α, β):         │
│   inicialize v = -∞                  │ │   inicialize v = +∞                  │
│   para cada sucessor:                │ │   para cada sucessor:                │
│     v = max(v, valor(sucessor, α, β))│ │     v = min(v, valor(sucessor, α, β))│
│     se v ≥ β retorne v               │ │     se v ≤ α retorne v               │
│     α = max(α, v)                    │ │     β = min(β, v)                    │
│   retorne v                          │ │   retorne v                          │
└──────────────────────────────────────┘ └──────────────────────────────────────┘ 
```


Essa otimização permite que a IA calcule as jogadas ideais quase instantaneamente, respeitando a mecânica de fila FIFO que adiciona e remove peças do estado simulado do tabuleiro.

## Como Testar?

O projeto é inteiramente client-side. Para executá-lo, basta realizar o clone deste repositório e abrir o arquivo `index.html` em qualquer navegador.
