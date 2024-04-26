<template>
    <div class="backdrop" v-if="showCriarTarefa">
        <div class="modal">
            <div class="escrita">
                <p class="p">Nome da tarefa</p>
                <p class="p2">Descrição</p>
                <div class="data">
                    <img src="../assets/calender.svg" alt="calender" class="calender">
                    <p>Data de vencimento</p>
                </div>
            </div>
            <div class="button">
                <button class="button-cancelar" @click="closeModal">Cancelar</button>
                <button class="button-criar">Criar Tarefa</button>
            </div>

            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        closeModal() {
            this.$emit('update:showCriarTarefa', false)
        }
    },
    props: {
        modalTitle: {
            type: String,
            default: ''
        },
        showCriarTarefa: {
            type: Boolean,
            requied: true,
        }
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
}

.p {
    font-size: 16px;
    font-weight: 400;
}

.p2 {
    margin-top: 10px;
    font-size: 14px;
}

.button {
    cursor: pointer;
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

.calender {
    width: 13px;
    height: 14.44px;
    top: 106px;
    margin-left: -8px;
    font-size: 14px;

}
</style>