<template>
<div class="store">
  <h1>Stored Passwords</h1>
  <p v-show="this.$root.$data.websites.length === 0">Visit the 'Create Password' page to begin storing website information!</p>
  <div class="websiteInfo" v-for="website in websites" :key="website.id">
    <h2>{{website.url}}</h2>
    <div class="line"></div>
    <p>Username: {{website.username}}</p>
    <p>Password: {{website.password}}</p>
    <button @click="deleteWebsite(website)">Delete Website</button>
  </div>
</div>
</template>

<style>
.store {
  justify-content: center;
  display: flex;
  flex-wrap: wrap;
}

.websiteInfo {
  margin: 0 auto;
  border: 2px solid;
  border-radius: 20px;
  width: 100%;
  padding-bottom: 5px;
  margin-bottom: 20px;
}

.websiteInfo p {
  text-align: left;
  padding-left: 20px;
}

.line {
  width: 100%;
  height: 2px;
  background-color: black;
}

@media only screen and (min-width: 961px) {
  .websiteInfo {
    width: 30%;
  }
}
</style>

<script>
export default {
  name: 'StoreView',
  computed: {
    websites() {
      return this.$root.$data.websites;
    }
  },
  methods: {
    deleteWebsite(website) {
      this.$root.$data.websites.splice(this.$root.$data.websites.indexOf(website), 1);
    }
  },
}
</script>
