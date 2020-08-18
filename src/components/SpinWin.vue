<template>
  <b-container>

    <b-row align-v="center">
      <b-col cols="10" class="heading pl-0">
        Spin to Win!
      </b-col>
      <b-col cols="2" class="text-right pr-0">
        <b-button id="button-spin" variant="success" v-bind:class="{ disabled_button: isDisabled }" :disabled="isDisabled" size="lg" @click="spinClick">Spin!
        </b-button>
      </b-col>
    </b-row>

    <b-row>
      <b-col>-spin-</b-col>
      <div class="spin-wrapper">
        <ul class="list">
          <li v-for="(name, index) in names" :key="index" :id="`name-${index}`">{{ name }} : {{ index }}</li>
        </ul>
      </div>

    </b-row>

    <!-- winner card -->
    <div class="top_space" v-show="winnerName.length">
      <b-card title="Congratulations to our winner:" img-src="./winner_images/winner_0.jpg" img-alt="Pampered Chef winner image" img-top tag="article" style="max-width: 20rem;" class="mb-2">
        <p class="win">
          {{ winnerName }}!
        </p>
        <b-button @click="winnerName = ''">Close</b-button>
      </b-card>
    </div>
  </b-container>

</template>

<script>
import gsap from "gsap";

export default {
  name: "Spin-and-Win",
  props: {
    contenders: {
      type: Array,
      required: true,
    },
    isDisabled: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      winnerName: "",
      targetElement: null,
    };
  },
  // TODO: Names array needs to be recreated each time spin() is run. 
  // Reason: each time we run spin(), the contender's chances are decremented.
  // If we don't create the list each time spin is run, we'll eventually end up 
  // with list items here being the targets for contestants that are no longer in the contest
  computed: {
    names() {
      let list = [];
      const tempNames = this.contenders.map((contender) => contender.text);

      for (let i = 0; i < 16; i++) {
        tempNames.forEach((element) => {
          list.push(element);
        });
      }
      return list;
    },
  },
  methods: {
    animateWinner(winnerIndex) {
      if (this.targetElement) {
        this.targetElement.classList.remove("listWinner");
      }

      let  targetId = "#name-" + (this.contenders.length * 10 + winnerIndex);
      this.targetElement = document.querySelector(targetId);
      
      const list = document.querySelector(".list");
       let distance = -Math.abs(
          list.getBoundingClientRect().top - this.targetElement.getBoundingClientRect().top 
        );

        // Reset list position
        gsap.to(list, {
          duration: .5,
          ease: "elastic.out(1, .1)",
          y: 0,
        });
   
        // animate list scroll
        gsap.to(list, {
          delay: .5,
          duration: 12,
          ease: "elastic.out(1, .1)",
          y: distance,
          onComplete: this.highlightWinner,
        });
    },
    highlightWinner() {
      this.targetElement.classList.add("listWinner");
    },
    spinClick() {
      this.$emit("spin");
      gsap.from("#button-spin", { scale: 1.5, duration: 0.5, ease: "back" });
    },
  },
};
</script>

<style scoped>
/* -- animated spinner  styles-- */
.spin-wrapper {
  height: 180px;
  width: 400px;
  border: solid 3px red;
  overflow: hidden;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  border: solid 2px #ccc;
  padding: 8px;
  height: 40px;
  text-align: center;
}

.listWinner {
  background-color: #eef;
  transform: scale(1.1);
}

/* -- Winner Card styles-- */
.top_space {
  margin-top: 3rem;
}

.heading {
  font-weight: 700;
  font-size: 3rem;
}
.gray {
  color: #666;
  font-size: 1rem;
}

.disabled_button {
  color: #333 !important;
  background-color: #999 !important;
  border-color: #666 !important;
}

.win {
  font-size: 2rem;
  font-weight: bold;
  color: red;
}
</style>
