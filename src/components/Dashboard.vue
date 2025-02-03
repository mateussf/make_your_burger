<template>
    <Message :msg="msg" v-show="msg"/>
    <div class="burger-table">
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Pão</div>
                <div>Carne</div>
                <div>Opcionais</div>
                <div>Ações</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.nome }}</div>
                <div>{{ burger.pao }}</div>
                <div>{{ burger.carne }}</div>
                <div>
                    <ul>
                        <li v-for="(opcional, index) in burger.opcionais" :key="index">{{ opcional }}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateBurger($event, burger.id)">
                        <option value="">Selecione</option>
                        <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">{{ s.tipo }}</option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    import Message from './Message.vue';

export default {
    name: 'Dashboard',
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null
        }
    },
    methods: {
        async getPedidos() {
            const response = await fetch('http://localhost:3000/burgers');
            this.burgers = await response.json();

            this.getStatus();
        },
        async getStatus(){
            const response = await fetch('http://localhost:3000/status');
            this.status = await response.json();
        },
        async deleteBurger(id){
            const res = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE"
            });

            this.getPedidos();

            this.msg = `Pedido nº ${res.id} removido com sucesso!`;

            setTimeout(() => {
                this.msg = null;
            }, 3000);

        },
        async updateBurger(event, id){
            const option = event.target.value;
            const dataJson = JSON.stringify({status: option});
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: { "Content-type": "application/json"},
                body: dataJson
            });

            const res = await req.json();

            this.msg = `Pedido nº ${res.id} atualizado para ${res.status}!`;

            setTimeout(() => {
                this.msg = null;
            }, 3000);
        }
    },
    mounted() {
        this.getPedidos();
    },
    components: {
        Message
    }
}
</script>

<style scoped>

    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #burger-table-heading,
    #burger-table-rows,
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

    .burger-table-row{
        width: 100%;
        padding: 12px;
        border: 1px solid #ccc;
    }

    #burger-table-heading .order-id,
    .burger-table-row .order-number{
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
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }
    .delete-btn:hover {
        background-color: #FCBA03;
        color: #222;
    }



</style>