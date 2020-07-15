<template>
  <div>
    <div>
      <b-container>
        <div>
          <h5>
            A simple Vue.js app for selecting a contest winner from a list of contestants with 1 - n entires per
            contestant. Each time a contestant wins, that contestants number of entries is reduced by one.
          </h5>

          <div class="heading marg-right">
            Spin & Win
            <span class="gray"> Total chances: {{ contenders.length }}</span>
          </div>

          <b-alert show variant="danger" v-show="errorMsg.length">{{ errorMsg }}</b-alert>

          <b-input-group prepend="Add Contender:" class="mb-2" @keyup.enter="add()">
            <b-form-input v-model="contender" ref="name" placeholder="Name" @focus="errorMsg = ''"></b-form-input>
            <b-form-input type="number" placeholder="chances" v-model="chances" @focus="errorMsg = ''"></b-form-input>

            <b-input-group-append
              ><b-button variant="info" v-on:click="add()">
                Add
              </b-button>
            </b-input-group-append>
          </b-input-group>
        </div>

        <div class="top5">
          <b-button variant="success" @click="spin()">Spin!</b-button>
        </div>

        <div class="top5" v-show="winner.length && errorMsg.length === 0">
          <b-card
            title="We spun, you won!!!"
            img-src="https://picsum.photos/600/300/"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem;"
            class="mb-2"
          >
            <b-card-text>
              And the winner is: <strong>{{ winner }}!</strong>
            </b-card-text>

            <b-button @click="winner = ''">Close</b-button>
          </b-card>
        </div>

        <b-list-group class="top5">
          <b-list-group-item
            class="d-flex justify-content-between align-items-center"
            v-for="(contender, i) in contenders"
            v-bind:key="contender.id"
          >
            <span>{{ contender.text }}</span>
            <span>{{ contender.chances }}</span>

            <b-button @click="deleteContender(i)">X</b-button>
          </b-list-group-item>
        </b-list-group>
      </b-container>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Spin-and-Win',
  data() {
    return {
      contender: '',
      contenders: [],
      chances: 1,
      winner: '',
      errorMsg: '',
    }
  },
  mounted() {
    this.$refs.name.focus()
  },
  methods: {
    add() {
      const name = this.contender.replace(/\b\w/g, (c) => c.toUpperCase()).trim()

      if (this.isInputValid(name) !== true) return

      this.contenders.unshift({
        text: name,
        chances: this.chances,
      })

      this.contender = ''
      this.chances = 1
      this.$refs.name.focus()
    },
    deleteContender(i) {
      this.contenders.splice(i, 1)
    },
    isInputValid(name) {
      let msg = ''
      let isValid = true
      if (this.contenders.some((elem) => elem.text === name)) {
        msg += 'A contestant with that name has already been entered. Please enter a different name. '
        isValid = false
      }
      if (name.length === 0) {
        msg += 'Name cannot be blank. '
        isValid = false
      }
      if (this.chances < 1) {
        msg += 'Number of entries must be a number greater than 0. '
        isValid = false
      }
      this.errorMsg = msg
      return isValid
    },
    spin() {
      if (this.contenders.length === 0) {
        this.errorMsg = 'You must add contenders before a winner can be selected.'
        return
      }

      let items = []

      for (let index = 0; index < this.contenders.length; index++) {
        let item = this.contenders[index]
        for (let count = 0; count < item.chances; count++) {
          items.push(item.text)
        }
      }
      this.winner = items[Math.floor(Math.random() * Math.floor(items.length))]
      console.log('WIN', this.winner)

      let windex = this.contenders.findIndex((element) => element.text === this.winner)
      let element = this.contenders[windex]

      if (element.chances > 1) {
        element.chances--
      } else {
        this.deleteContender(windex)
      }
    },
  },
}
</script>

<style scoped>
.top5 {
  margin-top: 3rem;
}

.heading {
  font-weight: 700;
  font-size: 3em;
}
.gray {
  color: #666;
  font-size: 1rem;
  font-weight: 500;
}
.marg-right {
  margin-right: 3rem;
}
.bolder {
  font-weight: bold;
}
</style>
