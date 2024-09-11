<template>
  <div id="app">
    <h1>Tic-Tac-Toe Tournament</h1>

    <div v-if="!gameStarted" id="setup-form">
      <input v-model="playerX" placeholder="Enter Player X Name" class="input-field" />
      <input v-model="playerO" placeholder="Enter Player O Name" class="input-field" />
      <input v-model="totalRounds" type="number" placeholder="Number of Rounds" class="input-field" />
      <button @click="startTournament" class="start-button">Start Tournament</button>
    </div>

    <div v-else>
      <div id="scoreboard">
        <p><strong>{{ playerX }}</strong> (X): {{ scores.X }}</p>
        <p><strong>{{ playerO }}</strong> (O): {{ scores.O }}</p>
        <p>Round: {{ currentRound }} / {{ totalRounds }}</p>
      </div>

      <div v-if="!tournamentOver">
        <div id="game-board">
          <div class="row" v-for="(row, rowIndex) in board" :key="'row-' + rowIndex">
            <button
              v-for="(cell, colIndex) in row"
              :key="'cell-' + rowIndex + '-' + colIndex"
              class="cell"
              :class="cell"
              @click="makeMove(rowIndex, colIndex)"
            >
              {{ cell }}
            </button>
          </div>
        </div>
        <p id="status">{{ status }}</p>
        <button 
  class="start-button" 
  @click="resetGame" 
  :disabled="currentRound  >= totalRounds">
  Next Round
</button>      </div>

      <div v-if="tournamentOver">
        <p class="winner-message">{{ tournamentWinner }} wins the tournament!</p>
        <button class="start-button"  @click="restartTournament">Restart Tournament</button>
      </div>
    </div>
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
      playerX: '',
      playerO: '',
      totalRounds: this.totalRounds,
      currentRound: this.currentRound,
      status: "Player X's turn",
      scores: {
        X: 0,
        O: 0
      },
      gameStarted: false,
      tournamentOver: false,
      tournamentWinner: null
    };
  },
  methods: {
    startTournament() {
      if (this.playerX && this.playerO && this.totalRounds > 0) {
        this.board = [
      ['', '', ''],
      ['', '', ''],
      ['', '', '']
    ];
        this.gameStarted = true;
        this.status = `Player ${this.currentPlayer === 'X' ? this.playerX : this.playerO}'s turn`;
      } else {
        alert('Please enter valid names and number of rounds.');
      }
    },
    makeMove(row, col) {
      if (this.board[row][col] === '' && !this.checkWinner()) {
        this.$set(this.board[row], col, this.currentPlayer);
        if (this.checkWinner()) {
          this.status = `Player ${this.currentPlayer === 'X' ? this.playerX : this.playerO} wins this round!`;
          this.scores[this.currentPlayer]++;
          if (this.currentRound === this.totalRounds) {
            this.endTournament();
          }
        } else if (this.board.flat().every(cell => cell !== '')) {
          this.status = "It's a tie this round!";
          if (this.currentRound === this.totalRounds) {
            this.endTournament();
          }
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
          this.status = `Player ${this.currentPlayer === 'X' ? this.playerX : this.playerO}'s turn`;
        }
      }
    },
    checkWinner() {
      const winPatterns = [
        [[0, 0], [0, 1], [0, 2]],
        [[1, 0], [1, 1], [1, 2]],
        [[2, 0], [2, 1], [2, 2]],
        [[0, 0], [1, 0], [2, 0]],
        [[0, 1], [1, 1], [2, 1]],
        [[0, 2], [1, 2], [2, 2]],
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
  if (this.currentRound <= this.totalRounds) {
    this.board = [
      ['', '', ''],
      ['', '', ''],
      ['', '', '']
    ];
    this.currentRound++;
    this.currentPlayer = 'X';
    this.status = `Player ${this.currentPlayer === 'X' ? this.playerX : this.playerO}'s turn`;
  } else {
    this.endTournament();
  }
},

    endTournament() {
      this.tournamentOver = true;
      if (this.scores.X > this.scores.O) {
        this.tournamentWinner = this.playerX;
      } else if (this.scores.O > this.scores.X) {
        this.tournamentWinner = this.playerO;
      } else {
        this.tournamentWinner = "No one";
      }
    },
    restartTournament() {
      
      this.gameStarted = false;
      this.tournamentOver = false;
      this.scores = {
        X: 0,
        O: 0
      };
      this.currentRound = 1;
      this.playerX = '';
      this.playerO = '';
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

p {
  border: 10px;
}

#setup-form {
  display: inline-block;
  padding: 20px;
  background-color: #202020;
  border: 2px solid #ffffff;
  border-radius: 10px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  margin-top: 30px;
}

.input-field {
  display: block;
  width: 300px;
  padding: 10px;
  margin: 10px auto;
  font-size: 16px;
  border: 2px solid #ffffff;
  border-radius: 5px;
  background-color: #303030;
  color: #ffffff;
  outline: none;
  transition: border 0.3s ease;
}

.input-field:focus {
  border-color: #ff7b00;
}

.start-button {
  width: 320px;
  padding: 10px;
  font-size: 18px;
  font-weight: bold;
  color: white;
  background-color: #ff7b00;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.start-button:hover {
  background-color: #e66a00;
  transform: scale(1.05);
}

.start-button:active {
  transform: scale(1);
}

#game-board {
  display: inline-block;
  border: 10px solid #ffffff;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.row {
  display: flex;
}

.cell {
  width: 100px;
  height: 100px;
  font-size: 24px;
  line-height: 60px;
  text-align: center;
  border: 1px solid #fff9f9;
  cursor: pointer;
  color: #ffffff;
  background-color: #020202;
  transition: background-color 0.3s, transform 0.2s;
}

.cell:hover {
  background-color: #000000;
  transform: scale(1.1);
}

.cell.X {
  color: #ff0000;
  animation: pulse 0.5s ease;
}

.cell.O {
  color: #0000ff;
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

#winner-message {
  font-size: 24px;
  color: gold;
}

#reset-button, #start-button {
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

#reset-button:hover, #start-button:hover {
  background-color: #555;
  transform: scale(1.05);
}

#winner-message {
  font-size: 24px;
  color: gold;
}

#scoreboard {
  margin-bottom: 20px;
  font-size: 18px;
}
</style>
