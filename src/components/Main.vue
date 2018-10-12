<template>
  <div class="content">
      <div>
        <div class="points" :v-bind="points"><h1>{{points}}</h1></div>
        <div class="button"><h3>CLICK</h3></div>
        <div class="clicks" :v-bind="clicks">Times clicked: {{clicks}}</div>
      </div>
      <Sidebar />
  </div>

</template>

<script>
import Sidebar from '@/components/Sidebar'

export default {
  data: function () {
    return {
      points: 0,
      clicks: 0,
      interval: null
    }
  },
  components: {
    Sidebar
  },
  methods: {
    getPoints () {
      if (this.$cookies.isKey('clicks')) {
        this.clicks = this.$cookies.get('clicks')
      } else {
        this.clicks = 0
      }

      if (this.$cookies.isKey('points')) {
        this.points = this.$cookies.get('points')
      } else {
        this.points = 0
      }
    },
    increasePoints () {
      this.interval = setInterval(this.increasePoint, 1000)
    },
    increasePoint () {
      this.points = ++this.points
    },
    increaseClicks () {
      this.clicks = ++this.clicks
    },
    save () {
      this.$cookies.set('points', this.points, -1)
      this.$cookies.set('clicks', this.clicks, -1)
    }
  },
  created () {
    this.getPoints()
    this.increasePoints()
    window.addEventListener('beforeunload', this.save)
  },
  mounted () {
    document
      .querySelector('.button')
      .addEventListener('click', this.increasePoint)
    document
      .querySelector('.button')
      .addEventListener('click', this.increaseClicks)
  }
}
</script>

<style>
.points {
  color: #efcc4c;
  text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.2);
}

.button {
  cursor: pointer;
}

.button:hover {
  color: rgba(0, 0, 0, 0.5);
}
.button:active {
  color: #efcc4c;
}
</style>
