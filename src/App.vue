<template>
  <div id="app">
    <b-container>

      <heading :app-error="appErrorMsg"></heading>

      <b-container class="subtle-border">

        <spin-win 
          v-on:spin="spin"
          :contenders="contenders" 
          :is-disabled="isDisabled" 
          ref="spinWin"
          >
        </spin-win>
        
        <contender-form 
          
          v-on:show-hide="showHideContenders($event)"
          v-on:addContestant="add($event)" 
          v-on:errors="errors($event)"
          :show-contenders="showContenders"
          >
        </contender-form>

        <contender-list 
          v-on:delete-contender="deleteContender($event)"
          :show-contenders="showContenders"
          :contenders="contenders" 
          >
        </contender-list>

      </b-container>

    </b-container>
  </div>
</template>

<script>
import SpinWin from "./components/SpinWin";
import Heading from "./components/Heading";
import ContenderList from "./components/ContenderList";
import ContenderForm from "./components/ContenderForm";


export default {
  name: "App",
  components: {
    SpinWin,
    Heading,
    ContenderList,
    ContenderForm,
  },
  data() {
    return {
      contender: "",
      contenders: [
        { text: "Fluffy McGuinnis", chances: 3 },
        { text: "Squawkalottapus", chances: 3 },
        { text: "Qwerty", chances: 3 },
        { text: "Tallulah", chances: 3 },
        { text: "Violet", chances: 3 },
        { text: "Phinneas", chances: 3 },
      ],
      winnerIndex: 0,
      appErrorMsg: '',
      showContenders: true
    };
  },
  computed: {
    isDisabled() {
      return this.contenders.length < 2;
    }
  },
  methods: {
    add(newContender) {
      if (this.contenders.some((elem) => elem.text.toLowerCase() === newContender.text.toLowerCase())) {
        this.errors("A contestant with that name has already been entered. Please enter a different name.")
      } else {
        this.contenders.unshift(newContender);
        this.appErrorMsg = "";
      }
    },
    deleteContender(i) {
      this.contenders.splice(i, 1);
    },
    spin() {
      if (this.contenders.length < 2) {
        this.appErrorMsg +=
          "You must add contenders before a winner can be selected.";
        return;
      }

      let contestEntries = [];

      this.contenders.forEach((contender, index) => {
        for (let count = 0; count < contender.chances; count++) {
          contestEntries.push(index);
        }
      });

      this.winnerIndex =
        contestEntries[
          Math.floor(Math.random() * Math.floor(contestEntries.length))
        ];
      const winner = this.contenders[this.winnerIndex];

      // Reduce constant's chances by 1 or remove contestant
      if (this.contenders[this.winnerIndex].chances > 1) {
        this.contenders[this.winnerIndex].chances -= 1;
      } else {
        this.deleteContender(this.winnerIndex);
      }
      //this.refs.spinWin.animateWinner(this.this.winnerIndex)

      this.$refs.spinWin.animateWinner(this.winnerIndex);
    },
    
    errors(msg) {
      this.appErrorMsg = msg;
    },
    showHideContenders(){
      this.showContenders = !this.showContenders;
    },
  },
};
</script>

<style>
#app {
  font-family: "MuseoSansRegular", Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 40px;
}

.subtle-border {
  border: solid 1px #ccc;
  border-radius: 0.5rem;
  padding: 0.8rem 0;
}

@font-face {
  font-family: "Antonio";
  src: local("Antonio"),
    url(../public/fonts/antonio/Antonio-Regular.ttf) format("truetype");
}
@font-face {
  font-family: "Filosophia";
  src: local("Filosophia"),
    url(../public/fonts/filosophia/FilosofiaRegular.ttf) format("truetype");
}
@font-face {
  font-family: "MuseoSansRegular";
  src: local("MuseoSansRegular"),
    url(../public/fonts/museo_sans/MuseoSans_500.otf) format("opentype");
}

.small-link {
  text-align:right;
  font-size: .8rem;
  color: #999;
  text-decoration: underline;
  margin-bottom: .5rem;
  cursor: pointer;
}
</style>



