<template>
  <v-app>
    <v-main class="main">
      <v-select 
        class="StateSelected"
        :items="estados"
        label="Estados"
        v-model="item"
        @change="getData"
        outlined
        >
        </v-select>
    </v-main>
    <v-card
      class="CardStatus"
      elevation="5"
    >
    <div v-if="status">
      <div class="d-flex flex-row">
        <v-img class="icon" :src="icons.brazil" width="2%"/> 
        <h1 v-if="status.state" class="StatusTitle">
          {{status.state}} - {{status.uf}}
        </h1>
        <h1 v-if="status.country" class="StatusTitle">
          {{status.country}}
        </h1>
      </div>

      <div class="d-flex flex-row">
        <v-img class="icon2" :src="icons.virus" width="10%"/>
        <h3>
          Casos ativos : {{status.cases}}
        </h3>
      </div>

      <div class="d-flex flex-row">
        <v-img class="icon2" :src="icons.death" width="10%"/>
        <h3>
          Mortes: {{status.deaths}}
        </h3>
      </div>

      <div class="d-flex flex-row">
        <v-img class="icon2" :src="icons.refuses" width="10%"/>
        <h3 v-if="status.country">
          Recuperados: {{status.recovered}}
        </h3>
        <h3 v-else>
          Recuperados: {{status.refuses}}
        </h3>
      </div>

      <div v-if="status.suspects" class="d-flex flex-row">
        <v-img class="icon2" :src="icons.suspects" width="10%"/>
        <h3>
          Suspeitos: {{status.suspects}}
        </h3>
      </div>
    </div>
    </v-card>
  </v-app>
</template>


<script>


export default {
  data(){
    return {
      icons:{
        brazil: require('../public/icons/brazil.svg'),
        virus: require('../public/icons/virus.svg'),
        death: require('../public/icons/death.svg'),
        suspects: require('../public/icons/suspects.svg'),
        refuses: require('../public/icons/refuses.svg')
      },
      item: '',
      estados: ['AC', 'AL', 'AP', 'AM', 'BA', 'CE', 'DF', 'ES', 'GO', 'MA', 'MT', 'MS', 'MG', 'PA', 'PB', 'PR', 'PE', 'PI', 'RJ', 'RN', 'RS', 'RO', 'RR', 'SC', 'SP', 'SE', 'TO'],
      status: {}
    }
  },
  beforeMount(){
    this.initData()
  },
  methods: {
    async initData(){
      console.log(this.item)
      const baseURL = `https://covid19-brazil-api.now.sh/api/report/v1/brazil`
      console.log(baseURL)
      const response = await fetch(baseURL).then(res => res.json())
      console.log(response)
      this.status = response.data
      console.log(this.status)
    },


    async getData(){
      // console.log(this.item)
      const baseURL = `https://covid19-brazil-api.now.sh/api/report/v1/brazil/uf/${this.item}`
      // console.log(baseURL)
      const response = await fetch(baseURL).then(res => res.json())
      // console.log(response)
      this.status = response
      console.log(this.status)

    }


  }
}
</script>

<style scoped>
  .main {
    margin-top: 70px;
    align-self: center;
  }
  .CardStatus {
    margin-top: 150px;
    width: 400px;
    height: 320px;
    align-self: center;
    position: absolute;
  }
  .StatusTitle {
    margin: 3% 0% 0% 0%;
    font-size: 24px;
  }
  .icon {
    max-width: 45px;
    margin: 3% 2% 4% 4%;
  }
  .icon2 {
    max-width: 40px;
    margin: 3% 2% 0% 5%;
  }
  h3 {
    font-size: 16px;
    margin: 6% 0 0 1%;
  }
</style>