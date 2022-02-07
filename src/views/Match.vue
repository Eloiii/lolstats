<template>
  <v-card width="30vw" :color="win ? 'blue lighten-1' : 'red lighten-1'" :loading="!ready ? 'black' : ''">
      <v-list-item three-line>
        <v-list-item-content>
          <div class="text-overline mb-4" v-if="ready">
            {{time}}
          </div>
          <v-list-item-title class="text-h5 mb-1" v-if="ready">
            {{win ? "VICTOIRE" : "DEFAITE"}} ({{gameMode}})
          </v-list-item-title>
          <v-list-item-subtitle>
            {{score}}
          </v-list-item-subtitle>
        </v-list-item-content>
        <v-list-item-avatar
            tile
            size="80"
        >
          <v-img :src="'http://ddragon.leagueoflegends.com/cdn/12.3.1/img/champion/'+champion+'.png'">

          </v-img>
        </v-list-item-avatar>
      </v-list-item>
  </v-card>
</template>
<script>
export default {
  name: 'Match',
  props: {
    // eslint-disable-next-line vue/require-prop-type-constructor
    ready: "",
    match: {},
    summonerId: null
  },
  data() {
    return {
      win: "...",
      time: "...",
      champion: "...",
      score: "...",
      gameMode : "..."
    }
  },
  created() {
    console.log(this.match)
    for (const player of this.match.info.participants) {
      if(player.summonerId === this.summonerId) {
        this.win = player.win
        this.champion = player.championName
        this.score = player.kills + "/" + player.deaths + "/" + player.assists
      }
    }
    this.gameMode = this.match.info.gameMode
    this.time = new Date(this.match.info.gameCreation)
    this.time = this.time.toLocaleDateString("fr-FR", { weekday: 'short', year: 'numeric', month: 'numeric', day: 'numeric', hour: 'numeric', minute: 'numeric' })
  }
}
</script>