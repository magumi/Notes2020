<template>
  <div class="login-card">
    <img class="logo" src="/assets/logo.svg">
    <input type="text" name="username" placeholder="Username" v-model="input.username">
    <input type="password" name="password" v-model="input.password" placeholder="Password">
    <div class="buttons">
      <button class="button buton-login" v-on:click="login()">LOGIN</button>
      <button class="button buton-register" v-on:click="goToRegister()">REGISTER</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "LogIn",
  data() {
    return {
      input: {
        username: "",
        password: ""
      },
      NOTES_AUTH: null,
      loginInProgress: false
    };
  },
  methods: {
    login() {
      if (this.loginInProgress) {
        return;
      }
      this.loginInProgress = true;
      const that = this;
      if (this.input.username !== "" && this.input.password !== "") {
        const data = {
          username: this.input.username,
          password: this.input.password
        };
        // username: super
        // password: puper
        axios
          .post("https://notes-api.girlsgoit.org/users/login/", data)
          .then(function(response) {
            if (response.status === 200) {
              localStorage.setItem("NOTES_AUTH", response.data.token);

              axios
                .get("https://notes-api.girlsgoit.org/users/me/")
                .then(function(response) {
                  console.log(response);
                  let user = response.data;
                  localStorage.setItem("USER_NAME", user.username);
                  localStorage.setItem("USER_ID", user.id);
                  localStorage.setItem("FULL_NAME", user.full_name);

                  that.$parent.$emit("auth");
                  that.$router.push({ path: "/dashboard" });
                });
            }

            console.log(response);
          })
          .catch(function(error) {
            alert("Invalid username or password.");
            console.log(error.response);
            that.loginInProgress = false;
          });
      } else {
        alert("A username and password must be present.");
        that.loginInProgress = false;
      }
    },
    goToRegister() {
      this.$router.push({ path: "/register" });
    }
  }
};
</script>

<style scoped>
body {
  background-color: #f2f2f2;
}

.login-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-items: center;
  width: 352px;
  height: 338px;
  margin: auto;
  margin-top: 150px;

  background-color: #ffffff;
  box-shadow: 0px 2px 27px rgba(230, 230, 230, 0.501961);
  border-radius: 10px;
  margin-bottom: 50px;
}

img {
  width: 132px;
  height: 52px;
  justify-items: center;
  margin-top: 36px;
  margin-bottom: 19px;
}

input {
  background: #ffffff;
  border: 1px solid #dfdfdf;
  box-sizing: border-box;
  border-radius: 4px;
  margin-bottom: 19px;
  height: 54px;
  width: 270px;
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  padding-left: 10px;
}

.buttons .button {
  display: inline-block;
  text-decoration: none;
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  margin-top: 14px;
  border: none;
  cursor: pointer;
  padding: 7px 10px;
  min-width: 130px;
}

.buttons .button:hover {
  text-decoration: underline;
}

.buton-login {
  font-size: 16px;
  line-height: 28px;
  color: white;
  background-color: #2ea1f8;
  border-radius: 5px;
  margin-right: 10px;
}

.buton-register {
  font-size: 16px;
  line-height: 28px;
  color: #2ea1f8;
  background-color: transparent;
}
</style>
