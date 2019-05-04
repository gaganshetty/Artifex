<template>
  <div class="home">
    <div class="conatiner">
      <div class="row">
        <div class="col-md-8">
          <div>
            <div  v-for='post in discover' v-bind:key='post.id'>
              <a  href="#" class="card-link">
                <b-card class="card">
                  <b-card-img :src="post.thumbnail" class="rounded-0 card-img"></b-card-img>
                  <button class="btn btn-success upvote-btn"
                      v-if='post.user_vote[0].user_vote'
                      :id='"outerUpvotebtn-"+post.id'
                      @click="upvote(post.id)">🔻 Downvote {{post.votes}}</button>
                  <button class="btn btn-success upvote-btn"
                      v-else
                      :id='"outerUpvotebtn-"+post.id'
                      @click="upvote(post.id)">🔺 Upvote {{post.votes}}</button>
                  <p style="color:#3498db" v-b-modal="'modal-xl' + post.id">{{ post.title }}</p>
                  <p>{{ post.author.first_name}} {{ post.author.last_name}}</p>
                </b-card>
              </a>
              <br>
            <!-- Modal Component -->
                <b-modal :id="'modal-xl' + post.id" :title="post.title" size="xl">
                  <div>
                    <div class="row">
                    <div class="col-md-8 main-bar">
                      <b-media>
                        <b-img slot="aside" blank blank-color="#ccc" width="64" alt="placeholder" class="channel-avatar"></b-img>
                        <h5 class="mt-0"><b>{{post.channel_name}}</b></h5>
                      </b-media>
                      <br>
                      <iframe width="550" height="325" :src="post.embed" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                      <br>
                      <div>
                       <h5><b>{{post.author.first_name}} {{post.author.last_name}} </b></h5>
                       <p id="timestamp">{{post.timestamp}}</p>
                       <p id='description'>
                         {{post.description}}
                       </p>
                      </div>
                     </div>
                    <div class="col-md-4">
                      <button class="btn btn-success upvote-btn"
                      v-if='post.user_vote[0].user_vote'
                      :id='"innerUpvotebtn-"+post.id'
                      @click="upvote(post.id)">🔺 Upvote {{post.votes}}</button>
                      <button class="btn btn-success upvote-btn"
                      v-else
                      :id='"innerUpvotebtn-"+post.id'
                      @click="upvote(post.id)">🔻 Downvote {{post.votes}}</button>
                    </div>
                    </div>
                  </div>
                  <hr>
                  <div class="row comments-section">
                    <b-form @reset="onReset" @submit.prevent="onComment(post.id)" inline>
                      <b-form-textarea style="width:300px"
                      type="text"
                      placeholder="Add Comments"
                      v-model='form2.comment'
                      id="comments"></b-form-textarea>
                      <br>
                      <b-button style="margin-left:1rem"
                      type="submit"
                      variant="warning">Add Comment</b-button>
                    </b-form>
                    <div :id='"commentsDiv"+post.id'>
                       <!-- <p>{post.comment}</p> -->
                    </div>
                  </div>
                </b-modal>
              </div>
            </div>
        </div>
        <div class="col-md-3">
          <b-card-group deck>
            <b-card border-variant="primary"
              align="center"
            >
              <b-card-text class="subscribe">Subscribe to obtain weekly trending social discovers</b-card-text>
              <b-form action="" @reset="onReset" @submit.prevent="onSubmit" >
                <b-form-input type="email" placeholder="Add Email"
                v-model='form1.subscribe'
                id="subscribeForm"></b-form-input>
                <b-button variant="success" type="submit">Subscribe</b-button>
              </b-form>
            </b-card>
          </b-card-group>
        </div>
        </div>
        </div>
    </div>
</template>

<script>
// @ is an alias to /src
// import HomePage from '@/components/HomePage.vue'
import axios from 'axios'

var host = 'http://127.0.0.1:8000'
// var host = "http://10.0.0.153:8000"
export default {
  name: 'home',
  components: {
  },
  data () {
    return {
      discover: {},
      form1: {
        subscribe: ''
      },
      form2: {
        comment: ''
      }
    }
  },
  mounted () {
    if (window.localStorage['token']) {
      this.$store.state.is_not_authenticated = false
      axios.get(host + '/discovery/', { headers: { 'Authorization': `Token ${window.localStorage['token']}` } }).then(
        response => {
          this.discover = response.data
          console.log(this.discover)
        })
    } else {
      axios.get(host + '/discovery/').then(
        response => {
          this.discover = response.data
          console.log(this.discover)
        })
    }
    var token = `Token ${window.localStorage['token']}`
    axios.get(host + '/comments/', { headers: { 'Authorization': token } })
      .then(response =>  document.getElementById("commentsDiv-"+id).innerHTML = response)
  },
  methods: {
    onSubmit () {
      var data = {}
      data['email'] = this.form1.subscribe
      axios.post(host + '/subscribe', data)
        .then(response => console.log('success'))
    },
    onComment (id) {
      if (window.localStorage['token']) {
        var data = {}
        data['discovery'] = id
        data['comment'] = this.form2.comment
        axios.post('http://127.0.0.1:8000/comments/', data, { headers: { 'Authorization': `Token ${window.localStorage['token']}` } })
        alert('Comment has been posted successfully')
      }
    },
    onReset (evt) {
      evt.preventDefault()
      this.form.comment = ''
      this.form.subscribe = ''
    },
    upvote (id) {
      if (!window.localStorage['token']) {
        alert('You have not registered to Upvote, Please Sign Up')
        this.$router.push('/signup/')
      } else {
        for (let post of this.discover) {
          if (post.id === id) {
            var x = document.getElementById('outerUpvotebtn-' + post.id)
            var y = document.getElementById('innerUpvotebtn-' + post.id)
            var token = `Token ${window.localStorage['token']}`
            if (x.innerHTML === '🔺 Upvote ' + post.votes || y.innerHTML === '🔺 Upvote ' + post.votes) {
              post.votes++
              x.innerHTML = '🔻 Downvote ' + post.votes
              y.innerHTML = '🔻 Downvote ' + post.votes
              axios.get('http://127.0.0.1:8000/vote/' + id, { headers: { 'Authorization': token } })
                .then(res => console.log(res))
            } else {
              post.votes--
              x.innerHTML = '🔺 Upvote ' + post.votes
              y.innerHTML = '🔺 Upvote ' + post.votes
              axios.get('http://127.0.0.1:8000/vote/' + id, { headers: { 'Authorization': token } })
                .then(res => console.log(res))
            }
          }
        }
      }
    }
  }
}
</script>
<style>
.card-link{
  color:#2f5f5f;
}
.card-link:hover{
  color:#2f5f5f;
}
.col-md-8, .col-md-3{
  margin-top:20px;
}

.col-md-8{
  margin-left: 20px;
}
.rounded-0{
  width:100px;
  height: 75px;
}
.subscribe{
  margin-right: 10px;
   font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#subscribeForm{
  margin-bottom: 10px;
  border-radius: 10px;
}
.card-img{
  float: left;
  margin-right: 20px;
}
.card {
  box-shadow: 0 2px 6px 0 hsla(0, 0%, 0%, 0.2);
  transition: .005s;
}
.card:hover {
  transform: scale(1.01);
}

.card-link p {
  text-align: left;
}

.card-info {
  margin-left: 120px;
}

.upvote-btn {
  float: right;
  margin-right: 20px;
}

.upvote-btn:hover {
  background-color: #fff;
  color: #28a745;
  border-color: #28a745;
}
h5{
  padding:1rem;
  font-size: 80%;
  text-align: left;
  color:#2f5f5f;
}
.main-bar{
  margin-left: 0%;
}
.comments-section{
  margin:3rem;
}
.channel-avatar{
 border-radius: 50%;
}
#timestamp{
  font-size: 70%;
  text-align: left;
  margin:1.5rem;
  margin-top: 0;
}
#description{
  font-size: 90%;
  font-style: arial;
  text-align: left;
  margin-left:1.5rem;
  margin-top: -1rem;
}
</style>
