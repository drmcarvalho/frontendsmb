<template>
  <v-card class="mx-auto" max-width="800" hover>
    <v-card-item>
      <v-card-title>
        Pessoas
      </v-card-title>
      <v-card-subtitle>
        Cadastro de pessoas
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-sheet class="mx-auto" width="800">
        <v-form fast-fail @submit.prevent>
          <v-text-field v-model="model.nome" label="Nome"></v-text-field>
          <v-text-field v-model="model.email" label="Email"></v-text-field>
          <v-text-field v-model="model.telefone" label="Telefone"></v-text-field>
          <v-text-field v-model="model.datanascimento" label="Data de nascimento" type="date"></v-text-field>
          <v-file-input label="Selecione uma foto" v-on:input.native="onFileChange"></v-file-input>
          <div id="preview">
            <img v-if="model.url" :src="model.url">
          </div>
          <v-btn class="mt-2" type="submit" @click="submit" block>Salvar</v-btn>
        </v-form>
      </v-sheet>
    </v-card-item>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    model: {
      nome: '',
      email: '',
      datanascimento: null,
      telefone: '',
      arquivo: null,
      url: null
    }
  }),
  methods: {
    onFileChange(e) {      
      const file = e.target.files[0]
      this.model.url = URL.createObjectURL(file)      
    },
    async submit() {
      try {
        await this.$axios({
          method: "POST",
          url: '/pessoas',
          data: this.model,
        })

        this.redirect()
      }
      catch (error) { alert(error) }
    },
    redirect() {
      this.$router.push({ name: 'pessoas' })
    }
  },
}
</script>
<style scoped>
#preview 
{
  display: flex;
  justify-content: center;
  align-items: center;
}

#preview img 
{
  max-width: 100%;
  max-height: 500px;
}
</style>