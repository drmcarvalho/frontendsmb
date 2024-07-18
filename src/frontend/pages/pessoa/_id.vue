<template>
    <v-card class="mx-auto" max-width="800" hover>
        <v-card-item>
            <v-card-title>
                Editar
            </v-card-title>
            <v-card-subtitle>
                Pessoa
            </v-card-subtitle>
        </v-card-item>
        <v-card-item>
            <v-sheet class="mx-auto" width="800">
                <v-form fast-fail @submit.prevent>
                    <v-text-field v-model="model.nome" label="Nome"></v-text-field>
                    <v-text-field v-model="model.email" label="Email"></v-text-field>
                    <v-text-field v-model="model.telefone" label="Telefone"></v-text-field>
                    <v-text-field v-model="model.datanascimento" label="Data de nascimento" type="date"></v-text-field>
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
            id: 0,
            nome: '',
            email: '',
            datanascimento: null,
            telefone: '',
            arquivo: null
        },
    }),
    methods: {
        async submit() {
            try {
                const response = await this.$axios({
                    method: "PUT",
                    url: `/pessoas/${this.model.id}`,
                    data: this.model,
                })

                if ('mensagem' in response.data) {
                    alert(response.data.mensagem)                    
                }
                else {
                    this.$router.push({ name: 'pessoas' })
                }                
            }
            catch (error) {
                console.log(error)
            }
        },
        async obterPessoa() {
            try {
                const config = {
                    method: "GET",
                    url: `/pessoas/${this.$route.params.id}`,
                }

                const pessoa = await this.$axios(config)

                this.model.id = pessoa.data[0].id
                this.model.nome = pessoa.data[0].nome
                this.model.email = pessoa.data[0].email
                this.model.datanascimento = pessoa.data[0].datanascimento
                this.model.telefone = pessoa.data[0].telefone
            } catch (error) {
                console.log(error)
            }
        }
    },
    mounted() {
        this.obterPessoa()
    }
}
</script>