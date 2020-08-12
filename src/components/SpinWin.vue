<template>
<b-container>

    <b-row align-v="center">
      <b-col  cols="11" class="heading">
        Spin to Win!
      </b-col>
      <b-col cols="1" class="right" style="background-color:pink">
        <b-button variant="success" v-bind:class="{ disabled_button: isDisabled }" :disabled="isDisabled" size="lg" @click="$emit('spin')">Spin!</b-button>
      </b-col>
    </b-row>

    <!-- winner card -->
    <div class="top_space" v-show="winnerName.length && errorMsg.length === 0">
      <b-card title="Congratulations to our winner:" img-src="./winner_images/winner_0.jpg" img-alt="Pampered Chef product image" img-top tag="article" style="max-width: 20rem;" class="mb-2">
        <p class="win">
          {{ winnerName }}!
        </p>
        <b-button @click="winnerName = ''">Close</b-button>
      </b-card>
      </div>
  </b-container>

</template>

<script>
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
    }
  },
  data() {
    return {
      errorMsg: "",
    };
  }
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
  padding-right: 5rem;
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
