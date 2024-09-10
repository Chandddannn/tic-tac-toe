<template>
    <div id="app">
      <h1>Tic-Tac-Toe</h1>
      <div id="game-board">
        <div class="row" v-for="(row, rowIndex) in board" :key="'row-' + rowIndex">
          <button
            v-for="(cell, colIndex) in row"
            :key="'cell-' + rowIndex + '-' + colIndex"
            class="cell"
            @click="makeMove(rowIndex, colIndex)"
          >
            {{ cell }}
          </button>
        </div>
      </div>
      <p id="status">{{ status }}</p>
      <button id="reset-button" @click="resetGame">Reset Game</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        board: [
          ['', '', ''],
          ['', '', ''],
          ['', '', '']
        ],
        currentPlayer: 'X',
        status: "Player X's turn"
      };
    },
    methods: {
      makeMove(row, col) {
        if (this.board[row][col] === '' && !this.checkWinner()) {
          this.$set(this.board[row], col, this.currentPlayer);
          if (this.checkWinner()) {
            this.status = `Player ${this.currentPlayer} wins!`;
          } else if (this.board.flat().every(cell => cell !== '')) {
            this.status = "It's a tie!";
          } else {
            this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
            this.status = `Player ${this.currentPlayer}'s turn`;
          }
        }
      },
      checkWinner() {
        const winPatterns = [
          // Horizontal
          [[0, 0], [0, 1], [0, 2]],
          [[1, 0], [1, 1], [1, 2]],
          [[2, 0], [2, 1], [2, 2]],
          // Vertical
          [[0, 0], [1, 0], [2, 0]],
          [[0, 1], [1, 1], [2, 1]],
          [[0, 2], [1, 2], [2, 2]],
          // Diagonal
          [[0, 0], [1, 1], [2, 2]],
          [[0, 2], [1, 1], [2, 0]]
        ];
        
        return winPatterns.some(pattern => {
          const [a, b, c] = pattern;
          return this.board[a[0]][a[1]] === this.currentPlayer &&
                 this.board[b[0]][b[1]] === this.currentPlayer &&
                 this.board[c[0]][c[1]] === this.currentPlayer;
        });
      },
      resetGame() {
        this.board = [
          ['', '', ''],
          ['', '', ''],
          ['', '', '']
        ];
        this.currentPlayer = 'X';
        this.status = "Player X's turn";
      }
    }
  };
  </script>
  
  <style scoped>
  #app {
    text-align: center;
    margin-top: 20px;
    font-family: Arial, sans-serif;
    color: #f9f9f9;
  }
  
  #game-board {
    display: inline-block;
    border: 2px solid #ffffff;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
  
  .row {
    display: flex;
  }
  
  .cell {
    width: 60px;
    height: 60px;
    font-size: 24px;
    line-height: 60px;
    text-align: center;
    border: 1px solid #fff9f9;
    cursor: pointer;
    color:#ffffff;
    background-color: #020202;
    transition: background-color 0.3s, transform 0.2s;
  }
  
  .cell:hover {
    background-color: #e0e0e0;
    transform: scale(1.1);
  }
  
  .cell.X {
    color: #ff5733;
    animation: pulse 0.5s ease;
  }
  
  .cell.O {
    color: #33c1ff;
    animation: pulse 0.5s ease;
  }
  
  @keyframes pulse {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.2);
    }
    100% {
      transform: scale(1);
    }
  }
  
  #status {
    margin-top: 20px;
    font-size: 18px;
    font-weight: bold;
    color: #f9f9f9;
  }
  
  #reset-button {
    margin-top: 20px;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    border: none;
    border-radius: 5px;
    background-color: #333;
    color: white;
    transition: background-color 0.3s, transform 0.2s;
  }
  
  #reset-button:hover {
    background-color: #555;
    transform: scale(1.05);
  }
  </style>
  