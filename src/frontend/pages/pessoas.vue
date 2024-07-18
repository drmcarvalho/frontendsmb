<template>
    <v-card class="mx-auto">
    <v-card-item>
      <v-btn color="primary" nuxt to="/cadastropessoa">
        Cadastrar nova pessoa
      </v-btn>
    </v-card-item>
    <v-card-item>
      <v-card-title>
        Pessoas
      </v-card-title>
      <v-card-subtitle>
        Lista de de pessoas
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-text-field label="Digite algo em seguida tecle enter para pesquisar" v-model="query" @keydown.enter="pesquisar" ></v-text-field>
      <v-data-table :headers="headers" :items="pessoas" item-key="id">
        <template v-slot:item.controls="props">
          <v-btn class="mx-2" fab dark small color="blue" @click="editar(props)">
            <v-icon dark>mdi-pencil</v-icon>
          </v-btn>
        </template>
      </v-data-table>        
    </v-card-item>
  </v-card>
</template>

<script>
    export default {
        name: 'Pessoas',
        data: () => ({
            headers: [
            { text: 'Nome', title: 'Nome', value: 'nome' },
            { text: 'Email', title: 'Email', value: 'email' },
            { text: 'Telefone', title: 'Telefone', value: 'telefone' },
            { text: 'Data nascimento', title: 'Data de nascimento', value: 'datanascimento' },
            { text: 'Idade', title: 'Idade', value: 'idade' },
            { text: "", value: "controls", sortable: false }
            ],
            pessoas: [],
            query: "",
        }),
        methods: {
            async obterPessoas() {
                try {
                    const response = await this.$axios({
                      method: "GET",
                      url: '/pessoas/',                    
                    })
                    this.pessoas = response.data
                } catch (error) {
                    return alert(error)
                }
            },
            editar(row) {
                this.$router.push({ path: `pessoa/${row.item.id}` })
            },
            async pesquisar(event) {
                event.preventDefault()
                if (this.query.length == 0) {
                    this.obterPessoas()
                    return
                }
                try {
                    const response = await this.$axios({
                    method: "GET",
                    url: `/pessoas/0/${this.query}`
                    })

                    this.pessoas = response.data
                } catch(error) {
                    alert(error)
                }
            },
        },      
        mounted() {
          this.obterPessoas()
        }
    }
</script>