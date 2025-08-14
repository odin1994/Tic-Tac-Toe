<template>
  <div class="tic-tac-toe">
    <h1>圈叉棋游戏</h1>
    <div class="status">{{ status }}</div>
    <div class="board">
      <div 
        v-for="(cell, index) in board" 
        :key="index" 
        class="cell" 
        @click="makeMove(index)"
      >
        {{ cell }}
      </div>
    </div>
    <button @click="resetGame" class="reset-btn">重新开始</button>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const board = ref(Array(9).fill(null));
    const currentPlayer = ref('X');
    const winner = ref(null);
    const gameOver = ref(false);

    const winningCombinations = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8], // 行
      [0, 3, 6], [1, 4, 7], [2, 5, 8], // 列
      [0, 4, 8], [2, 4, 6]             // 对角线
    ];

    const status = computed(() => {
      if (winner.value) return `获胜者: ${winner.value}`;
      if (gameOver.value) return '平局!';
      return `当前玩家: ${currentPlayer.value}`;
    });

    const checkWinner = () => {
      for (const combination of winningCombinations) {
        const [a, b, c] = combination;
        if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
          winner.value = board.value[a];
          gameOver.value = true;
          return;
        }
      }
      
      if (!board.value.includes(null)) {
        gameOver.value = true;
      }
    };

    const makeMove = (index) => {
      if (board.value[index] || gameOver.value) return;
      
      board.value[index] = currentPlayer.value;
      checkWinner();
      
      if (!gameOver.value) {
        currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
      }
    };

    const resetGame = () => {
      board.value = Array(9).fill(null);
      currentPlayer.value = 'X';
      winner.value = null;
      gameOver.value = false;
    };

    return {
      board,
      currentPlayer,
      winner,
      gameOver,
      status,
      makeMove,
      resetGame
    };
  }
};
</script>

<style scoped>
.tic-tac-toe {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
  margin-top: 50px;
}

.status {
  margin: 20px 0;
  font-size: 24px;
  font-weight: bold;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 5px;
}

.cell {
  width: 100px;
  height: 100px;
  background-color: #f0f0f0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 48px;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.cell:hover {
  background-color: #e0e0e0;
}

.reset-btn {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.reset-btn:hover {
  background-color: #45a049;
}
</style>