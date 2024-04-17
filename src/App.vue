<template>
  <div id="app">
    <div class="status">{{ status }}</div>
    <button @click="reset">Reset</button>
    <template v-for="rowIndex in 3" :key="rowIndex">
      <div class="row" >
        <button
          v-for="colIndex in 3"
          :key="colIndex"
          class="square"
          style="width:40px;height:40px;"
          @click="handleClick(rowIndex - 1, colIndex - 1)"
          :disabled="(board[rowIndex - 1][colIndex - 1] !== '' || gameCompleted)"
        >
          {{ board[rowIndex - 1][colIndex - 1] }}
        </button>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      gameCompleted : false,
      status: "Next player: X",
      currentPlayer: "X",
      board: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
      ]
    };
  },
  methods: {
    handleClick(row, col) {
      if (this.board[row][col] === "") {
        // this.board[row].splice(col, 1, this.currentPlayer);
        this.board[row][col] = this.currentPlayer
        console.log( this.board[row].splice(col, 1, this.currentPlayer));
        if (this.checkWinner(row, col)) {
          this.status = `Player ${this.currentPlayer} wins!`;
          this.gameCompleted = true
        } else {
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
          this.status = `Next player: ${this.currentPlayer}`;
        }
      }
    },
    checkWinner(row, col) {
      const player = this.board[row][col];
      // Check row
      let checkRow = this.board[row].map(val => val !== player)
      if(!checkRow){
        return true
      }
      // if (this.board[row].every(value => {
      //   debugger
      //   value === player
      // })) {
      //   return true;
      // }
      // Check column
      if (this.board.every(row => row[col] === player)) {
        return true;
      }
      let checkColoumn = this.board.map(row =>{
        return row.map(val => val !== player)
      })
      if(!checkColoumn){
        return true
      }
      // Check diagonal
      if (
        (this.board[0][0] === player &&
          this.board[1][1] === player &&
          this.board[2][2] === player) ||
        (this.board[0][2] === player &&
          this.board[1][1] === player &&
          this.board[2][0] === player)
      ) {
        return true;
      }
      return false;
    },
    reset() {
      this.board = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
      ];
      this.currentPlayer = "X";
      this.status = "Next player: X";
    }
  }
};
</script>

<style>
.square {
  font-size: 24px;
  margin: 4px;
}
</style>
