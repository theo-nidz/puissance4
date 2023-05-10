<template>
  <div>
    <div v-if="popup" class="modal">

    </div>
 <p>Tour du joueur <span v-if="isPlayer1Play">1</span><span v-else>2</span></p>

    <!--  GRID  -->
    <div class="grid-container" v-if="gameStart">
      <div v-for="(item, index) in grid" class="grid-item">
        <div
          @click="playerPlay(index)"
          :class="getClass(grid[index])"
          class="rond">
          {{index}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      grid:[],
      gridLength: 7,
      gridHeight:  6,
      gridMaxValue: 42,
      gridMinValue: 0,
      gameStart: false,

      isPlayer1Play: true,
      // Players
      player1:{
        name: 'Joueur 1',
        color: 'red',
        victory:0
      },
      player2:{
        name:  'Joueur 2',
        color: 'yellow',
        victory:0
      },
      // check victory
      column: 0,
      row:0,
      playerPlayPlace:0,
      checkBottom : true,
      numberToWin: 4,
      isOver:false,
    }
  },
  mounted(){
    this.openPopup()
  },
  methods: {
    gameLaunch(){
      this.generateGrid()
      return this.gameStart = true;
    },
    generateGrid() {
      this.grid = [];
      for (let i = 0; i < 42; i++) {
        this.grid.push(0)
      }
    },
    switchPlayerPlay() {
      this.isPlayer1Play = !this.isPlayer1Play
    },

    checkValidPlay(i, numberPlayerPlayer) {

      const col = i % this.gridLength;
      for (let row = this.gridHeight - 1; row >= 0; row--) {
        const index = row * this.gridLength + col;
        if (this.grid[index] === 0) {
          this.grid[index] = numberPlayerPlayer;
          this.switchPlayerPlay();
          this.col = col
          this.row = row
          this.playerPlayPlace = i
          return this.grid[index];
        }
      }
    },

    checkCol(col, player) {
      const oppositePlayer = player === 1 ? 2 : 1;
      let count = 0;
      for (let i = 1; i < this.gridHeight ; i++) {
        const index = i * this.gridLength + col;
        if(this.grid[index] === player) count++
      }

      if(count >= this.numberToWin) {
        console.log('victoire')
        this.endGame()
      }


    },
    endGame(){
      this.gameStart = false;
      this.isOver=true;
    },
    checkRow(row, player) {
      console.log('checkRow')
      const oppositePlayer = player === 1 ? 2 : 1;
      let count = 0;

      for (let i = 0; i < this.gridLength; i++) {
        const index = row * this.gridLength + i;
        if(this.grid[index] === player) count++
        if(this.grid[index] !== player) count = 0

        if(count >= this.numberToWin) {
          console.log('victoire')
        this.endGame()
        }
      }
    },

    checkDiagonalRight(playerPlayPlace, player) {
      console.log("checkDiagonalRight")
      let count = 1
      const oppositePlayer = player === 1 ? 2 : 1;
      console.log({playerPlayPlace})
      let canCheckTopRight = true
      let canCheckBottomRight = true

      for (let i = 1; i < 4; i++) {


        console.log('tour de boucle', i)

        if(canCheckTopRight) {
          const index = playerPlayPlace - ( 6*i )
          console.log('top')
          console.log( {grid: this.grid[index], player, count, index})
          if(this.grid[index] === player) {
            count++
          }else {
            canCheckTopRight = false
          }

        }

        if(canCheckBottomRight) {
          const index = playerPlayPlace + ( 6*i )
          console.log('bot')
          console.log( {grid: this.grid[index], player, count, index})
          if(this.grid[index] === player){
            count++
          }else{
            canCheckBottomRight = false
          }
        }

        if(count >= this.numberToWin) {
          console.log('victoire')
          this.endGame()
        }
      }
    },

    checkDiagonalLeft(playerPlayPlace, player) {
      console.log("checkDiagonalLeft")
      let count = 1
      const oppositePlayer = player === 1 ? 2 : 1;
      console.log({playerPlayPlace})
      let canCheckTopRight = true
      let canCheckBottomRight = true

      for (let i = 1; i < 4; i++) {


        console.log('tour de boucle', i)

        if(canCheckTopRight) {
          const index = playerPlayPlace - ( 8*i )
          console.log('top')
          console.log( {grid: this.grid[index], player, count, index})
          if(this.grid[index] === player) {
            count++
          }else {
            canCheckTopRight = false
          }

        }

        if(canCheckBottomRight) {
          const index = playerPlayPlace + ( 8*i )
          console.log('bot')
          console.log( {grid: this.grid[index], player, count, index})
          if(this.grid[index] === player){
            count++
          }else{
            canCheckBottomRight = false
          }
        }

        if(count >= this.numberToWin) {
          this.endGame()
        }
      }
    },


    playerPlay(i) {
    // définir qui joue
      const player = this.isPlayer1Play ? 1 : 2
      const positionPlay = this.checkValidPlay(i,player)

      this.checkCol(this.col, player)
      this.checkRow(this.row, player)
      this.checkDiagonalRight(this.playerPlayPlace, player)
      this.checkDiagonalLeft(this.playerPlayPlace, player)
      // re rendu de la grille
      this.$forceUpdate();
    },



    getClass(value) {
      switch (value) {
        case 0:
          return 'white'
        case 1:
          return 'yellow'
        case 2:
          return 'red'
      }
    },
  },
}
</script>
<style>
.modal {
  position: absolute;
  z-index: 5;
  background-color: rgba(0, 0, 0, 0.3);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
</style>
