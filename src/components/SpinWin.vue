<template>
  <div>
    <div>
      <b-container>
        <div>
          <h5>
            A simple Vue.js app for selecting a contest winner from a list of contestants with 1 - n entires per
            contestant. Each time a contestant wins, that contestant's number of entries is reduced by one.
          </h5>

          <div class="heading marg-right">
            Spin & Win
            <span class="gray"> Total contenders: {{ contenders.length }}</span>
          </div>

          <b-alert show variant="danger" v-show="errorMsg.length">{{ errorMsg }}</b-alert>

          <b-input-group prepend="New Contender:" class="mb-2" @keyup.enter="add()">
            <b-form-input v-model="contender" ref="name" placeholder="Name" @focus="errorMsg = ''"></b-form-input>
            <b-form-input type="number" placeholder="chances" v-model="chances" @focus="errorMsg = ''"></b-form-input>

            <b-input-group-append>
              <b-button variant="primary" v-on:click="add()">
                Add
              </b-button>
            </b-input-group-append>
          </b-input-group>
        </div>

        <div class="top_space">
          <b-button variant="success" v-bind:class="{ disabled_button: isDisabled }" :disabled="isDisabled" size="lg" @click="spin()">Spin!</b-button>
        </div>

        <div class="top_space" v-show="winnerName.length && errorMsg.length === 0">
          <b-card title="Congratulations to our winner:" img-src="./winner_images/winner_0.jpg" img-alt="Pampered Chef product image" img-top tag="article" style="max-width: 20rem;" class="mb-2">
            <p class="win">
              {{ winnerName }}!
            </p>
            <b-button @click="winnerName = ''">Close</b-button>
          </b-card>
        </div>

        <b-list-group class="top_space">
          <b-list-group-item variant="secondary" class="d-flex justify-content-between align-contestEntries-center">
            <span class="sm_label">Contender</span>
            <span class="sm_label">Number of Entries</span>
          </b-list-group-item>
          <b-list-group-item class="d-flex justify-content-between align-contestEntries-center" v-for="(contender, i) in contenders" v-bind:key="contender.id">
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
  name: "Spin-and-Win",
  data() {
    return {
      contender: "",
      contenders: [],
      chances: 1,
      winnerName: "",
      errorMsg: "",
    };
  },
  mounted() {
    this.$refs.name.focus();
  },
  computed: {
    isDisabled: function () {
      return this.contenders.length < 2;
    },
  },
  methods: {
    add() {
      const name = this.contender
        .replace(/\b\w/g, (c) => c.toUpperCase())
        .trim();

      if (this.isInputValid(name) !== true) return;

      this.contenders.unshift({
        text: name,
        chances: +this.chances,
      });

      this.contender = "";
      this.chances = 1;
      this.$refs.name.focus();
    },
    deleteContender(i) {
      this.contenders.splice(i, 1);
    },
    isInputValid(name) {
      let msg = "";
      let isValid = true;
      if (this.contenders.some((elem) => elem.text === name)) {
        msg +=
          "A contestant with that name has already been entered. Please enter a different name. ";
        isValid = false;
      }
      if (name.length === 0) {
        msg += "Name cannot be blank. ";
        isValid = false;
      }
      if (this.chances < 1) {
        msg += "Number of entries must be a number greater than 0. ";
        isValid = false;
      }
      this.errorMsg = msg;
      return isValid;
    },
    spin() {
      if (this.contenders.length < 2) {
        this.errorMsg =
          "You must add contenders before a winner can be selected.";
        return;
      }

      let contestEntries = [];

      this.contenders.forEach((contender, index) => {
        for (let count = 0; count < contender.chances; count++) {
          contestEntries.push(index);
        }
      });

      const winnerIndex =
        contestEntries[
          Math.floor(Math.random() * Math.floor(contestEntries.length))
        ];
      const winner = this.contenders[winnerIndex];
      this.winnerName = winner.text;

      // Reduce constant's chances by 1 or remove contestant
      if (this.contenders[winnerIndex].chances > 1) {
        this.contenders[winnerIndex].chances -= 1;
      } else {
        this.deleteContender(winnerIndex);
      }
    },
  },
};
</script>

<style scoped>
.top_space {
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
.sm_label {
  font-size: 0.9em;
  font-weight: 700;
  font-style: italic;
}

.disabled_button {
  color: #333 !important;
  background-color: #999 !important;
  border-color: #666 !important;
}

.win{
  font-size: 2rem;
  font-weight: bold;
  color:red;
}

</style>
