<template>
    <div>
        <Message :msg="msg" v-show="msg" /> <!-- Mensagem informando id pedido -->
        <div>
            <form id="burger-form" @submit="createBurger"> <!-- Dispara o evento com createBurger -->
                <div class="input-container">
                    <label for="nome">Nome do cliente:</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha o pão:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione o seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo"> <!-- Percorre o array paes com v-for e exibe em pao.tipo -->
                            {{ pao.tipo }}
                        </option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha a carne do seu burger:</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione o tipo de carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo"> <!-- Percorre o array carnes com v-for e exibe em carne.tipo -->
                            {{ carne.tipo }}
                        </option>
                    </select>
                </div>
                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id"> <!-- Percorre o array opcionais com v-for e exibe em opcional.tipo -->
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar meu Burger!">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
// Diretorios de componentes importados
import Message from './Message.vue'

export default {
    //nome deste componente
    name: "BurgerForm",
    data() {
        return {
            // Variaveis deste componente
            paes: null,
            carnes: null,
            opcionaisdata: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            msg: null, 
        }
    },

    // Métodos
    methods: {
        // Método para buscar ingredientes na "Api"
        async getIngredientes() {

            // Requisição para "Api" local
            const req = await fetch("http://localhost:3000/ingredientes");

            // Convertendo informações da "Api" em JSON
            const data = await req.json();

            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisdata = data.opcionais;
        },

        // Método para criar o pedido (Burger)
        async createBurger(e) {

            e.preventDefault()

            // Carrega as variaveis do formulario para data
            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "solicitado"
            }

            // Converte o objeto em Texto
            const dataJson = JSON.stringify(data);

            // Requisição de envio para "Api"
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            //Retorna a requisição enviada 
            const res = await req.json();

            // colocar msg no sistema
            this.msg =`Pedido Nº ${res.id} realizado com sucesso!`;

            //limpar msg
            setTimeout(() => this.msg = "", 4000);

            // limpar os campos

            this.nome = "";
            this.carne = "";
            this.pao = "";
            this.opcionais = "";
        }
    },
    // Faz o chamado assim que o componente é montado
    mounted() {
        this.getIngredientes()
    },
    // Componentes usados
    components: {
        Message
    },
}
</script>

<style scoped>
    #burger-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #opcionais-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #opcionais-title {
        width: 100%;
    }

    .checkbox-container {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span {
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin:  0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }


</style>