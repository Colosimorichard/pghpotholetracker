<template>
  <div id="app">
    <div id="nav">
      <div id="branding">
      <img id="ppt-logo" src="./img/PPT-Logo-Crop.png" alt="Pittsburgh Pothole Tracker Logo">
      <h3 id="page-title">PITTSBURGH POTHOLE TRACKER</h3>
      </div>
      <div id="links">
      <router-link v-bind:to="{ name: 'home' }">Home</router-link>
      <span class="pipe">   |   </span>
      <router-link v-bind:to="{ name: 'map' }">Map</router-link>
      <span class="pipe" v-if="$store.state.token != ''">   |   </span>
      <router-link v-if="$store.state.token != ''" v-bind:to="{ name: 'addPothole' }">Report</router-link>
      <span class="pipe" v-if="$store.state.token != ''">   |   </span>
      <router-link v-bind:to="{ name: 'list' }" v-if="checkRole">List View</router-link>
      <span class="pipe" v-if="checkRole" >   |  </span>
      <router-link v-bind:to="{ name: 'logout' }" v-if="$store.state.token != ''">Logout</router-link>
      </div>
      <div id="role-checker">
        <span v-if="$store.state.token == ''">You are not currently logged in.</span>
        <span v-if="$store.state.token != ''">You are currently logged in as: {{ this.$store.state.user.authorities[0].name.substring(5) }}</span>
      </div>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  computed: {
    checkRole() {
      if (this.$store.state.token != '' && (this.$store.state.user.authorities[0].name == 'ROLE_EMPLOYEE' || this.$store.state.user.authorities[0].name == 'ROLE_ADMIN')) {
        return true;
      } else {
        return false;
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Work+Sans:wght@200&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Staatliches&display=swap');

#app {
  font-family: 'Work Sans', sans-serif;
  font-weight: bold;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 5px;
}
body {
  background-color: #ffffea;
}
#nav {
  display: flex;
  width: 98%;
  height: 50px;
  border-radius: 15px 0 0 15px;
  align-items: center;
  justify-content: space-around;
  background-color: #025252;
  color: white;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  font-size: 20px;
  font-weight: bold;
  padding: 0 20px;
  font-size: 25px;
}
#nav a.router-link-exact-active {
  color: #42b983;
}
#nav a {
  font-weight: bold;
  color: white;
}
.pipe {
  padding: 0 20px;
}
#page-title {
  font-weight: bolder;
  font-family: 'Staatliches', sans-serif;
  flex-basis: 100%;
  white-space: nowrap;
  padding-left: 20px;
}
#role-checker {
  flex-basis: 100%;
  white-space: nowrap;
}
#links {
  display: flex;
  justify-content: center;
  padding: 5px;
  white-space: nowrap;
  flex-basis: 100%;
}
#branding {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-basis: 100%;
}
#ppt-logo {
  width: auto;
  height: 50px;
}
@media only screen and (max-width: 1456px) {
  #nav {
    font-size: 20px;
    flex-wrap: wrap;
    height: 200px;
    align-content: flex-start;
    margin: auto;
  }
  #role-checker {
    font-size: 20px;
    padding-top: 10px;
  }
  #page-title {
    font-size: 35px;
    padding: 0;
  }
  #ppt-logo {
    height: 100px;
  }
  #branding {
    flex-basis:0%;
  }
}

@media only screen and (max-width: 500px) {
  #nav {
    height: 230px;
    overflow: hidden;
    padding: 5px 0 0 0;
  }
  #links {
    display: flex;
    justify-content: center;
    padding: 5px;
    white-space: nowrap;
    font-size: 4.5vw;
    flex-wrap: wrap;
  }
  #page-title {
    white-space: normal;
    margin: 0;
  }
  #role-checker {
    font-size: 4.5vw;
  }
}
</style>



