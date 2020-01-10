<template>
  <div class="container-fluid mt-5" align="center">
    <div class="text-right button-twitter container" style="margin-right: 220px">
      <b-button id="show-btn" @click="showModal" style="background-color: #00acee; border: none;">CREATE TWEET</b-button>
    </div>
      <b-spinner label="Spinning" v-if="tweets.length == 0"></b-spinner>
    <div class="row">
      <div class="col-12 mt-5" align="center" style="position: relative;" v-for="tweet in tweets" :key="tweet.id_tweet">
          <div class="card w-75 shadow-sm">
            <div class="ribbon-wrapper">
                <div class="ribbon"></div>
            </div>
            <div class="card-body">
              <img alt="Tweet" src="../assets/twitter.png" class="top-logo" style="width: 100px !important; height: 100px !important;"/>
              <div class="row">
                <div class="col-md-4 col-sm-6">
                  <img alt="Tweet" src="../assets/defaultprofile.png" class="rounded img-fluid rounded-circle shadow-sm"/>
                </div>
                <div class="col-md-4 col-sm-12">
                  <span class="text-patrick text-muted">{{tweet.first_name}} {{tweet.last_name}}</span>
                  <br>
                  <span>{{tweet.tweet}}</span>
                  <br>
                  <span class="text-patrick text-muted">{{tweet.date | moment("d/M/YYYY - h:mm:ss a")}}</span>
                </div>
                <div class="col-md-4 col-sm-12 text-right align-items-end" style="margin-top: 50px;">
                  <span v-on:click="updateTweet(tweet.id_tweet, tweet.tweet)" @click="modalShow = !modalShow" id="show-btn2" style="cursor: pointer;">
                    <i class="fas fa-pencil-alt mr-3" style="color: #00acee;"></i>
                  </span> 
                  <span v-on:click="deleteTweet(tweet.id_tweet, tweet.tweet)" style="cursor: pointer;">
                    <i class="fas fa-trash-alt text-danger"></i>
                  </span>
                </div>
              </div>
            </div>
          </div>
      </div>
        <b-modal ref="my-modal2" hide-footer title="UPDATING TWEET" v-model="modalShow" :no-close-on-backdrop="true">
            <div class="form-group">
              <i class="fab fa-twitter" style="color: #00acee;"></i> <label>TWEET</label>
              <input type="text" class="form-control" placeholder="Insert your new Tweet" v-model="modify_tweet" label="Tweet" required autocomplete="off">
            </div>
            <div class="row justify-content-center">
              <button type="submit" class="btn col-10 text-light" style="background-color: #00acee;" v-on:click="sendEditContact">SEND</button>
            </div>
        </b-modal>

        <b-modal ref="my-modal" hide-footer title="CREATING A NEW TWEET">
            <div class="form-group">
              <i class="fab fa-twitter" style="color: #00acee;"></i> <label>TWEET</label>
              <input type="text" class="form-control" id="exampleInputPassword1" placeholder="Insert your new Tweet" v-model="tweet" :rules="tweetRules" :disabled="view" label="Tweet" required autocomplete="off">
            </div>
            <div class="row justify-content-center">
              <button type="submit" class="btn text-light col-10" style="background-color: #00acee;" v-on:click="sendNewTweet">SEND</button>
            </div>
        </b-modal>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
export default {
  name: 'app',
  data () {
    return {
      tweets: [],
      modalShow: false,
      tweet: "",
      modify_tweet: "",
      id_tweet: Number,
      view: false,
      tweetRules: [v => !!v || "Name is required"],
    }
  },
  mounted() {
    this.fetchTweets();
  },
  methods: {
    fetchTweets() {
      const baseURI = 'https://twitter-app-back.herokuapp.com/api/tweets'
      this.$http.get(baseURI)
      .then((result) => {
        setTimeout(() => this.tweets = result.data, 2000);
      })
    },
    sendNewTweet() {
      axios.post("https://twitter-app-back.herokuapp.com/api/tweets", {
          id_usuario: 1,
          tweet: this.tweet
        })
        .then(response => {
          this.fetchTweets();
          this.tweet = "";
          this.$refs['my-modal'].hide()
        })
        .catch(e => console.log(e));
    },
    deleteTweet(id_tweet) {
      axios.delete("https://twitter-app-back.herokuapp.com/api/tweets/" + id_tweet)
        .then(response => {
          console.log(response);
          console.log('Test');
          this.fetchTweets();
        })
        .catch(e => console.log(e));
    },
    updateTweet(id_tweet, tweet) {
      this.id_tweet = id_tweet;
      this.modify_tweet = tweet;
    },
    sendEditContact() {   
      axios.put("https://twitter-app-back.herokuapp.com/api/tweets/" + this.id_tweet, {
          id_tweet: this.id_tweet,
          tweet: this.modify_tweet
        })
        .then(response => {
          this.fetchTweets();
          this.$refs['my-modal2'].hide()
        })
        .catch(e => console.log(e));
    },
    showModal() {
        this.$refs['my-modal'].show()
      },
    showModal2() {
        this.$refs['my-modal2'].show()
      }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Patrick+Hand&display=swap');

  .top-logo {
    margin-top: -70px !important;
  }

  .text-patrick {
  font-family: 'Patrick Hand', cursive;
}

.ribbon-wrapper {
    width: 85px;
    height: 88px;
    overflow: hidden;
    position: absolute;
    top: -3px;
    right: -3px
}
.ribbon {
    font-size: 12px;
    color: #FFF;
    text-transform: uppercase;
    font-family: 'Montserrat Bold', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    letter-spacing: .05em;
    line-height: 15px;
    text-align: center;
    text-shadow: 0 -1px 0 rgba(0, 0, 0, .4);
    -webkit-transform: rotate(45deg);
    -moz-transform: rotate(45deg);
    -ms-transform: rotate(45deg);
    -o-transform: rotate(45deg);
    transform: rotate(45deg);
    position: relative;
    padding: 7px 0;
    right: -11px;
    top: 10px;
    width: 100px;
    height: 28px;
    -webkit-box-shadow: 0 0 3px rgba(0, 0, 0, .3);
    box-shadow: 0 0 3px rgba(0, 0, 0, .3);
    background-color: #dedede;
    background-image: -webkit-linear-gradient(top, #00acee 45%, #00acee 100%);
    background-image: -o-linear-gradient(top, #00acee 45%, #00acee 100%);
    background-image: linear-gradient(to bottom, #00acee 45%, #00acee 100%);
    background-repeat: repeat-x;
    filter: progid: DXImageTransform.Microsoft.gradient(startColorstr='#ffffffff', endColorstr='#ffdedede', GradientType=0)
}

.ribbon:before,
.ribbon:after {
    content: "";
    border-top: 3px solid #9e9e9e;
    border-left: 3px solid transparent;
    border-right: 3px solid transparent;
    position: absolute;
    bottom: -3px
}

.ribbon:before {
    left: 0
}

.ribbon:after {
    right: 0
}
</style>
