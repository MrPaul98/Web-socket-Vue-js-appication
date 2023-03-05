<template>
  <div id="log-sign-page" class="box">
  <div id="login-page" class="log">
    <h3 style="float: right;padding-right: 50%;"><b>Login</b></h3><br>
    <div style="padding-top: 50px; margin: 10px 10px 10px 10px;">
    <label><b>Username</b></label>
    <input type="text" placeholder="Enter Username" v-model="log.username"  required>

    <label ><b>Password</b></label>
    <input type="password" placeholder="Enter Password" v-model="log.password"  required>
        
    <button type="submit" @click="login">Login</button>
    </div>
  </div>
  <div id="sign-page" class="sign">
    <h3 style="float: right;padding-right: 50%;"><b>Sign-in</b></h3><br>
    <div style="padding-top: 50px; margin: 10px 10px 10px 10px;">
    <label><b>Username</b></label>
    <input type="text" placeholder="Enter Username" v-model="sign.username" required>

    <label ><b>Password</b></label>
    <input type="password" placeholder="Enter Password" v-model="sign.password" required>
        
    <button type="submit" @click="signUp">Sign Up</button>
    </div>
  </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'LoginPage',
  data() {
    return {
      sign: {
        username: '',
        password: ''
      },
      log: {
        username: '',
        password: ''
      }
    };
  },
  methods: {
    signUp() {
      axios.post(`http://localhost:9090/createUser`, this.sign)
    .then(response => {
      console.log(response);
      this.sign.username = '';
      this.sign.password = '';
    })
    .catch(e => {
      console.log(e);
    });
    },
    login() {
      axios.post(`http://localhost:9090/login`, this.log)
    .then(response => {
      console.log(response);
      localStorage.setItem("username", this.log.username);
      this.log.username = '';
      this.log.password = '';
      this.$emit("changeView");
    })
    .catch(e => {
      console.log(e);
    });
    }

  }
}
</script>

<style>
input[type=text], input[type=password] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  box-sizing: border-box;
  border-radius: 5px;
}
.log{
  height: 50vh;
  width: 50%;
  float: right;
  border-radius: 0 20px 20px 0;
  background-color: aqua;
}
.sign {
  height: 50vh;
  width: 50%;
  float: left;
  border-radius: 20px 0 0 20px;
  background-color: aqua;
}
.box {
  border: solid;
  height: 50vh;
  width: 50%;
  border-radius: 20px;
  float: right;
  margin-right: 25%;
  margin-top: 10%;
}
</style>