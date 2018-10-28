<template>
 <div class='game'>
    <div class='game-board'>
      <Board
       :squares='currentContext.squares'
       :winningCombination='getWinningCombination(currentContext.squares)'
       @square-click='handleSquareClick'/>
    </div>
    <div class='game-info'>
      <div class='status'>{{status}}</div>
      <button 
        class='sort'
        @click="sort" >
        Sort
      </button>
      <ol>
        <li v-for="(e, index) in sortedHistory"
          :key="index">
          
          <button
            @click="moveBack(index)"
            :class="{ 'current-item' : isActive(index) }">
            {{getMessage(index)}}
          </button>
          
        </li>
      </ol>

    </div>

  </div>
</template>

<script>
import Board from "./Board";

const player1 = "X";
const player2 = "O";

export default {
  name: "Game",
  components: {
    Board
  },
  data: function() {
    return {
      history: [
        {
          squares: Array(9).fill(null),
          position: null
        }
      ],
      player1IsNext: true,
      move: 0,
      isDesc: true
    };
  },
  computed: {
    status: function() {
      const winner = this.getWinningCombination(
        this.currentContext.squares
      );

      return winner === null
        ? `Next player: ${this.player}`
        : `Winner: ${this.player1IsNext ? player2 : player1}`;
    },
    currentContext: function() {
       return this.history[this.move];
    },
    player: function() {
      return this.player1IsNext ? player1 : player2;
    },
    sortedHistory: function() {
      return this.history.slice().reverse();
    }
  },
  methods: {
    handleSquareClick: function(i) {
      const squares = [...this.currentContext.squares];
      if (squares[i] || this.getWinningCombination(squares)) {
        return;
      }

      this.move++;

      const col = i % 3 + 1;
      const row = (i - col + 1) / 3 + 1;
      const position = { col, row };

      this.history.splice(this.move);
      squares[i] = this.player;

      this.history = this.history.concat({ squares, position });

      this.player1IsNext = !this.player1IsNext;
    },
    moveBack: function(i) {
      const realIndex = this.getIndex(i);
      this.player1IsNext = realIndex % 2 === 0;
      this.move = this.getIndex(i);
    },

    getWinningCombination: function(squares) {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ];
      for (let i = 0; i < lines.length; i++) {
        const currentLine = lines[i];
        const [a, b, c] = currentLine;
        if (
          squares[a] &&
          squares[a] === squares[b] &&
          squares[a] === squares[c]
        ) {
          return currentLine;
        }
      }
      return null;
    },
    getMessage: function(i) {
      const realIndex = this.getIndex(i);
      return realIndex === 0
        ? "Go to game start"
        : `Go to move #${realIndex} [
          col=${this.history[realIndex].position.col},
          row=${this.history[realIndex].position.row}
          ]`;
    },
    sort: function() {
      this.isDesc = !this.isDesc;
    },
    getIndex: function(index) {
      return this.isDesc ? index : this.history.length - 1 - index;
    },
    isActive: function(i) {
      return this.move === this.getIndex(i);
    }
  }
};
</script>

<style scoped >
ol,
ul {
  padding-left: 30px;
}

.sort {
  margin-left: 30px;
  margin-top: 10px;
}

.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}

.current-item {
  font-weight: bold;
}
</style>
