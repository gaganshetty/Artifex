<template>

  <div class="container">
    <div class="row">
      <div class="col-md-6 signup-box">
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <h4>Register here!🖋 to obtain complete access to the Site</h4>
          <br>
          <b-form-group
            id="input-group-1"
            label-for="input-1"
          >
            <b-form-input
              id="input-1"
              v-model="form.username"
              type="text"
              required
              placeholder="Enter Username"
            ></b-form-input>
          </b-form-group>
          <form class="form-name">
          <div class="row">
            <div class="col">
              <input type="text" class="form-control" placeholder="First name" v-model="form.firstName">
            </div>
            <div class="col">
              <input type="text" class="form-control" placeholder="Last name" v-model="form.lastName">
            </div>
          </div>
          </form>

          <b-form-group
            id="input-group-4"
            label-for="input-4"
          >
            <b-form-input
              id="input-4"
              v-model="form.email"
              type="email"
              required
              placeholder="Enter email"
            ></b-form-input>
          </b-form-group>
          <b-form-group
            id="input-group-5"
            label-for="input-5"
          >
            <b-form-input
              id="input-5"
              v-model="form.password"
              type="password"
              required
              placeholder="Enter Password"
            ></b-form-input>
          </b-form-group>
          <b-button type="submit" variant="primary">Submit</b-button>

          <h5 style="text-align:center">If you have already Registered, Please <router-link :to="'/login/'">login Here!</router-link></h5>
        </b-form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

var host = 'http://127.0.0.1:8000'
export default {
  data () {
    return {
      form: {
        username: '',
        firstName: '',
        lastName: '',
        email: '',
        password: ''
      },
      show: true
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()
      var data = {}
      data['username'] = this.form.username
      data['first_name'] = this.form.firstName
      data['last_name'] = this.form.lastName
      data['email_id'] = this.form.email
      data['password'] = this.form.password
      console.log(this.form)
      axios.post(host + '/signup/', data)
        .then(response => console.log(response))
        .catch(error => console.log(error))
      this.$router.push('/login/')
    },
    onReset (evt) {
      evt.preventDefault()
      this.form.email = ''
      this.form.username = ''
      this.form.firstName = ''
      this.form.lastName = ''
    }
  }
}
</script>

<style>
.signup-box{
  position: relative;
  margin-right:auto;
  top:50px;
  left:250px;
}

.form-name{
  margin-bottom: 10px;
}
</style>
