<template>
  <nav>
    <ul class="navList">
      <li class="shopNav">SHOP</li>
      <li class="achiNav">EXTRA</li>
    </ul>
  </nav>
</template>
<script>
export default {
  name: 'Nav',
  methods: {
    showShop () {
      let shop = document.querySelector('#shopBox')
      if (shop.style.display !== 'block') {
        shop.style.display = 'block'
        document.querySelector('.shopNav').classList.add('activeNav')
        this.hideAchievments()
      } else {
        this.hideShop()
      }
    },
    hideShop () {
      document.querySelector('#shopBox').style.display = 'none'
      document.querySelector('.shopNav').classList.remove('activeNav')
    },
    showAchievments () {
      let achi = document.querySelector('#achiBox')
      if (achi.style.display !== 'block') {
        achi.style.display = 'block'
        document.querySelector('.achiNav').classList.add('activeNav')
        this.hideShop()
      } else {
        this.hideAchievments()
      }
    },
    hideAchievments () {
      document.querySelector('#achiBox').style.display = 'none'
      document.querySelector('.achiNav').classList.remove('activeNav')
    }
  },
  mounted () {
    document.querySelector('.shopNav').addEventListener('click', this.showShop)
    document.querySelector('.achiNav').addEventListener('click', this.showAchievments)
  }
}
</script>

<style>
nav {
  color: black;
  height: 50px;
}

.navList {
  height: 100%;
  display: flex;
  justify-content: flex-end;
  list-style: none;
  align-items: center;
}

.navList > li {
  margin-right: 1.5em;
  font-weight: 800;
  cursor: pointer;
  padding: 1em;
}

.navList > li:hover {
  background-color: rgba(0, 0, 0, 0.1);
}

.activeNav {
  text-decoration: underline;
}
</style>
