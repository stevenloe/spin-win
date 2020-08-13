<template>
  <div id="app">
    <b-container>
      <heading></heading>
      <b-container class="subtle-border">
        <spin-win :is-disabled="isDisabled" :winner-name="winnerName" v-on:spin="spin"></spin-win>
        <b-alert show variant="danger" v-show="appErrorMsg.length">{{ appErrorMsg }}</b-alert>
        <contender-form v-on:addContestant="add($event)"></contender-form>
        <contender-list :contenders="contenders" v-on:delete-contender="deleteContender($event)"></contender-list>
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
      ],
      winnerName: "",
      appErrorMsg: "",
    };
  },
  computed: {
    isDisabled() {
      return this.contenders.length < 2;
    },
  },
  methods: {
    add(newContender) {
      if (
        this.contenders.some(
          (elem) => elem.text.toLowerCase() === newContender.text.toLowerCase()
        )
      ) {
        this.appErrorMsg +=
          "A contestant with that name has already been entered. Please enter a different name. ";
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
    errors(msg) {
      this.appErrorMsg = msg;
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
</style>



