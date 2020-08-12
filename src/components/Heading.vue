<template>
  <div id="heading">
    <b-row align-v="center">
      <b-col cols="2" class="d-none d-xl-block">
        <b-img src="./images/me.jpg" width="150" thumbnail fluid alt="Shelby"></b-img>
      </b-col>
      <b-col cols="8" class="heading-text">Shelby Loe's Kitchen Fun with Pampered Chef</b-col>
      <b-col cols="2" class="right d-none d-xl-block">
        <b-img src="./images/profile-teal.png" width="150" thumbnail fluid alt="Shelby"></b-img>
      </b-col>
    </b-row>
    <b-row class="the-details">
      <b-col style="padding-left:0 !important">
        <span class="label"> Party: </span> 
        <span class="msg"> Lemon Tesla's Spring Spree</span> 
      </b-col>
      <b-col class="right" style="padding-right:0 !important">
        <span class="label"> Drawing: </span> 
        <span class="msg"> Scramble-O-Prizes!</span> 
      </b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  name: "heading",
};
</script>

<style scoped>
#heading {
  font-family: "Filosophia", Georgia, serif;
  font-size: 2em;
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
  margin: 1em 0;
}

.label {
  font-family: "Antonio", Impact, Haettenschweiler, "Arial Narrow Bold",
    sans-serif;
  font-weight: 600;
  color: #777;
}

.msg {
  color: #333;
  margin: 0 1em;
}
</style>
