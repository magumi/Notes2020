<template>
  <header class="header-style">
    <div class="header-text">
      <div class="name">
        <span class="link">{{ user.fullName || user.username }}</span>
      </div>

      <div class="logo">
        <router-link to="/dashboard">
          <img src="/assets/logo.svg" alt="Logo">
        </router-link>
      </div>

      <div class="nav-links">
        <ul>
          <li>
            <router-link to="/settings" class="link">SETTINGS</router-link>
          </li>
          <li>
            <router-link to="/help" class="link">HELP</router-link>
          </li>
          <li>
            <button @click.prevent="signOut()" class="link">SIGN OUT</button>
          </li>
        </ul>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "HeaderLogIn",
  props: ["user"],
  methods: {
    signOut() {
      localStorage.clear();
      this.$parent.$emit("auth");
      this.$router.push({ path: "/" });
    }
  }
};
</script>

<style scoped>
.header-style {
  padding: 30px 0;
  background: #ffffff;
  box-shadow: 0 0 27px rgba(230, 230, 230, 0.5);
}

.header-text {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: Roboto, sans-serif;
  font-size: 16px;
  margin: 0 auto;
  padding: 0 30px;
  max-width: 960px;
  color: #393939;
  position: relative;
}

.link {
  text-decoration: none;
  color: #393939;
  text-transform: uppercase;
  border: none;
  background: transparent;
  cursor: pointer;
  display: block;
}

.nav-links ul {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  padding: 0;
  margin: 0;
  justify-content: center;
  align-items: center;
}

.nav-links .link {
  margin-left: 40px;
}

.nav-links ul li:first-child .link {
  margin-left: 0;
}

a:hover {
  color: #050505;
}

.logo {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  height: 100%;
  right: 50%;
  width: 0;

}

@media screen and (max-width: 900px) {
  .header-text {
    display: flex;
    flex-direction: column;
  }

  .logo {
    align-items: start;
  }

  .name {
    margin-top: 80px;
  }

  .link,
  .nav-links {
    width: 100%;
    justify-content: center;
  }

  .nav-links ul {
    justify-content: center;
  }

  .link {
    display: inline-block;
    margin-bottom: 20px;
  }
}

@media screen and (max-width: 400px) {
  .nav-links ul {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .nav-links .link {
    margin: 5px 0;
  }
}
</style>
