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
  </div>
</template>

<script>

export default {
  name: 'PageIndex',
  data () {
    return {
      xIds: [],
      oIds: [0, 1, 2, 3, 4, 5, 6, 7, 8],
      game: false
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
      this.xIds.forEach((item) => {
        if (this.oIds.includes(item)) {
          var indexItem = this.oIds.indexOf(item)
          if (indexItem !== -1) {
            this.oIds.splice(indexItem, 1)
          }
        }
      })
      var randomElement = this.rand(this.oIds)
      if (!this.game) {
        document.getElementById(randomElement.toString()).textContent = 'O'
      }

      var index = this.oIds.indexOf(randomElement)
      if (index !== -1) {
        this.oIds.splice(index, 1)
      }
      console.log(this.oIds)
    },
    checkState: function () {
      var td = document.querySelectorAll('td')
      const allEqual = arr => arr.every(v => v === arr[0])
      var rows = [[td[0].textContent, td[1].textContent, td[2].textContent], [td[3].textContent, td[4].textContent, td[5].textContent], [td[6], td[7], td[8]]]
      var columns = [[td[0].textContent, td[3].textContent, td[6].textContent], [td[1].textContent, td[4].textContent, td[7].textContent], [td[2].textContent, td[5].textContent, td[8].textContent]]
      var diagonals = [[td[0].textContent, td[4].textContent, td[8].textContent], [td[2].textContent, td[4].textContent, td[6].textContent]]

      var i
      for (i in rows) {
        if ((allEqual(rows[i])) && (rows[i][0] !== '')) {
          console.log('game ended')
          this.game = true
          //  decoration
        }
      }
      for (i in columns) {
        if ((allEqual(columns[i])) && (columns[i][0] !== '')) {
          console.log('game ended')
          this.game = true
        }
      }
      for (i in diagonals) {
        if ((allEqual(diagonals[i])) && (diagonals[i][0] !== '')) {
          console.log('game ended')
          this.game = true
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
 width: 35rem;
}
td {
  text-align: center;
  height: 10rem;
  font-size: 5rem;
  width: 20rem;
}
table, td {
  border: 1px solid black;
}
.center {
  margin-left: auto;
  margin-right: auto;
}
</style>
