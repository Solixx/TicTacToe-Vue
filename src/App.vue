<script setup>
  import { ref, computed } from 'vue'
  
  // Define o estado do jogador atual (X ou O) usando o reativo "ref".
  const player = ref('X')
  // Define o estado do tabuleiro como uma matriz 3x3 vazia usando "ref".
  const board = ref([
    ['', '', ''],
    ['', '', ''],
    ['', '', '']
  ])

  // Função para calcular o vencedor com base nas combinações vencedoras possíveis.
  const CalculateWinner = (board) => {
    const lines = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]]

    for (let i = 0; i < lines.length; i++) {
      const [a, b, c] = lines[i]

      if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        return board[a]
      }
    }

    return null
  }

  // Calcula o vencedor com base no estado atual do tabuleiro usando "computed".
  const winner = computed(() => CalculateWinner(board.value.flat()))

  // Função para fazer uma jogada no tabuleiro.
  const makeMove = (x, y) => {
    if (winner.value) return

    if (board.value[x][y]) return

    board.value[x][y] = player.value

    player.value = player.value === 'X' ? 'O' : 'X'
  }

  // Função para redefinir o jogo para o estado inicial.
  const resetGame = () => {
    board.value = [
      ['', '', ''],
      ['', '', ''],
      ['', '', '']
    ]
    player.value = 'X'
  }

</script>

<template>
  <main>
    <h1>Tic Tac Toe</h1>

    <h3>Player {{ player }}'s turn</h3>

    <div class="game">
      <!-- Cria os espaços do jogo e lida com as jogadas -->
      <!-- Usa ternários para exibir 'X' ou 'O' nos espaços ocupados -->
      <div class="c1 gameSpace" @click="makeMove(0, 0)"> {{ board[0][0] === 'X' ? 'X': board[0][0] === 'O' ? 'O': '' }} </div>
      <div class="c2 gameSpace" @click="makeMove(0, 1)"> {{ board[0][1] === 'X' ? 'X': board[0][1] === 'O' ? 'O': '' }} </div>
      <div class="c3 gameSpace" @click="makeMove(0, 2)"> {{ board[0][2] === 'X' ? 'X': board[0][2] === 'O' ? 'O': '' }} </div>
      <div class="c4 gameSpace" @click="makeMove(1, 0)"> {{ board[1][0] === 'X' ? 'X': board[1][0] === 'O' ? 'O': '' }} </div>
      <div class="c5 gameSpace" @click="makeMove(1, 1)"> {{ board[1][1] === 'X' ? 'X': board[1][1] === 'O' ? 'O': '' }} </div>
      <div class="c6 gameSpace" @click="makeMove(1, 2)"> {{ board[1][2] === 'X' ? 'X': board[1][2] === 'O' ? 'O': '' }} </div>
      <div class="c7 gameSpace" @click="makeMove(2, 0)"> {{ board[2][0] === 'X' ? 'X': board[2][0] === 'O' ? 'O': '' }} </div>
      <div class="c8 gameSpace" @click="makeMove(2, 1)"> {{ board[2][1] === 'X' ? 'X': board[2][1] === 'O' ? 'O': '' }} </div>
      <div class="c9 gameSpace" @click="makeMove(2, 2)"> {{ board[2][2] === 'X' ? 'X': board[2][2] === 'O' ? 'O': '' }} </div>
    </div>

    <!-- Exibe o vencedor quando houver um -->
    <h3 class="winner" v-if="winner">Player {{ winner }} wins</h3>
    <!-- Botão para redefinir o jogo -->
    <button @click="resetGame">RESET</button>
  </main>
</template>

<style scoped>
  main{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100vw;
    height: 100vh;
  }

  h1{
    font-size: 3rem;
    margin-bottom: 3rem;
    color: whitesmoke;
  }

  h3{
    font-size: 1.5rem;
    margin-bottom: 1rem;
    color: whitesmoke;
  }

  .game {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 0px;
    width: 25rem;
    height: 25rem;
  }

  .gameSpace{
    width: 100%;
    border: 1px solid whitesmoke;;
    background-color: rgba(161, 181, 216, 0);
    transition: 0.5s ease;
    cursor: pointer;
    text-align: center;
    display: flex;
    align-items: center;
    widows: 100%;
    height: 100%;
    justify-content: center;
    font-size: 4rem;
    font-weight: bold;
    color: whitesmoke;
  }

  .gameSpace:hover{
    background-color: rgba(161, 181, 216, 0.1);
  }

  .c1 { grid-area: 1 / 1 / 2 / 2; }
  .c2 { grid-area: 1 / 2 / 2 / 3; }
  .c3 { grid-area: 1 / 3 / 2 / 4; }
  .c4 { grid-area: 2 / 1 / 3 / 2; }
  .c5 { grid-area: 2 / 2 / 3 / 3; }
  .c6 { grid-area: 2 / 3 / 3 / 4; }
  .c7 { grid-area: 3 / 1 / 4 / 2; }
  .c8 { grid-area: 3 / 2 / 4 / 3; }
  .c9 { grid-area: 3 / 3 / 4 / 4; }

  button{
    width: 7rem;
    height: 3rem;
    border: none;
    border-radius: 0.5rem;
    color: whitesmoke;
    background-color: rgba(185, 95, 137, 1);
    margin-top: 2.5rem;
    font-size: 1.5rem;
    text-align: center;
    font-weight: bold;
    cursor: pointer;
    transition: 0.5s ease;
  }

  button:hover{
    background-color: rgb(224, 73, 143);
  }

  .winner{
    color: rgba(245, 255, 198, 1);
    margin-top: 2rem;
  }
</style>
