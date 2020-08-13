<template>
  <div>
    <div class="small-link"><span @click="$emit('show-hide')">{{ showContenders ? 'Hide' : 'Show'}} Contenders</span></div>

    <div v-show="showContenders">
      <hr>
      <b-input-group prepend="New Contender:" class="mb-2" @keyup.enter="validateContestant()">
        <b-form-input v-model="contender" ref="name" placeholder="Name"></b-form-input>
        <b-form-input type="number" placeholder="chances" v-model="chances"></b-form-input>

        <b-input-group-append>
          <b-button v-on:click="validateContestant()">
            Add
          </b-button>
        </b-input-group-append>

      </b-input-group>

    </div>
  </div>
</template>

<script>
export default {
  name: "contender-form",
  props: {
    showContenders: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      contender: "",
      chances: 1,
    };
  },
  methods: {
    validateContestant() {
      const name = this.contender
        .replace(/\b\w/g, (c) => c.toUpperCase())
        .trim();

      if (this.isInputValid(name)) {
        this.$emit("addContestant", { text: name, chances: +this.chances });

        this.contender = "";
        this.chances = 1;
        this.$refs.name.focus();
      }
    },
    isInputValid(name) {
      let msg = "";
      let isValid = true;

      if (name.length === 0) {
        msg += "Please enter the contender's name. ";
        isValid = false;
      }
      if (this.chances < 1) {
        msg += "Number of entries must be a number greater than 0. ";
        isValid = false;
      }

      this.$emit("errors", msg);

      return isValid;
    },
  },
};
</script>

