<template>
  <b-container>

    <b-row align-v="center">
      <b-col cols="10" class="heading pl-0">
        Spin to Win!
      </b-col>
      <b-col cols="2" class="text-right pr-0">
        <b-button variant="success" v-bind:class="{ disabled_button: isDisabled }" :disabled="isDisabled" size="lg" @click="$emit('spin')">Spin!
        </b-button>
      </b-col>
    </b-row>

    <b-row>
      <b-col>-spin-</b-col>
      <div class="spin-wrapper">
          <ul class="list">
            <li>Lorem.</li>
            <li>Quae!</li>
            <li>Numquam.</li>
            <li>Aut!</li>
            <li>Repellat?</li>
            <li>At.</li>
            <li>Commodi?</li>
            <li>Qui.</li>
            <li>Voluptate.</li>
            <li>Obcaecati!</li>
            <li>Animi.</li>
            <li>Quibusdam!</li>
            <li>Totam.</li>
            <li>Iste!</li>
            <li>Aspernatur?</li>
            <li>Placeat?</li>
            <li>Ratione!</li>
            <li>Delectus.</li>
            <li>Alias?</li>
            <li>Assumenda.</li>
            <li>Dolores.</li>
            <li>Sunt?</li>
            <li>Consectetur?</li>
            <li>Fuga.</li>
            <li>Sed.</li>
            <li>Consequatur?</li>
            <li>Consequatur.</li>
            <li>Voluptatem!</li>
            <li>Eos.</li>
            <li>Molestiae?</li>
            <li>Pariatur.</li>
            <li>Non?</li>
            <li>Voluptatum.</li>
            <li>Fugiat.</li>
            <li>Mollitia.</li>
            <li>Iusto.</li>
            <li>Minima.</li>
            <li>Vero.</li>
            <li>Perspiciatis!</li>
            <li>Unde!</li>
            <li>Spiffcus</li>
            <li>Swankicus!</li>
            <li>Swillicus!</li>
            <li>Obcaecati.</li>
            <li>Tempora!</li>
            <li>Tempora?</li>
            <li id='target-item'>Target Item</li>
            <li>Voluptatum.</li>
            <li>Facilis!</li>
            <li>Quasi?</li>
            <li>In.</li>
            <li>Autem.</li>
            <li>Sapiente.</li>
            <li>Placeat?</li>
            <li>Ratione!</li>
            <li>Delectus.</li>
            <li>Alias?</li>
            <li>Assumenda.</li>
            <li>Dolores.</li>
            <li>Sunt?</li>
            <li>Consectetur?</li>
            <li>Fuga.</li>
            <li>Sed.</li>
            <li>Consequatur?</li>
            <li>Consequatur.</li>
            <li>Voluptatem!</li>
            <li>Eos.</li>
            <li>Molestiae?</li>
            <li>Pariatur.</li>
            <li>Non?</li>
            <li>Voluptatum.</li>
            <li>Fugiat.</li>
            <li>Mollitia.</li>
            <li>Iusto.</li>
            <li>Minima.</li>
            <li>Vero.</li>
            <li>Perspiciatis!</li>
            <li>Unde!</li>
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
    isDisabled: {
      type: Boolean,
      required: true,
    },
    winnerName: {
      type: String,
      required: true,
      default: "",
    },
  },
  data() {
    return {};
  },
  mounted() {
    const distance = -Math.abs(document.querySelector('.list').getBoundingClientRect().top - document.querySelector('#target-item').offsetTop)
    gsap.to(".list", {
      duration: 7,
      ease: "elastic.out(1, .1)",
      y: distance,
      onComplete: this.showWinner,
    });
  },
  methods: {
    showWinner() {
      const elem = document.querySelector("#target-item");
      elem.classList.add("listWinner");
      gsap.to("#target-item", {
        duration: 0.2,
        rotation: 5,
        scaleX: 1.3,
        scaleY: 1.3,
      });
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
  background-color: pink;
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
