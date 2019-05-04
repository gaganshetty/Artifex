<template>
     <div class="container">
    <div class="row">
      <div class="col-md-6 signup-box">
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <h4>Add Discovery</h4>
          <br>
          <b-form-group
            id="input-group-1"
            label-for="input-1"
          >
            <b-form-input
              id="input-1"
              v-model="form.url"
              type="text"
              required
              placeholder="Add video URL"
            ></b-form-input>
          </b-form-group>
          <b-form-group
            id="input-group-2"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="form.title"
              type="text"
              required
              placeholder="Enter title of the discover"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-5"
            label-for="input-5"
          >
            <b-form-textarea
              id="input-5"
              v-model="form.description"
              type="text"
              required
              placeholder="Enter description of the Post"
            ></b-form-textarea>
          </b-form-group>
          <b-form-group
            id="input-group-2"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="form.channel_name"
              type="text"
              required
              placeholder="Enter name of the channel"
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
// var host = "http://10.0.0.153:8000"

export default {
  data () {
    return {
      form: {
        url: '',
        title: '',
        description: '',
        channel_name: '',
        categories: [1]
      },
      show: true
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()
      axios.post(host + '/discovery/', this.form, { headers: { 'Authorization': `Token ${window.localStorage['token']}` } }).then(
        response => {
          console.log(response.data)
          window.location = '/'
        }
      )
    },
    onReset (evt) {
      evt.preventDefault()
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
