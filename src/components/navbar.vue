<template>
  <div id="app">
    <nav class="navbar navbar-expand navbar-dark ">
      <a href class="navbar-brand" @click.prevent>
          <img src="..\assets\kmerfreelance.png" alt="kmerFreeLance" class="kmerFreeLanceLogo">
      </a>
      <div class="navbar-nav mr-auto">
        <li class="nav-item">
          <!-- <router-link to="/home" class="nav-link">
            <font-awesome-icon icon="home" />Home
          </router-link> -->
        </li>
        <li v-if="showLanceBoard" class="nav-item">
          <router-link to="/lance" class="nav-link">DashBoard</router-link>
        </li>
        <li v-if="showEnterpriseBoard" class="nav-item">
          <router-link to="/enterprise" class="nav-link">DashBoard</router-link>
        </li>

      </div>

      <div v-if="!currentUser" class="navbar-nav ml-auto">
        <li class="nav-item">
          <router-link to="/register" class="nav-link">
                 <img src="..\assets\signup.png" alt="Create Account" class="signinSignup">
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/login" class="nav-link">
            <img src="..\assets\signin.png" alt="Login" class="signinSignup">
          </router-link>
        </li>
      </div>

      <div v-if="currentUser" class="navbar-nav ml-auto">
        <li class="nav-item">
          <router-link to="/profile" class="nav-link">
            <img src="..\assets\profil.png" alt="Login" class="signinSignup">
            {{ currentUser.username }}
          </router-link>
        </li>
        <li class="nav-item">
          <a class="nav-link" href @click.prevent="logOut">
            <img src="..\assets\logout.png" alt="Login" class="signinSignup">
          </a>
        </li>
      </div>
    </nav>

    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<script>
export default {
    name:'navbar',
  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    },
    showLanceBoard() {
      if (this.currentUser && this.currentUser.roles) {
        return this.currentUser.roles.includes('LANCE');
      }

      return false;
    },
    showEnterpriseBoard() {
      if (this.currentUser && this.currentUser.roles) {
        return this.currentUser.roles.includes('ENTERPRISE');
      }

      return false;
    }
  },
  methods: {
    logOut() {
      this.$store.dispatch('logout');
      this.$router.push('/login');
    }
  }
};
</script>
<style>
.signinSignup{
    height: 40px;
}
.kmerFreeLanceLogo{
    height: 50px;
}
.navbar{
 background-image: url("../assets/bg1.png");
 background-repeat: no-repeat;
 background-size: 100%;
 height: 100%;
}
</style>