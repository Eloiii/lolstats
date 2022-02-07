<template>
  <v-container>
    <!--    summonerName : {{summonerName}}-->
    <!--    <br/>-->
    <!--    summonerId: {{summonerId}}-->
    <!--    <br/>-->
    <!--    accountId: {{accountId}}-->
    <!--    <br/>-->
    <!--    puuid: {{puuid}}-->
    <!--    <br/>-->
    <v-row justify="center" align="center" class="mt-5" v-for="match in matches" :key="match">
      <Match :key="match" :ready="matchesReady" :match="match" :summonerId="summonerId"/>
    </v-row>
  </v-container>
</template>

<script>
import Match from "@/views/Match";

export default {
  name: 'Home',
  components: {Match},
  data() {
    return {
      data: {},
      api_key: "RGAPI-dc1b8fd3-f720-4f98-8e63-a7c7e5d3c938",
      summonerName: "",
      summonerId: "",
      accountId: "",
      puuid: "",
      matches: [],
      matchesReady: false
    }
  },
  methods: {
    async fetchApi(link) {
      const query = await fetch(link)
      const response = await query.json()
      return response
    }
  },
  async created() {
    this.summonerName = this.$route.params.summonerName
    this.data = await this.fetchApi("https://euw1.api.riotgames.com/lol/summoner/v4/summoners/by-name/" + this.summonerName + "?api_key=" + this.api_key)
    this.summonerId = this.data.id
    this.accountId = this.data.accountId
    this.puuid = this.data.puuid
    this.matches = await this.fetchApi("https://europe.api.riotgames.com/lol/match/v5/matches/by-puuid/" + this.puuid + "/ids?start=0&count=10&api_key=" + this.api_key)
    for (let i = 0; i < this.matches.length; i++) {
      this.matches[i] = await this.fetchApi("https://europe.api.riotgames.com/lol/match/v5/matches/" + this.matches[i] + "?api_key=" + this.api_key)
    }
    this.matchesReady = true
  }
}
</script>
