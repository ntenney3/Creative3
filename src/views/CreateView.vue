<template>
<div class="create">
  <h1>Enter Website information</h1>
  <h3>Step 1: Enter Website URL and Username</h3>
  <input v-model="url" placeholder="Website URL">
  <div class='divider'></div>
  <input v-model="username" placeholder="Username">
  <h3>Step 2: Choose Options to Generate a Random Password</h3>
  <div class='settings'>
    <div class='option'>
      <input type="checkbox" v-model='passwordOptions.numbers'>
      <p>Include Numbers </p>
    </div>
    <div class='option'>
      <input type="checkbox" v-model='passwordOptions.specialCharacters'>
      <p>Include Special Characters </p>
    </div>
    <div class='option'>
      <input type="checkbox" v-model='passwordOptions.uppercaseLetters'>
      <p>Include Uppercase Letters </p>
    </div>
    <div class='option'>
      <input id="passwordLengthInput" v-model="passwordOptions.passwordLength" placeholder="12">
      <p id="passwordLengthDescription">Enter Password Length (default 12) </p>
    </div>
    <button @click="generatePassword(passwordOptions)">Get Password!</button>
  </div>
  <h3>Step 3: Your Generated Password is Below. You may change it as desired.</h3>
  <input v-model="password" placeholder='Password'>
  <h3>Step 4: If you are ready, you may store your website information</h3>
  <button @click="storePassword(url, username, password)">Store Website Data</button>
</div>
</template>

<style>
.create {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

h1,
h3 {
  width: 100%;
}

.option {
  text-align: left;
  margin: auto;
}

.option input {
  float: left;
  margin-left: 20px;
  margin-top: 5px;
}

.option p {
  padding-left: 65px;
  width: 300px;
}

#passwordLengthInput {
  width: 25px;
  margin-top: 0;
}

.divider {
  width: 100%;
  padding: 5px;
}

@media only screen and (min-width: 961px) {
  .option input {
    margin-left: 0;
  }
  .option p {
    padding-left: 50px;
  }
}
</style>

<script>
export default {
  name: 'CreateView',
  data() {
    return {
      url: "",
      username: "",
      password: "",
      passwordOptions: {
        numbers: false,
        specialCharacters: false,
        uppercaseLetters: false,
        passwordLength: ""
      }
    }
  },
  methods: {
    generatePassword(options) {
      let url = "https://passwordinator.herokuapp.com/generate";
      let urlQueries = "";
      if (options.numbers) {
        urlQueries += '?num=true';
      }
      if (options.specialCharacters) {
        if (urlQueries.length > 0) {
          urlQueries += '&'
        } else {
          urlQueries += '?'
        }
        urlQueries += 'char=true';
      }
      if (options.uppercaseLetters) {
        if (urlQueries.length > 0) {
          urlQueries += '&'
        } else {
          urlQueries += '?'
        }
        urlQueries += 'caps=true';
      }
      if (!isNaN(options.passwordLength) && options.passwordLength.length > 0) {
        if (urlQueries.length > 0) {
          urlQueries += '&'
        } else {
          urlQueries += '?'
        }
        urlQueries += 'len=' + options.passwordLength;
      }
      url += urlQueries;

      let self = this;
      fetch(url)
        .then(function(response) {
          return response.json();
        }).then(function(json) {
          self.password = json.data;
        });
    },
    storePassword(url, username, password) {
      this.$root.$data.websites.push({
        "url": url,
        "username": username,
        "password": password
      });
      this.url = "";
      this.username = "";
      this.password = "";
      this.passwordOptions.numbers = false;
      this.passwordOptions.specialCharacters = false;
      this.passwordOptions.uppercaseLetters = false;
      this.passwordOptions.passwordLength = "";
    }
  }
}
</script>
