<template>
  <div id="app" :style="{'background-image':'url(https://cdn.discordapp.com/attachments/917544129691541575/925826655866462208/Clementine_SOCIAL_AH_SW_01.jpg)'}" class="hello" >

    <h1 style="color:white;">Welcome to the Clementine faction war!</h1>
    <h4 style="color:white;">Click to decide which faction is best!</h4>
    <h4 style="color:white;">You can vote as many times as you like!</h4>

    <b-row align-h="center" class="mt-5">
      <!-- hollow -->
      <b-card-group deck>
        <b-card bg-variant="dark" img-src="https://cdn.discordapp.com/attachments/917544129691541575/928826387375222804/Clementine_Golem_looped.mp4" img-height="500" img-width="400" text-variant="white" header="Vote @iamabsinthe" class="text-center" footer-tag="footer">
          <b-card-text>Vote Dreamers</b-card-text>
          <b-button size="lg" variant="primary"  @click="vote('dreamer')">Vote Dreamer</b-button>
          <em slot="footer">{{ votesDreamer }} voted</em>
        </b-card>

        <!-- kami -->
        <b-card bg-variant="danger" img-src="https://i.ibb.co/3Wp85b2/rei.png" text-variant="white" header="Vote @reincannon" class="text-center" footer-tag="footer">
          <b-card-text>Vote Rei</b-card-text>
          <b-button size="lg" variant="primary" @click="vote('no')">Vote Rei</b-button>
          <em slot="footer">{{ votesNo }} voted</em>
        </b-card>

        <!-- idk -->
        <b-card bg-variant="dark" img-src="https://i.ibb.co/x8m49M4/absinthe.png" img-height="500" img-width="400" text-variant="white" header="Vote @iamabsinthe" class="text-center" footer-tag="footer">
          <b-card-text>Vote Pris</b-card-text>
          <b-button size="lg" variant="primary"  @click="vote('yes')">Vote Pris</b-button>
          <em slot="footer">{{ votesYes }} voted</em>
        </b-card>


      </b-card-group>
    </b-row>
    <b-row align-h="center" class="mt-5">
      <p style="color:white;">Questions? Ask AlphaIndustries <a href="https://twitter.com/AlphaIndustry">@AlphaIndustry</a>.</p>
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
      if (vote === 'dreamer') this.votesDreamer = response.data.Attributes.votesDreamer
      if (vote === 'no') this.votesNo = response.data.Attributes.votesNo
    },
    updateVotes: async function () {
      const response = await API.get(this.apiName, '/votes/poll-001')
      this.votesNo = response[0].votesNo
      this.votesYes = response[0].votesDreamer
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
