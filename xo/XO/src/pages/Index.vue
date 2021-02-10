<template>
  <div>
  <table class="center">
    <tr>
      <td id="0" @click="DrawX($event)"></td>
      <td id="1" @click="DrawX($event)"></td>
      <td id="2" @click="DrawX($event)"></td>
    </tr>
    <tr>
      <td id="3" @click="DrawX($event)"></td>
      <td id="4" @click="DrawX($event)"></td>
      <td id="5" @click="DrawX($event)"></td>
    </tr>
    <tr>
      <td id="6" @click="DrawX($event)"></td>
      <td id="7" @click="DrawX($event)"></td>
      <td id="8" @click="DrawX($event)"></td>
    </tr>
  </table>
  <div v-if="winner" class="text_center">
    Winner is {{winner}}
  </div>
  </div>
</template>

<script>

export default {
  name: 'PageIndex',
  data () {
    return {
      xIds: [],
      oIds: [0, 1, 2, 3, 4, 5, 6, 7, 8],
      game: false,
      winner: null
    }
  },
  methods: {
    rand: function (items) {
      // "~~" for a closest "int"
      return items[~~(items.length * Math.random())]
    },
    DrawX: function (event) {
      var id = event.currentTarget.id
      var grid = document.getElementById(id)
      if (!this.game) {
        if (!grid.textContent) {
          grid.textContent = 'X'
          this.xIds.push(Number(id))
        }
      }
    },
    DrawO: function () {
      if (!this.game) {
        this.xIds.forEach((item) => {
          if (this.oIds.includes(item)) {
            var indexItem = this.oIds.indexOf(item)
            if (indexItem !== -1) {
              this.oIds.splice(indexItem, 1)
            }
          }
        })
        var randomElement = this.rand(this.oIds)
        if (randomElement !== undefined) {
          document.getElementById(randomElement.toString()).textContent = 'O'
          var index = this.oIds.indexOf(randomElement)
          if (index !== -1) {
            this.oIds.splice(index, 1)
          }
          console.log(this.oIds)
        } else {
          this.winner = 'undefined'
        }
      }
    },
    checkState: function () {
      var td = document.querySelectorAll('td')
      const allEqual = arr => arr.every(v => v === arr[0])
      var rows = [[td[0].textContent, td[1].textContent, td[2].textContent], [td[3].textContent, td[4].textContent, td[5].textContent], [td[6].textContent, td[7].textContent, td[8].textContent]]
      var columns = [[td[0].textContent, td[3].textContent, td[6].textContent], [td[1].textContent, td[4].textContent, td[7].textContent], [td[2].textContent, td[5].textContent, td[8].textContent]]
      var diagonals = [[td[0].textContent, td[4].textContent, td[8].textContent], [td[2].textContent, td[4].textContent, td[6].textContent]]

      var i
      for (i = 0; i < 3; i++) {
        if ((allEqual(rows[i])) && (rows[i][0] !== '')) {
          console.log('game ended row: ', i)
          console.log(i)
          this.game = true
          //  decoration
          var j
          if (i === 0) {
            //  determining a winner
            this.winner = document.getElementById('0').textContent
            for (j = 0; j < 3; j++) {
              document.getElementById(j.toString()).style.color = 'green'
            }
          }
          if (i === 1) {
            //  determining a winner
            this.winner = document.getElementById('3').textContent
            for (j = 3; j < 6; j++) {
              document.getElementById(j.toString()).style.color = 'green'
            }
          }
          if (i === 2) {
            //  determining a winner
            this.winner = document.getElementById('6').textContent
            for (j = 6; j < 9; j++) {
              document.getElementById(j.toString()).style.color = 'green'
            }
          }
        }
      }
      for (i = 0; i < 3; i++) {
        if ((allEqual(columns[i])) && (columns[i][0] !== '')) {
          console.log('game ended column: ', i)
          this.game = true
          //  decoration
          if (i === 0) {
            //  determining a winner
            this.winner = document.getElementById('0').textContent
            for (j = 0; j < 7;) {
              document.getElementById(j.toString()).style.color = 'green'
              console.log(j)
              j = j + 3
            }
          }
          if (i === 1) {
            //  determining a winner
            this.winner = document.getElementById('1').textContent
            for (j = 1; j < 8;) {
              document.getElementById(j.toString()).style.color = 'green'
              j = j + 3
            }
          }
          if (i === 2) {
            //  determining a winner
            this.winner = document.getElementById('2').textContent
            for (j = 2; j < 9;) {
              document.getElementById(j.toString()).style.color = 'green'
              j = j + 3
            }
          }
        }
      }
      for (i = 0; i < 2; i++) {
        if ((allEqual(diagonals[i])) && (diagonals[i][0] !== '')) {
          console.log('game ended diag: ', i)
          this.game = true
          //  decoration
          if (i === 0) {
            //  determining a winner
            this.winner = document.getElementById('0').textContent
            for (j = 0; j < 9;) {
              document.getElementById(j.toString()).style.color = 'green'
              console.log(j)
              j = j + 4
            }
          }
          if (i === 1) {
            //  determining a winner
            this.winner = document.getElementById('2').textContent
            for (j = 2; j < 7;) {
              document.getElementById(j.toString()).style.color = 'green'
              console.log(j)
              j = j + 2
            }
          }
        }
      }
    }
  },
  watch: {
    xIds: function (newVal, oldVal) {
      setTimeout(this.DrawO, 1000)
      this.checkState()
    },
    oIds: function (newVal, oldVal) {
      this.checkState()
    }
  }
}
</script>

<style>
table {
 width: 33rem;
 height: 33rem;
}
.text_center {
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  padding: 10px;
  font-size: 2rem;
  color: chocolate;
}
td {
  height: 11rem;
  width: 11rem;
  font-size: 7rem;
  text-align: center;
}
table, td {
  border: 0.3rem solid black;
  border-radius: 1rem;
  border-color: grey;
}
.center {
  margin-left: auto;
  margin-right: auto;
}
@media only screen and (max-width: 560px) {
  table {
    width: 24rem;
    height: 24rem;
  }
  td {
    text-align: center;
    font-size: 3.5rem;
    height: 8rem;
    width: 8rem;
  }
}
@media only screen and (max-width: 420px) {
  table {
    width: 18rem;
    height: 18rem;
  }
  td {
    text-align: center;
    font-size: 3.5rem;
    height: 6rem;
    width: 6rem;
  }
}
</style>
