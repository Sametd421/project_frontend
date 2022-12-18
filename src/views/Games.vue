<template>
  <h1>Welcome to Games</h1>
  <div class="container-fluid">
    <div class="row row-cols-1 row-cols-md-4 g-4">
      <div class="col" v-for="game in games" :key="game.id">
        <div class="card h-100">
          <img :src="getAvatar(game)" class="card-img-top" :alt="game.title">
          <div class="card-body">
            <h5 class="card-title">{{ game.title }} </h5>
            <p class="card-text">
              {{ game.title }} ist {{ game.finished ? 'finished' : 'noch nicht finished' }}.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Games',
  data () {
    return {
      games: []
    }
  },
  methods: {
    getAvatar (game) {
      if (game.genre === 'MALE') {
        return require('../assets/PSLogo.png')
      } else if (game.genre === 'FEMALE') {
        return require('../assets/SuperMario.png')
      }
    }
  },
  mounted () {
    const endpoint = process.env.VUE_APP_BACKEND_BASE_URL + '/api/v1/games'
    const requestOptions = {
      method: 'GET',
      redirect: 'follow'
    }
    fetch(endpoint, requestOptions)
      .then(response => response.json())
      .then(result => result.forEach(game => {
        this.games.push(game)
      }))
      .catch(error => console.log('error', error))
  }
}
</script>

<style scoped>
</style>
