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
      yIds: [0, 1, 2, 3, 4, 5, 6, 7, 8]
    }
  },
  methods: {
    rand: function (items) {
      // "~~" for a closest "int"
      return items[~~(items.length * Math.random())]
    },
    DrawX: function (event) {
      var id = event.currentTarget.id
      document.getElementById(id).textContent = 'X'
      this.xIds.push(id)
    },
    DrawO: function () {
      //  parsing ecach string number to number
      var i
      for (i in this.xIds) {
        this.xIds[i] = Number(this.xIds[i])
      }

      console.log('x s: ', this.xIds)

      // !intersection
      var self = this
      var extract = (num) => {
        if (!self.xIds.includes(num)) return num
      }
      this.yIds = this.yIds.filter(extract)
      // choosing random element from yIds
      var restFunc = () => {
        var randomElement = this.rand(this.yIds)
        document.getElementById(randomElement.toString()).textContent = 'O'
        console.log('rand:', randomElement)
        this.yIds.pop(4)
        console.log('y s: ', this.yIds)
        console.log('')
      }
      setTimeout(restFunc, 500)
    }
  },
  watch: {
    xIds: function (val) {
      this.DrawO()
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
