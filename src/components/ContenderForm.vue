<template>
  <div>
    <b-alert show variant="danger" v-show="errorMsg.length">{{ errorMsg }}</b-alert>

    <b-input-group prepend="New Contender:" class="mb-2" @keyup.enter="validateContestant()">
      <b-form-input v-model="contender" ref="name" placeholder="Name" @focus="errorMsg = ''"></b-form-input>
      <b-form-input type="number" placeholder="chances" v-model="chances" @focus="errorMsg = ''"></b-form-input>

      <b-input-group-append>
        <b-button variant="primary" v-on:click="validateContestant()">
          Add
        </b-button>
      </b-input-group-append>

    </b-input-group>
  </div>
</template>

<script>
export default {
  name: "contender-form",
  data() {
    return {
      contender: "",
      chances: 1,
      errorMsg: "",
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
        msg += "Name cannot be blank. ";
        isValid = false;
      }
      if (this.chances < 1) {
        msg += "Number of entries must be a number greater than 0. ";
        isValid = false;
      }
      this.errorMsg = this.msg;
      return isValid;
    },
  },
};
</script>

