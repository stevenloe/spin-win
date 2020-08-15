<template>
  <div>
    <div class="heading" v-show="false">
      <b-row align-v="center">
        <b-col cols="2" class="d-none d-xl-block">
          <b-img src="./images/me.jpg" width="150" thumbnail fluid alt="Shelby"></b-img>
        </b-col>
        <b-col cols="8" class="heading-text">Shelby Loe's Kitchen Fun with Pampered Chef</b-col>
        <b-col cols="2" class="right d-none d-xl-block">
          <b-img src="./images/profile-teal.png" width="150" thumbnail fluid alt="Shelby"></b-img>
        </b-col>
      </b-row>
    </div>

    <b-card v-show="showForm" class="text-center bg-gray" title="Edit labels for this drawing:">
      <b-card-body class="heading-form">
        <b-row>
          <b-col cols="2"><input v-model="form.labelLeft" placeholder="Enter Left Label" size="15"></b-col>
          <b-col cols="4"><input v-model="form.textLeft" placeholder="Enter Left Text" size="30"></b-col>
          <b-col cols="2"><input v-model="form.labelRight" placeholder="Enter Right Label" size="15"></b-col>
          <b-col cols="4"><input v-model="form.textRight" placeholder="Enter Right Text" size="30"></b-col>
        </b-row>

        <b-row class="right mt-3 mr-1">
          <b-col align-v="center">
            <b-button pill variant="success" size="sm" @click="showForm=false">Close</b-button>
          </b-col>
        </b-row>
      </b-card-body>
    </b-card>

    <b-row @click="showForm=true" class="the-details">
      <b-col style="padding-left:0 !important; margin-left:0 !important">
        <span class="label">{{ form.labelLeft }}</span>
        <span class="msg"> {{ form.textLeft }}</span>
      </b-col>
      <b-col class="right" style="padding-right:0 !important;">
        <span class="label">{{ form.labelRight }}</span>
        <span class="msg">{{ form.textRight }}</span>
      </b-col>
    </b-row>

    <div class="small-link"><span @click="showForm=!showForm">edit labels</span></div>

    <b-alert show variant="danger" v-show="appError.length">{{ appError }}</b-alert>
  </div>
</template>

<script>
export default {
  name: "heading",
  props: {
    appError: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      form: {
        labelLeft: "Party:",
        textLeft: "Swanky's Spatula Fest!",
        labelRight: "Drawing:",
        textRight: "Winner winner chicken dinner",
      },
      showForm: false,
    };
  },
};
</script>

<style scoped>
.heading {
  font-family: "Filosophia", Georgia, serif;
  font-size: 2rem;
}

.heading-form {
  font-family: "MuseoSansRegular", Avenir, Helvetica, Arial, sans-serif;
  font-size: 1rem !important;
}

.heading-text {
  font-size: 3.4rem;
  text-align: center;
  color: #777;
}

.right {
  text-align: right;
}

.the-details {
  font-family: "Antonio", Impact, Haettenschweiler, "Arial Narrow Bold",
    sans-serif;
  margin: 1em 0 0 0;
  padding-left: 0;
  font-size: 2rem;
  cursor: pointer;
}

.label {
  font-family: "Antonio", Impact, Haettenschweiler, "Arial Narrow Bold",
    sans-serif;
  font-weight: 600;
  color: #777;
  padding-right: 0.5rem;
}

.msg {
  color: #333;
}

.bg-gray {
  background-color: #efefef;
}
</style>
