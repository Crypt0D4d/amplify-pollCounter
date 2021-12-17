<template>
  <div id="app" :style="{'background-image':'url(https://vuejs.org/images/logo.png)'}" class="hello" >

    <h1>Welcome to the Nanopass clout vote!</h1>
    <h4>Click to decide who has more clout.</h4>
    <h4>You can vote as many times as you like. Click away!</h4>

    <b-row align-h="center" class="mt-5">
      <b-card-group deck>
        <b-card bg-variant="dark" img-src="https://pbs.twimg.com/media/FFRzLnVVIAA_QHo?format=jpg&name=small" text-variant="white" header="Vote @iamabsinthe" class="text-center" footer-tag="footer">
          <b-card-text>Vote Pris</b-card-text>
          <b-button size="lg" variant="primary"  @click="vote('yes')">Button</b-button>
          <em slot="footer">{{ votesYes }} voted</em>
        </b-card>

        <b-card bg-variant="danger" img-src="https://pbs.twimg.com/profile_images/1461267178594861057/V4yKqxU4_400x400.jpg" text-variant="white" header="Vote @reincanno" class="text-center" footer-tag="footer">
          <b-card-text>Vote Rei</b-card-text>
          <b-button size="lg" variant="primary" @click="vote('no')">Button</b-button>
          <em slot="footer">{{ votesNo }} voted</em>
        </b-card>
      </b-card-group>
    </b-row>
    <b-row align-h="center" class="mt-5">
      <p>Questions? Ask James <a href="https://twitter.com/jbesw">@jbesw</a>.</p>
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
