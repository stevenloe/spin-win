<template>
  <div>
    <div>
      <b-container>
        <div>
          <div class="heading marg-right">
            Spin & Win
            <span v-show="justAdded.name" class="gray marg-right"
              ><span class="bolder">Last Entry: </span>
              {{ justAdded.entries }} of : {{ justAdded.name }} </span
            ><span class="gray"> Total Entries: {{ contenders.length }}</span>
          </div>

          <b-alert show variant="danger" v-show="hasError">{{
            errorMsg
          }}</b-alert>

          <b-input-group
            prepend="Add Contender:"
            class="mb-2"
            @keyup.enter="add()"
          >
            <b-form-input
              v-model="contender"
              ref="name"
              placeholder="Name"
              @focus="hasError = false"
            ></b-form-input>

            <b-form-input
              type="number"
              placeholder="entries"
              v-model="entries"
              @focus="hasError = false"
            ></b-form-input>

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

        <div class="top5" v-show="winner.length">
          <b-jumbotron header="We spun, you won!" lead="And the winner is...">
            <h1 class="big">{{ winner }}</h1>
            <b-button @click="winner = ''">Dismiss</b-button>
          </b-jumbotron>
        </div>

        <b-list-group class="top5">
          <b-list-group-item
            class="d-flex justify-content-between align-items-center"
            v-for="(contender, i) in contenders"
            v-bind:key="contender.id"
          >
            {{ contender.text }}

            <b-button @click="delete(i)">X</b-button>
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
      entries: 1,
      winner: '',
      justAdded: { text: '', entries: 0 },
      errorMsg: '',
      hasError: false,
    }
  },
  methods: {
    add() {
      if (this.contender.trim().length === 0) {
        this.errorMsg = 'Name cannot be blank.'
        this.hasError = true
        return
      } else if (this.entries < 1) {
        this.errorMsg = 'entries must be greater than 0.'
        this.hasError = true
        return
      }

      this.hasError = false
      let date = new Date().valueOf().toString()
      this.justAdded.name = this.contender
      this.justAdded.entries = this.entries

      for (let index = 0; index < this.entries; index++) {
        this.contenders.unshift({
          text: this.contender,
          id: date + '-' + index,
        })
      }
      this.contender = ''
      this.entries = 1

      this.$refs.name.focus()
    },
    delete(i) {
      this.contenders.splice(i, 1)
      contenders
    },
    spin() {
      if (this.contenders.length === 0) {
        this.errorMsg =
          'You must add contenders before a winner can be selected.'
        this.hasError = true
        return
      }

      let windex = Math.floor(
        Math.random() * Math.floor(this.contenders.length)
      )
      let winnerData = this.contenders[windex]
      this.winner = winnerData.text
      this.contenders.splice(windex)
    },
  },
}
</script>

<style scoped>
.top5 {
  margin-top: 3rem;
}
.big {
  font-size: 8em;
  color: royalblue;
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
