<template>
  <div>
  <b-navbar id="navbar" toggleable="sm" type="dark" variant="info">
    <b-navbar-brand href="/">
      <img id="logo" src="@/assets/images/logo.png">                                                                                                               
    </b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav id="links">
        <router-link class="navLink" v-if="currentUser" to="/dashboard" tag="b-nav-item">Dashboard</router-link>
        <!-- <b-nav-item href="#" disabled>Disabled</b-nav-item> -->
        <!-- <router-link v-if="currentUser" v-bind:to="{name : 'addbook'}" tag="b-nav-item">Add Book</router-link> -->
        <!-- <b-nav-item href="#" disabled>Disabled</b-nav-item> -->
        <router-link class="navLink" v-if="currentUser" v-bind:to="{name : 'bookshelf'}" tag="b-nav-item">BookShelf</router-link>
        <!-- <b-nav-item href="#" disabled>Disabled</b-nav-item>
        <router-link v-if="currentUser" v-bind:to="{name : 'addactivity'}" tag="b-nav-item">Add Activity</router-link> -->
        <!-- <b-nav-item href="#" disabled>Disabled</b-nav-item> -->
        <router-link class="navLink" v-if="currentUser" v-bind:to="{name : 'activitylog'}" tag="b-nav-item">Activity Log</router-link>
        <!-- <b-nav-item href="#" disabled>Disabled</b-nav-item> -->
        <router-link class="navLink" v-if="currentUser != null && currentUser.rol === 'parent'" v-bind:to="{name : 'childactivity'}" tag="b-nav-item">Child Activity</router-link>
        <router-link class="navLink" v-if="currentUser" v-bind:to="{name : 'prizes'}" tag="b-nav-item">Prizes</router-link>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <router-link v-if="currentUser && currentUser.rol === 'parent'" v-bind:to="{name : 'accounts'}" tag="b-nav-item" id="manage">Manage Accounts  </router-link>
        
        <em id="hello" v-if="currentUser">Hello, {{currentUser.sub}}!</em>
        <router-link v-if="!currentUser" to="/login" tag="b-nav-item"> login</router-link>
        <a id="sign-out" v-if="currentUser" @click="logout" href="#" tag="b-nav-item"> Sign Out</a>
          
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
</template>

<script>
import auth from '@/auth.js';
export default {
    name: 'dashboard',
    props: {
      currentUser: Object
    },
    data() {
        return {
        }
    },
    methods: {
        logout() {
        auth.logout();
        this.$emit('update-user')
        this.$router.push('/login');
        }
    }
}
</script>

<style scoped>
  #navbar {
    margin-bottom: 2%;
  }

#links{
  display: flex;
  justify-content: space-between;
}

.navLink{
  width: 100%;
}
li.nav-item {
  width: 120px;
}
#manage {
  width: 200px;
}
  #logo{
    width: 40%;
    height: auto;
  }
#sign-out {
  width: 100px;
  margin-top: 2%;
}
#hello {
  width: 120px;
  margin-top: 2%
}
</style>