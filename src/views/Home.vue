<template>
  <v-container>
    summonerName : {{summonerName}}
    <br/>
    summonerId: {{summonerId}}
    <br/>
    accountId: {{accountId}}
    <br/>
    puuid: {{puuid}}
  </v-container>
</template>

<script>
  export default {
    name: 'Home',
    data() {
      return {
        data: {},
        api_key: "RGAPI-dc1b8fd3-f720-4f98-8e63-a7c7e5d3c938",
        summonerName: "",
        summonerId: "",
        accountId: "",
        puuid: ""

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
      this.data = await this.fetchApi("https://euw1.api.riotgames.com/lol/summoner/v4/summoners/by-name/"+this.summonerName+"?api_key="+this.api_key)
      this.summonerId = this.data.id
      this.accountId = this.data.accountId
      this.puuid = this.data.puuid
    }
  }
</script>
