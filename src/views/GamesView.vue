<template>
  <h1 style="color: white">Willkommen zum GamePlaner!</h1>
  <div class="container-fluid">
    <div class="row row-cols-1 row-cols-md-4 g-4">
      <div class="col-3">
        <div class="input-group flex-nowrap search">
        <span class="input-group-text" id="addon-wrapping">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-search" viewBox="0 0 16 16">
            <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
          </svg>
        </span>
          <input type="text" class="form-control" placeholder="Nach Titel suchen" v-model="searchText" @keyup="search()" aria-label="Suche" aria-describedby="addon-wrapping">
        </div>
      </div>
      <div class="col-8"></div>
      <div class="col-1"></div>
      <button type="button" class="btn btn-primary add-btn" data-bs-toggle="offcanvas" data-bs-target="#games-offcanvas" aria-controls="#games-offcanvas">Hinzufügen</button>
    </div>
    <table class="table games-table">
      <thead>
      <tr>
        <th scope="col">Titel</th>
        <th scope="col">Konsole</th>
        <th scope="col">Genre</th>
        <th scope="col">Erscheinungsjahr</th>
        <th scope="col">Startdatum</th>
        <th scope="col">Abgeschlossen</th>
        <th scope="col">Kommentar</th>
        <th scope="col">Aktion</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="game in filteredGames" :key="game.id">
        <td>{{game.title}}</td>
        <td>{{game.console}}</td>
        <td>{{game.genre}}</td>
        <td>{{game.releaseYear}}</td>
        <td>{{game.startDate}}</td>
        <td>
          <div class="form-check finished-check">
            <input class="form-check-input" type="checkbox" disabled v-model="game.finished" id="flexCheckDefault">
          </div>
        </td>
        <td>{{game.comment}}</td>
        <td>
          <svg v-if="!game.favorised" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-star action-icons" viewBox="0 0 16 16">
            <path d="M2.866 14.85c-.078.444.36.791.746.593l4.39-2.256 4.389 2.256c.386.198.824-.149.746-.592l-.83-4.73 3.522-3.356c.33-.314.16-.888-.282-.95l-4.898-.696L8.465.792a.513.513 0 0 0-.927 0L5.354 5.12l-4.898.696c-.441.062-.612.636-.283.95l3.523 3.356-.83 4.73zm4.905-2.767-3.686 1.894.694-3.957a.565.565 0 0 0-.163-.505L1.71 6.745l4.052-.576a.525.525 0 0 0 .393-.288L8 2.223l1.847 3.658a.525.525 0 0 0 .393.288l4.052.575-2.906 2.77a.565.565 0 0 0-.163.506l.694 3.957-3.686-1.894a.503.503 0 0 0-.461 0z"/>
          </svg>
          <svg v-if="game.favorised" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-star-fill action-icons" viewBox="0 0 16 16">
            <path d="M3.612 15.443c-.386.198-.824-.149-.746-.592l.83-4.73L.173 6.765c-.329-.314-.158-.888.283-.95l4.898-.696L7.538.792c.197-.39.73-.39.927 0l2.184 4.327 4.898.696c.441.062.612.636.282.95l-3.522 3.356.83 4.73c.078.443-.36.79-.746.592L8 13.187l-4.389 2.256z"/>
          </svg>
          <svg data-bs-toggle="offcanvas" data-bs-target="#games-offcanvas" aria-controls="#games-offcanvas" @click="openEditForm(game)" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil action-icons" viewBox="0 0 16 16">
            <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
          </svg>
          <svg @click="deleteGame(game.id)" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash action-icons" viewBox="0 0 16 16">
            <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
            <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
          </svg>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

  <!-- Modal -->
  <div class="offcanvas offcanvas-end" tabindex="-1" id="games-offcanvas" aria-labelledby="offcanvas-label">
    <div class="offcanvas-header">
      <h5 id="offcanvas-label">Spiel hinzufügen</h5>
      <button type="button" id="close-offcanvas" class="btn-close text-reset" data-bs-dismiss="offcanvas" aria-label="Close"></button>
    </div>
    <div class="offcanvas-body">
      <form class="text-start needs-validation" id="games-form" novalidate>
        <div class="mb-3">
          <label for="title" class="form-label">Titel</label>
          <input type="text" class="form-control" id="title" v-model="currentGame.title" required>
        </div>
        <div class="mb-3">
          <label for="console" class="form-label">Konsole</label>
          <select id="console" class="form-select" v-model="currentGame.console" required>
            <option value="" selected disabled>Auswählen</option>
            <option value="PlayStation">PlayStation</option>
            <option value="Xbox">Xbox</option>
            <option value="Nintendo">Nintendo</option>
            <option value="PC">PC</option>
            <option value="Mobile">Mobile</option>
          </select>
        </div>
        <div class="mb-3">
          <label for="genre" class="form-label">Genre</label>
          <select id="genre" class="form-select" v-model="currentGame.genre" required>
            <option value="" selected disabled>Auswählen</option>
            <option value="ACTION">ACTION</option>
            <option value="SHOOTER">SHOOTER</option>
            <option value="HORROR">HORROR</option>
            <option value="RPG">RPG</option>
            <option value="JUMPNRUN">JUMPNRUN</option>
            <option value="STRATEGY">STRATEGY</option>
            <option value="SPORTS">SPORTS</option>
          </select>
        </div>
        <div class="mb-3">
          <label for="releaseYear" class="form-label">Erscheinungsjahr</label>
          <input type="number" class="form-control" id="releaseYear" v-model="currentGame.releaseYear" required>
        </div>
        <div class="mb-3">
          <label for="startDate" class="form-label">Startdatum</label>
          <input type="text" class="form-control" id="startDate" v-model="currentGame.startDate" required>
        </div>
        <div class="mb-3">
          <div class="form-check">
            <input class="form-check-input" type="checkbox" id="finished" v-model="currentGame.finished">
            <label class="form-check-label" for="finished">
              Abgeschlossen
            </label>
          </div>
        </div>
        <div class="mb-3">
          <label for="comment" class="form-label">Kommentar</label>
          <input type="text" class="form-control" id="comment" v-model="currentGame.comment" required>
        </div>
        <div class="mb-3">
          <div class="form-check">
            <input class="form-check-input" type="checkbox" id="favorised" v-model="currentGame.favorised">
            <label class="form-check-label" for="favorised">
              Favorisiert
            </label>
          </div>
        </div>
        <div class="mt-5">
          <button v-if="!editForm" class="btn btn-primary me-3" type="submit" @click="addGame()">Hinzufügen</button>
          <button v-if="editForm" class="btn btn-primary me-3" type="submit" @click="updateGame()">Speichern</button>
          <button v-if="!editForm" class="btn btn-danger" type="reset">Zurücksetzen</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Games',
  data () {
    return {
      filteredGames: [],
      allGames: [],
      currentGame: {},
      searchText: '',
      editForm: false
    }
  },
  emits: ['created'],
  methods: {
    async addGame () {
      const endpoint = process.env.VUE_APP_BACKEND_BASE_URL + '/api/v1/games'
      const headers = new Headers()
      headers.append('Content-Type', 'application/json')

      const game = JSON.stringify({
        title: this.currentGame.title,
        console: this.currentGame.console,
        genre: this.currentGame.genre,
        releaseYear: this.currentGame.releaseYear,
        startDate: this.currentGame.startDate,
        finished: this.currentGame.finished,
        favorised: this.currentGame.favorised,
        comment: this.currentGame.comment
      })
      const requestOptions = {
        method: 'POST',
        headers: headers,
        body: game,
        redirect: 'follow'
      }
      const response = await fetch(endpoint, requestOptions)
      await this.handleResponse(response)
    },
    async updateGame () {
      const endpoint = process.env.VUE_APP_BACKEND_BASE_URL + '/api/v1/games/' + this.currentGame.id
      const headers = new Headers()
      headers.append('Content-Type', 'application/json')

      const game = JSON.stringify({
        title: this.currentGame.title,
        console: this.currentGame.console,
        genre: this.currentGame.genre,
        releaseYear: this.currentGame.releaseYear,
        startDate: this.currentGame.startDate,
        finished: this.currentGame.finished,
        favorised: this.currentGame.favorised,
        comment: this.currentGame.comment
      })
      const requestOptions = {
        method: 'PUT',
        headers: headers,
        body: game,
        redirect: 'follow'
      }
      const response = await fetch(endpoint, requestOptions)
      await this.handleResponse(response)
    },
    async deleteGame (id) {
      const endpoint = process.env.VUE_APP_BACKEND_BASE_URL + '/api/v1/games/' + id

      const requestOptions = {
        method: 'DELETE',
        redirect: 'follow'
      }
      const response = await fetch(endpoint, requestOptions)
      await this.handleResponse(response)
      await this.getAllGames()
    },
    async getAllGames () {
      this.allGames = []
      this.filteredGames = []
      const endpoint = process.env.VUE_APP_BACKEND_BASE_URL + '/api/v1/games'
      const requestOptions = {
        method: 'GET',
        redirect: 'follow'
      }
      fetch(endpoint, requestOptions)
        .then(response => response.json())
        .then(result => result.forEach(game => {
          this.allGames.push(game)
        }))
        .catch(error => console.log('error', error))
      this.filteredGames = this.allGames
    },
    async handleResponse (response) {
      if (response.ok) {
        this.$emit('created', response.headers.get('location'))
        document.getElementById('close-offcanvas').click()
      } else if (response.status === 400) {
        response = await response.json()
        response.errors.forEach(error => {
          this.serverValidationMessages.push(error.defaultMessage)
        })
      } else {
        this.serverValidationMessages.push('Unknown error occurred')
      }
    },
    search () {
      if (this.searchText) {
        this.filteredGames = this.allGames
      }
      this.filteredGames = this.allGames.filter(game => game.title.includes(this.searchText))
    },
    openEditForm (game) {
      this.currentGame = game
      this.editForm = true
    }
  },
  mounted () {
    this.getAllGames()
  }
}
</script>

<style scoped>

.games-table {
  margin-left: 15px;
  margin-right: 15px;
  color: white;
}

.search {
  width: 250px;
}

.add-btn {
  width: 150px;
  margin-left: 150px;
}

.finished-check {
  padding-left: 9.5em;
}

.action-icons {
  margin-left: 5px;
}
</style>
