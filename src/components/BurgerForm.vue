<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <div>
            <form id="burger-form" @submit="createBurger">
                <div class="input-container">
                    <label for="nome">Nome:</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
                </div>

                <div class="input-container">
                    <label for="pao">Escolha o tipo de pão</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione o tipo</option>
                        <option v-for="pao in paes" :value="pao.tipo" :key="pao">{{ pao.tipo }}</option>
                    </select>
                </div>

                <div class="input-container">
                    <label for="carne">Escolha o tipo de carne</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione o tipo</option>
                        <option v-for="carne in carnes" :value="carne.tipo" :key="carne">{{ carne.tipo }}</option>
                    </select>
                </div>

                <div id="opctionais-container" class="input-container">
                    <label for="opcionais">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="op in opcionaisdata" :key="op.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="op.tipo"> <span for="alface">{{ op.tipo }}</span>
                    </div>

                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar meu Burger">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
    import Message from './Message.vue';

    export default {
        name: 'BurgerForm',
        data() {
            return {
                paes: null,
                carnes: null,
                opcionaisdata: null,

                nome: null,
                pao: null,
                carne: null,
                opcionais: [],


                msg: null
            }
        },
        methods: {
            async getIngredientes() {
                const req = await fetch('http://localhost:3000/ingredientes');
                const data = await req.json();

                this.paes = data.paes;
                this.carnes = data.carnes;
                this.opcionaisdata = data.opcionais;
            },
            async createBurger(e){
                e.preventDefault();

                const burger = {
                    nome: this.nome,
                    pao: this.pao,
                    carne: this.carne,
                    opcionais: Array.from(this.opcionais),
                    status: "Solicitado",
                }

                const req = await fetch('http://localhost:3000/burgers', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(burger)
                });

                const data = await req.json();
                this.msg = `Pedido Nº ${data.id} realizado com sucesso!`;

                setTimeout(() => {
                    this.msg = null;
                }, 3000);


                this.clearScreen();

            },
            async clearScreen(){
                this.nome = null;
                this.carne = null;
                this.pao = null;
                this.opcionais = [];
            }
        },
        mounted() {
            this.getIngredientes();
        },
        components: {
            Message
        }
    }
</script>

<style lang="css" scoped>
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
        width: 100%;
    }

    #opctionais-container {
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
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>