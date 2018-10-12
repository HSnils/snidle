<template>
  <div class="content">
    <div>
      <div class="points" :v-bind="points">
        <h1>{{points}}</h1>
      </div>
      <div class="button" v-on:click="clicked">
        <h3>CLICK</h3>
      </div>
      <p>Points per second: {{pps}}</p>
      <p>Points per click: {{cpc}}</p>
    </div>
    <div class="sidebar">
      <div id="shopBox" class="sidebarBox" style="display: none">
        <h3>Shop</h3>
        <ul class="sideList">
          <li class="item" v-for="item in items" :key="item.id" :title="'Adds '+ item.damage +' '+item.type +' damage'">
            <span>{{item.name}}
              <b>lv.{{item.level}}</b>
            </span>
            <span v-if="points >= item.price" class="buy-item can-buy" v-on:click="buy(item.id)">BUY {{item.price}}</span>
            <span v-else class="buy-item cant-buy" v-on:click="buy(item.id)">BUY {{item.price}}</span>
          </li>
        </ul>
      </div>
      <div id="achiBox" class="sidebarBox" style="display: none">
        <h3>Stats</h3>
        <ul class="sideList">
          <li>
            <div>Times clicked:</div>
            <div :v-bind="clicks">{{clicks}}</div>
          </li>
          <li>
            <div>Total earned:</div>
            <div :v-bind="totalPoints">{{totalPoints}}</div>
          </li>
        </ul>
        <h3 style="margin-top: 1em">Achievments</h3>
        <ul class="sideList">
          <li>
            <div>Click 100 times:</div>
            <div v-if="clicks < 100" :v-bind="clicks">{{clicks}}/100</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>Click 1000 times:</div>
            <div v-if="clicks < 1000" :v-bind="clicks">{{clicks}}/1000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>Click 10000 times:</div>
            <div v-if="clicks < 10000" :v-bind="clicks">{{clicks}}/10000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <br>
          <li>
            <div>dayumers:</div>
            <div v-if="totalPoints < 10000" :v-bind="totalPoints">{{totalPoints}}/10000</div>
            <div class="completedAchi" v-else>Completed</div>
          </li>
          <li>
            <div>Richboi:</div>
            <div v-if="totalPoints < 10000000" :v-bind="totalPoints">{{totalPoints}}/10000000</div>
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
      items: [
        {
          id: 1,
          type: 'click',
          name: 'Small sword',
          damage: 2,
          price: 100,
          level: 0
        },
        {
          id: 2,
          type: 'auto',
          name: 'Rat',
          damage: 1,
          price: 1000,
          level: 0
        },
        {
          id: 3,
          type: 'auto',
          name: 'Warrior',
          damage: 5,
          price: 5000,
          level: 0
        },
        {
          id: 4,
          type: 'click',
          name: 'Big sword',
          damage: 10,
          price: 10000,
          level: 0
        },
        {
          id: 5,
          type: 'auto',
          name: 'Army',
          damage: 50,
          price: 100000,
          level: 0
        },
        {
          id: 6,
          type: 'click',
          name: 'Katana',
          damage: 25,
          price: 100000,
          level: 0
        },
        {
          id: 7,
          type: 'auto',
          name: 'Kingdom',
          damage: 1000,
          price: 100000000,
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
    buy (itemId) {
      const self = this
      this.items.forEach(function (item) {
        if (item.id === itemId) {
          if (self.points >= item.price) {
            // updates item
            ++item.level
            item.price = parseInt(item.price) * 2
            item.damage = parseInt(item.damage) * 1.5

            // updates user
            self.points = parseInt(self.points) - parseInt(item.price)
            if (item.type === 'auto') {
              self.pps = parseInt(self.pps) + parseInt(item.damage)
            } else if (item.type === 'click') {
              self.cpc = parseInt(self.cpc) + parseInt(item.damage)
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
</style>
