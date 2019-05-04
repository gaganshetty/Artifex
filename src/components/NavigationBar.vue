<template>
    <div>
  <b-navbar toggleable="lg" type="dark" variant="info" class="mynav">
    <b-navbar-brand>
    <router-link to="/" component=HomePage class="alink">Social Discover</router-link>
    </b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item>
            <router-link to="/add" class="alink">Add Discovery</router-link>
        </b-nav-item>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <!-- r<b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        </b-nav-fom> -->

        <b-nav-item>
          <router-link v-if="$store.state.is_not_authenticated" to="/login" class="alink">Login</router-link>
        </b-nav-item>
        <b-nav-item>
          <router-link v-if="$store.state.is_not_authenticated" to="/signup" class="alink">Sign Up</router-link>
        </b-nav-item>
        <b-nav-item>
          <div v-if="! $store.state.is_not_authenticated" @click.prevent="logout" class="alink">Logout</div>
        </b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
</template>

<script>
import axios from 'axios'
export default {
  methods: {
    logout(evt) {
      window.localStorage.removeItem('token')
    this.$store.state.is_not_authenticated = true
  },
}
}
</script>

<style>
.bg-info {
    background-color: #ee5253 !important;
}
.alink{
  color:#ddd;
}
.alink:hover{
  color:#fff;
  text-decoration:none;
}
.alink:active{
  color:#fff;
}


</style>
