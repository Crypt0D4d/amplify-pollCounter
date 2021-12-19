<template>
  <div id="app" :style="{'background-image':'url(https://www.nanopass.io/assets/images/cyberpunk_hq.png)'}" class="hello" >

    <h1 style="color:white;">Welcome to the Nanopass clout war!</h1>
    <h4 style="color:white;">Click to decide who has more clout!</h4>
    <h4 style="color:white;">You can vote as many times as you like!</h4>

    <b-row align-h="center" class="mt-5">
      <b-card-group deck>
        <b-card bg-variant="dark" img-src="https://i.ibb.co/x8m49M4/absinthe.png" img-height="500" img-width="400" text-variant="white" header="Vote @iamabsinthe" class="text-center" footer-tag="footer">
          <b-card-text>Vote Pris</b-card-text>
          <b-button size="lg" variant="primary"  @click="vote('yes')">Vote Pris</b-button>
          <em slot="footer">{{ votesYes }} voted</em>
        </b-card>

        <b-card bg-variant="danger" img-src="https://i.ibb.co/3Wp85b2/rei.png" text-variant="white" header="Vote @reincannon" class="text-center" footer-tag="footer">
          <b-card-text>Vote Rei</b-card-text>
          <b-button size="lg" variant="primary" @click="vote('no')">Vote Rei</b-button>
          <em slot="footer">{{ votesNo }} voted</em>
        </b-card>
      </b-card-group>
    </b-row>
    <b-row align-h="center" class="mt-5">
      <p style="color:white;">Questions? Ask Crypt0D4d <a href="https://twitter.com/Crypt0d4d">@Crypt0D4d</a>.</p>
    </b-row>
  </div>  
</template>


<script>
import { API } from 'aws-amplify'
export default {
  name: 'app',
  data() {
    return {
      apiName: 'pollCounterAPI',
      votesYes: 0,
      votesNo: 0
    }
  },
  methods: {
    vote: async function (vote) {
      const init = {
        queryStringParameters: {
          vote
        }
      }
      const response = await API.post(this.apiName, '/votes', init)
      if (vote === 'yes') this.votesYes = response.data.Attributes.votesYes
      if (vote === 'no') this.votesNo = response.data.Attributes.votesNo
    },
    updateVotes: async function () {
      const response = await API.get(this.apiName, '/votes/poll-001')
      this.votesNo = response[0].votesNo
      this.votesYes = response[0].votesYes    
    }
  },
  created () {
    this.updateVotes()
    setInterval(this.updateVotes, 3000)
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
