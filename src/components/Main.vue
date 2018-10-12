<template>
  <div class="content">
    <div>
      <div class="points" :v-bind="points">
        <h1>{{numberWithCommas(points)}}</h1>
      </div>
      <div class="button" v-on:click="clicked">
        <h3>CLICK</h3>
      </div>
      <p>Points per second: {{numberWithCommas(pps)}}</p>
      <p>Points per click: {{numberWithCommas(cpc)}}</p>
    </div>
    <div class="sidebar">

      <!-- SHOP -->
      <div id="shopBox" class="sidebarBox" style="display: none">
        <h3>Shop</h3>
        <ul class="sideList">
          <li class="item" v-for="item in items" :key="item.id" :title="'Adds '+ numberWithCommas(item.damage) +' '+item.type +' damage'">
            <span>{{item.name}}
              <b>lv.{{numberWithCommas(item.level)}}</b>
            </span>
            <span v-if="points >= item.price" class="buy-item can-buy" v-on:click="buy(item.id)">BUY {{numberWithCommas(item.price)}}</span>
            <span v-else class="buy-item cant-buy" v-on:click="buy(item.id)">BUY {{numberWithCommas(item.price)}}</span>
          </li>
        </ul>
        <div class="shopBuffsBox">
          <div class="buff" v-bind:class="{inUse: doublePPSactive}" title="Double your Points per second for 30sec" v-on:click="doublePPS()">x2</div>
        </div>
      </div>

      <!-- EXTRAS -->
      <div id="achiBox" class="sidebarBox" style="display: none">
        <h3>Stats</h3>
        <ul class="sideList">
          <li>
            <div>Times clicked:</div>
            <div :v-bind="clicks">{{numberWithCommas(clicks)}}</div>
          </li>
          <li>
            <div>Total earned:</div>
            <div :v-bind="totalPoints">{{numberWithCommas(totalPoints)}}</div>
          </li>
          <br>
          <li>
            <div>Sword:</div>
            <div :v-bind="items[0].name">{{items[0].name}}</div>
          </li>
        </ul>
        <h3 style="margin-top: 1em">Achievments</h3>
        <ul class="sideList">
          <li>
            <div>100 clicks:</div>
            <div v-if="clicks < 100" :v-bind="clicks">{{clicks}}/100</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>1000 clicks:</div>
            <div v-if="clicks < 1000" :v-bind="clicks">{{clicks}}/1,000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>10,000 clicks:</div>
            <div v-if="clicks < 10000" :v-bind="clicks">{{numberWithCommas(clicks)}}/10,000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>1,000,000 clicks:</div>
            <div v-if="clicks < 1000000" :v-bind="clicks">{{numberWithCommas(clicks)}}/1,000,000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <br>
          <li>
            <div>10,000 earned:</div>
            <div v-if="totalPoints < 10000" :v-bind="totalPoints">{{numberWithCommas(totalPoints)}}/10,000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>1,000,000 earned:</div>
            <div v-if="totalPoints < 1000000" :v-bind="totalPoints">{{numberWithCommas(totalPoints)}}/1,000,000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>10,000,000 earned:</div>
            <div v-if="totalPoints < 10000000" :v-bind="totalPoints">{{numberWithCommas(totalPoints)}}/10,000,000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>Good luck:</div>
            <div v-if="totalPoints < 100000000000000" :v-bind="totalPoints">{{numberWithCommas(totalPoints)}}/100,000,000,000,000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
        </ul>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  data: function () {
    return {
      points: 0,
      clicks: 0,
      totalPoints: 0,
      interval: null,
      pps: 1, /* Points per second */
      cpc: 1, /* Clicks per click */
      doublePPSactive: false,
      items: [
        {
          id: 1,
          type: 'click',
          name: 'Small Sword',
          damage: 1,
          price: 15,
          level: 0
        },
        {
          id: 2,
          type: 'auto',
          name: 'Peasant',
          damage: 1,
          price: 100,
          level: 0
        },
        {
          id: 3,
          type: 'auto',
          name: 'Thief',
          damage: 4,
          price: 500,
          level: 0
        },
        {
          id: 4,
          type: 'auto',
          name: 'Warrior',
          damage: 10,
          price: 3000,
          level: 0
        },
        {
          id: 5,
          type: 'auto',
          name: 'Bounty Hunter',
          damage: 40,
          price: 10000,
          level: 0
        },
        {
          id: 6,
          type: 'auto',
          name: 'Assasin',
          damage: 100,
          price: 40000,
          level: 0
        },
        {
          id: 7,
          type: 'auto',
          name: 'Wizard',
          damage: 400,
          price: 200000,
          level: 0
        },
        {
          id: 8,
          type: 'auto',
          name: 'Undead Lord',
          damage: 6666,
          price: 1666666,
          level: 0
        },
        {
          id: 9,
          type: 'auto',
          name: 'Dragon',
          damage: 98765,
          price: 123456789,
          level: 0
        },
        {
          id: 10,
          type: 'auto',
          name: 'Dragon Slayer',
          damage: 999999,
          price: 3999999999,
          level: 0
        },
        {
          id: 11,
          type: 'auto',
          name: 'Overlord',
          damage: 10000000,
          price: 75000000000,
          level: 0
        },
        {
          id: 12,
          type: 'auto',
          name: 'Taxes',
          damage: 1000000000,
          price: 999999999999,
          level: 0
        }
      ]

    }
  },
  methods: {
    getCookies () {
      if (this.$cookies.isKey('clicks')) {
        this.clicks = this.$cookies.get('clicks')
      }

      if (this.$cookies.isKey('points')) {
        this.points = this.$cookies.get('points')
      }

      if (this.$cookies.isKey('totalPoints')) {
        this.totalPoints = this.$cookies.get('totalPoints')
      }

      if (this.$cookies.isKey('items')) {
        // this.items = JSON.parse(this.$cookies.get('items'))
      }

      if (this.$cookies.isKey('pps')) {
        this.pps = this.$cookies.get('pps')
      }

      if (this.$cookies.isKey('cpc')) {
        this.cpc = this.$cookies.get('cpc')
      }
    },
    increaseClick () {
      this.clicks = ++this.clicks
    },
    increasePoint () {
      this.points = parseInt(this.points) + parseInt(this.pps)
      this.totalPoints = parseInt(this.totalPoints) + parseInt(this.pps)
    },
    increasePointsInterval () {
      this.interval = setInterval(this.increasePoint, 1000)
    },
    clicked () {
      this.points = parseInt(this.points) + parseInt(this.cpc)
      this.totalPoints = parseInt(this.totalPoints) + parseInt(this.cpc)
      this.increaseClick()
    },
    doublePPS () {
      if (this.doublePPSactive === false) {
        this.doublePPSactive = true
        const self = this
        self.pps = self.pps * 2

        setTimeout(
          function () {
            self.pps = self.pps / 2
          }
          , 30000)

        setTimeout(
          function () {
            self.doublePPSactive = false
          }
          , 300000)
      }
    },
    round (value, precision) {
      var multiplier = Math.pow(10, precision || 0)
      return Math.round(value * multiplier) / multiplier
    },
    numberWithCommas (number) {
      return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },
    buy (itemId) {
      const self = this
      this.items.forEach(function (item) {
        if (item.id === itemId) {
          if (self.points >= item.price) {
            // updates user
            self.points = parseInt(self.points) - parseInt(item.price)
            if (item.type === 'auto') {
              self.pps = parseInt(self.pps) + parseInt(item.damage)
            } else if (item.type === 'click') {
              self.cpc = parseInt(self.cpc) + parseInt(item.damage)
            }

            // updates item
            ++item.level
            item.price = self.round(item.price * 1.15, 1)
            item.damage = self.round(item.damage * 1.1, 1)

            if (item.type === 'click') {
              if (item.level >= 1000) {
                item.name = '8===D'
              } else if (item.level >= 100) {
                item.name = 'Excalibur'
              } else if (item.level >= 90) {
                item.name = 'SwordInATopHat'
              } else if (item.level >= 80) {
                item.name = 'The T-Bone'
              } else if (item.level >= 70) {
                item.name = 'Burning Sword'
              } else if (item.level >= 60) {
                item.name = 'Magic Sword'
              } else if (item.level >= 50) {
                item.name = 'Massive Sword'
              } else if (item.level >= 40) {
                item.name = 'Big Sword'
              } else if (item.level >= 30) {
                item.name = 'Gold Sword'
              } else if (item.level >= 20) {
                item.name = 'Iron Sword'
              } else if (item.level >= 10) {
                item.name = 'Rusty Sword'
              }
            }
          }
        }
      })
    },
    save () {
      this.$cookies.set('points', this.points, -1)
      this.$cookies.set('clicks', this.clicks, -1)
      this.$cookies.set('totalPoints', this.totalPoints, -1)
      this.$cookies.set('items', JSON.stringify(this.items), -1)
      this.$cookies.set('pps', this.pps, -1)
      this.$cookies.set('cpc', this.cpc, -1)
    }
  },
  created () {
    this.getCookies()
    this.increasePointsInterval()
    window.addEventListener('beforeunload', this.save)
  }
}
</script>

<style>
:root {
  --red: rgb(228, 96, 96);
  --green: rgb(96, 228, 96);
  --gold: #efcc4c;
}

.points {
  color: var(--gold);
  text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.2);
}

.button {
  cursor: pointer;
}

.button:hover {
  color: rgba(0, 0, 0, 0.5);
}

.button:active {
  color: var(--gold);
}

.sidebar {
  position: fixed;
  top: 5%;
  right: 0;
  bottom: 0;
  width: 20%;
  height: 100%;
}

.sidebarBox {
  color: white;
  width: 100%;
  height: 100%;
  background: #778599;
  padding: 1em;
}

.sideList {
  list-style: none;
  display: flex;
  flex-flow: column wrap;
  justify-content: space-between;
}

.sideList > li {
  display: flex;
  justify-content: space-between;
  padding: 2%;
  border: 1px solid black;
  align-items: center;
  align-content: center;
  background-color: rgba(0, 0, 0, 0.1);
}

h3 {
  margin-bottom: 1em;
}

.completedAchi {
  color: var(--green);
}

.buy-item {
  box-sizing: border-box;
  cursor: pointer;
  padding: 5px;
}

.can-buy {
  background-color: var(--green);
}

.cant-buy {
  background-color: var(--red);
}

.item:first-child {
  margin-bottom: 1em;
}
.shopBuffsBox {
  position: absolute;
  bottom: 10%;
  width: 100%;
}
.buff {
  border: 1px solid var(--gold);
  background-color: rgba(0, 0, 0, 0.3);
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.inUse {
  border: 1px solid var(--red) !important;
}
</style>
