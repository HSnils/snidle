<template>
  <div class="content">
    <div class="clicker-area" v-on:click="clicked">
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
          <div v-if="items[3].level >= 1" class="buff" v-bind:class="{inUse: doublePPSactive}" title="Double your Points per second for 30sec, 5min cooldown" v-on:click="doublePPS()">x2</div>
          <div class="buff evolveBuff" v-if="items[9].level >= 1" v-on:click="evolve()" title="Click to evolve">!+!</div>
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
            <div>Times evolved:</div>
            <div :v-bind="evolveLvl">{{evolveLvl}}</div>
          </li>
          <li>
            <div>Evolve bonus:</div>
            <div :v-bind="evolveLvl">{{evolveLvl * 10}}% pps</div>
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
            <div v-if="totalPoints < 100000000000000" :v-bind="totalPoints" title="HINT: ITS 100,000,000,000,000">{{numberWithCommas(totalPoints)}}/?</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <br>
          <li>
            <div>Evolved:</div>
            <div v-if="evolveLvl < 1" :v-bind="evolveLvl">{{numberWithCommas(evolveLvl)}}/1</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>Evolver:</div>
            <div v-if="evolveLvl < 10" :v-bind="evolveLvl">{{numberWithCommas(evolveLvl)}}/10</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>Pokemon master:</div>
            <div v-if="evolveLvl < 100" :v-bind="evolveLvl">{{numberWithCommas(evolveLvl)}}/100</div>
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
      evolveLvl: 0,
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
        this.items = JSON.parse(this.$cookies.get('items'))
      }

      if (this.$cookies.isKey('pps')) {
        this.pps = this.$cookies.get('pps')
      }

      if (this.$cookies.isKey('cpc')) {
        this.cpc = this.$cookies.get('cpc')
      }

      if (this.$cookies.isKey('evolve')) {
        this.evolveLvl = this.$cookies.get('evolve')
      }
    },
    increaseClick () {
      this.clicks = ++this.clicks
    },
    increasePoint () {
      this.points = this.round((parseInt(this.points) + parseInt(this.pps)), 1)
      this.totalPoints = this.round((parseInt(this.totalPoints) + parseInt(this.pps)), 1)
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
              self.pps = (parseInt(self.pps) + parseInt(item.damage) * (1 + (0.1 * self.evolveLvl)))
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
                item.price = self.round(item.price * 1.37, 1)
              } else if (item.level >= 90) {
                item.name = 'SwordInATopHat'
                item.price = self.round(item.price * 1.35, 1)
              } else if (item.level >= 80) {
                item.name = 'The T-Bone'
                item.price = self.round(item.price * 1.33, 1)
              } else if (item.level >= 70) {
                item.name = 'Burning Sword'
                item.price = self.round(item.price * 1.31, 1)
              } else if (item.level >= 60) {
                item.name = 'Magic Sword'
                item.price = self.round(item.price * 1.29, 1)
              } else if (item.level >= 50) {
                item.name = 'Massive Sword'
                item.price = self.round(item.price * 1.27, 1)
              } else if (item.level >= 40) {
                item.name = 'Big Sword'
                item.price = self.round(item.price * 1.25, 1)
              } else if (item.level >= 30) {
                item.name = 'Gold Sword'
                item.price = self.round(item.price * 1.23, 1)
              } else if (item.level >= 20) {
                item.name = 'Iron Sword'
                item.price = self.round(item.price * 1.21, 1)
              } else if (item.level >= 10) {
                item.name = 'Rusty Sword'
                item.price = self.round(item.price * 1.19, 1)
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
      this.$cookies.set('evolve', this.evolveLvl, -1)
    },
    evolve () {
      this.evolveLvl = ++this.evolveLvl
      this.points = 0
      this.cpc = 1
      this.pps = 1
      this.items = [
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

@media only screen and (max-width: 738px) {
  .sidebar {
    width: 100%;
    top: 6%;
  }
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
  display: flex;
  justify-content: space-around;
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

.evolveBuff {
  border: 1px solid var(--green);
}
.clicker-area {
  cursor: pointer;
}
</style>
