<template>
     <div class="container">
    <div class="row">
      <div class="col-md-6 signup-box">
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <h4>⚙ Login</h4>
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
        </b-form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

var host = 'http://127.0.0.1:8000'
// var host = 'http://10.0.0.153:8000'

export default {
  data () {
    return {
      form: {
        username: '',
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
      data['password'] = this.form.password
      console.log(data)
      axios.post(host + '/api-token-auth/', data)
        .then(response => {
          console.log(response.data)
          window.localStorage['token'] = response.data['token']
          this.$store.state.is_not_authenticated = false
          this.$router.push('/')
          console.log(this.$store.state.is_not_authenticated)
        }).catch(err => console.log(err)
        )
    },
    onReset (evt) {
      evt.preventDefault()
      this.form.username = ''
      this.form.password = ''
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
