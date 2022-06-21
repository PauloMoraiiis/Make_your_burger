<template>
    <div id="burger-table">
        <Message :msg="msg" v-show="msg" /> <!-- Mensagem informando id pedido excluido -->
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id"> <!-- O v-for percorre burgers inserindo em burger a cada ciclo -->
                <!-- Exibe os atributos de burger (id, nome, pao, carne) -->
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.nome }}</div>
                <div>{{ burger.pao }}</div>
                <div>{{ burger.carne }}</div>
                <div>
                    <ul v-for="(opcional, index) in burger.opcionais" :key="index"> <!-- Percorre opcionais e imprimi um opcional do pedido a cada repetição  -->
                        <li > {{ opcional }} </li>
                    </ul>
                </div>
                <div>
                    <select name="status" id="status" @change="updateBurger($event, burger.id)"> <!-- Dispara updateBurger quando altera o status do pedido -->
                        <option  v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo"> <!-- Atravez do v-for s.tipo exibe status do pedido, e selected atualiza o status em burger.status -->
                            {{ s.tipo }}
                        </option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)" >Cancelar</button> <!-- Dispara a função deleteBurger -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
// Diretorios de componentes importados
import Message from './Message.vue'

export default {
    //nome deste componente
    name: "Dashboard",
    data() {
        return {
            // Variaveis deste componente
            burgers: null,
            burgers_id: null,
            status: [],
            msg: null,
        }
    },
    // Métodos
    methods: {
        // Método para buscar pedidos na "Api"
        async getPedidos() {

            const req = await fetch('http://localhost:3000/burgers');

            const data = await req.json();

            this.burgers = data;

            //resgatar os status
            this.getStatus();


        },
        // Método para buscar status na "Api"
        async getStatus() {
            const req = await fetch("http://localhost:3000/status");

            const data = await req.json();

            this.status = data;


        },
        // Método para deletar pedido na "Api"
        async deleteBurger(id) {
            
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE"
            });

            const res = await req.json();

            // colocar msg no sistema
            this.msg =`Pedido removido com sucesso!`;

            //limpar msg
            setTimeout(() => this.msg = "", 4000);

            this.getPedidos();
        },
        // Atualiza status do pedido
        async updateBurger(event, id) {

            // Salva em option o status selecionado
            const option = event.target.value;

            // convert JSON em STRING
            const dataJson = JSON.stringify({ status: option });

            // Requisição de alteração na "Api"
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();

            // colocar msg no sistema
            this.msg =`O pedido Nº ${res.id} foi atualizado prara ${res.status}`;

            //limpar msg
            setTimeout(() => this.msg = "", 4000);

            console.log(res);

        }
    },
    // Faz o chamado assim que o componente é montado
    mounted() {
        this.getPedidos();
    },
    // Componentes usados
    components: {
        Message
    },
    
}
</script>

<style scoped>
    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #burger-table-heading,
    #buger-table-rows,
    .burger-table-row {
        display: flex;
        flex-wrap: wrap;
    }

    #burger-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #burger-table-heading div,
    .burger-table-row div {
        width: 19%;
    }

    .burger-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }

    #burger-table-heading .order-id,
    .burger-table-row .order-number {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn {
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

    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }
    


</style>