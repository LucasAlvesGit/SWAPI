<template>
  <q-card
    class="my-card text-white"
    style="background: radial-gradient(circle, #35a2ff 0%, #014a88 100%)"
  >
    <q-card-section
      v-for="Personagem in Personagens"
      :key="Personagem.name"
    >
      <div class="text-h6">{{Personagem.name}}</div>
      <div class="text-subtitle2">{{Personagem.species}}, {{Personagem.homeworld}} </div>
    </q-card-section>
  </q-card>
</template>

<script>
export default {
  name: 'PersonagensPage',
  data () {
    return {
      Personagens: [],
      especie: null,
      planeta: []
    }
  },
  methods: {
    async CarregarPersonagem () {
      await this.$axios.get('https://swapi.co/api/people/')
        .then(res => {
          this.Personagens = (res.data.results)
        })
        .then(() => {
          for (let i = 0; i < this.Personagens.length; i++) {
            this.$axios.get(this.Personagens[i].homeworld)
              .then(res => {
                this.Personagens[i].homeworld = (res.data.name)
              })
              .then(() => {
                this.$axios.get(this.Personagens[i].species)
                  .then(res => {
                    this.Personagens[i].species = res.data.name
                  })
              })
          }
        })
    }
  },
  beforeMount () {
    this.CarregarPersonagem()
  }
}
</script>
