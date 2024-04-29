<template>
    <div class="backdrop" v-if="abreSub">
        <div class="modal">
            <div class="escrita">
                <input type="text" class="p" placeholder="Nome da subtarefa" v-model="nomeSubTarefa" />
                <input type="text" class="p2" placeholder="Descrição" v-model="descricaoSubTarefa">
            </div>
            <div class="button">
                <button class="button-cancelar" @click="closeModal()">Cancelar</button>
                <button class="button-criar" @click="createTarefa(), closeModal()">Criar subtarefa</button>
            </div>

            <slot></slot>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            nomeSubTarefa: '',
            descricaoSubTarefa: '',
            dateSubTarefa: 0
        }
    },
    props: {
        abreSub: {
            type: Boolean,
            required: true,
        }
    },
    methods: {
        closeModal() {
            console.log('fecha o modal');
            this.$emit('update:showCriarTarefa', false);
        },
        createTarefa() {
            let data = {
                title: this.nomeSubTarefa,
                description: this.descricaoSubTarefa,
                due_date: this.dateSubTarefa,
            }
            // axios.post('', data)
                // .then(() => this.$emit('update:showCriarTarefa', false))
            console.log('criou a subtarefa');
            console.log(`nome da subtarefa ${this.nomeSubTarefa}`);
            console.log(`descrição de subtarefa ${this.descricaoSubTarefa}`);
            console.log(`data de vencimento ${this.dateSubTarefa}`);
        },
    },
    
}
</script>

<style scoped>
.backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    background-color: #0005;
    display: flex;
    justify-content: center;
    align-items: center;
    color: black;
    cursor:default;
}

.modal {
    width: 678px;
    height: 216px;
    align-items: center;
    justify-content: center;
    background-color: #FFFFFF;
    color: #81858E;
    position: relative;
}

.escrita {
    padding-left: 25px;
    padding-top: 21px;
    font-weight: 400;
    height: 148px;
    border: 1px solid #E5E5E5;
    display: grid;
}
input{
    border: transparent;
    outline: none;
}

.p {
    width: 180px;
    font-size: 16px;
    font-weight: 600;
    color:black;
}

.p2 {
    margin-top: 10px;
    font-size: 14px;
    width: 280px;
    color: #81858E;


}

.button {
    width: 678px;
    height: 68px;
    top: 148px;
    gap: 15px;
    justify-content: end;
    align-items: center;
    display: flex;
    padding-right: 20px;
}

.button-cancelar {
    cursor: pointer;
    width: 102px;
    height: 40px;
    top: 162px;
    left: 414px;
    font-weight: 600;
    font-size: 14px;
    background: #F7F7F7;
    border: 0px;

}

.button-criar {
    width: 122px;
    height: 40px;
    top: 162px;
    left: 531px;
    background-color: black;
    color: white;
    font-weight: 600;
    font-size: 14px;
    border: 0px;
    cursor: pointer;

}

.data {
    width: 199px;
    height: 40px;
    margin-top: 18px;
    padding-left: 25px;
    border: 1px solid #E5E5E5;
    display: flex;
    align-items: center;
    gap: 12px;

}

.calendar {
    width: 13px;
    height: 14.44px;
    top: 106px;
    margin-left: -8px;
    font-size: 14px;

}
.input-date::-webkit-calendar-picker-indicator {
    display: none;
}
</style>