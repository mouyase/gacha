<template>
  <div id="gacha">
    <div class="gacha-result" v-show="gachaDataArray.length === 10">
      <div class="gacha-result-box" v-for="item in gachaDataArray" :key="item.id" @click="goToPixiv(item)">
        <img class="gacha-result-image" :style="{'background-image': 'url(' + item.url + ')'}">
      </div>
    </div>
    <button @click="gacha">Gacha!</button>
  </div>
</template>

<script>
export default {
  name: 'Gacha',
  data() {
    return {
      gachaDataArray: []
    }
  },
  mounted() {
    // this.$http.get("https://www.pixiv.cool/ajax/illust/91490130").then(response => {
    //   console.log(response.data)
    // })
  },
  methods: {
    gacha() {
      this.gachaDataArray = []
      let dateArray = this.getRandomDateArray()
      dateArray.forEach(item => {
        this.$http.get("https://api.pixiv.cool/ranking.php?format=json&mode=daily&p=1&date=" + item).then(response => {
          if (response) {
            if (response.data) {
              if (response.data.contents) {
                let illust = response.data.contents[Math.round(Math.random() * 20)]
                this.gachaDataArray.push({
                  id: illust.illust_id,
                  title: illust.title,
                  date: illust.date,
                  url: illust.url.replace(/c\/[0-9]+x[0-9]+/, "").replace("i.pximg.pixiv.cool", "i.pixiv.cat")
                })
              }
            }
          }
        }).catch(error => {
        })
      })
    },
    getDay(day) {
      let targetTime = new Date().getTime() - 1000 * 60 * 60 * 24 * day;
      let targetDate = new Date(targetTime)
      let Y = targetDate.getFullYear()
      let M = targetDate.getMonth() + 1
      let D = targetDate.getDate()
      M = M < 10 ? "0" + M : M;
      D = D < 10 ? "0" + D : D;
      return `${Y}${M}${D}`
    },
    getRandomDateArray() {
      let today = new Date().getTime()
      let longLongAgo = new Date("2010-01-01").getTime()
      let longTime = Math.floor((today - longLongAgo) / 1000 / 60 / 60 / 24)
      let randomDateSet = new Set
      for (let i = 0; i < 10; i++) {
        let date = this.getDay(Math.round(Math.random() * longTime))
        if (!randomDateSet.has(date)) {
          randomDateSet.add(date)
        } else {
          i--
        }
      }
      return Array.from(randomDateSet)
    },
    goToPixiv(item) {
      let pixivUrl = `https://www.pixiv.cool/artworks/${item.id}`
      window.open(pixivUrl)
    }
  }
}
</script>

<style scoped>
.gacha-result {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.gacha-result-image {
  width: 200px;
  height: 200px;
  margin: 8px;
  background-size: cover;
  background-position: center center;
}
</style>
