<template>
    <div class="backdrop" v-if="showCreateSubtask">
        <div class="modal">
            <div class="escrita">
                <input type="text" class="input1" placeholder="Nome da subtarefa" v-model="titleSubtask" />
                <input type="text" class="input2" placeholder="Descrição" v-model="descriptionSubtask">
            </div>
            <div class="button">
                <button class="button-cancelar" @click="closeModal()">Cancelar</button>
                <button class="button-criar" @click="CreateTask(), closeModal()">Criar subtarefa</button>
            </div>

            <slot></slot>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import CreateTask from './CreateTask.vue';
export default {
    data() {
        return {
            titleSubtask: '',
            descriptionSubtask: ''
        }
    },
    props: {
        showCreateSubtask: {
            type: Boolean,
            required: true,
        }
    },
    methods: {
        closeModal() {
            console.log('fecha o modal');
            this.$emit('update:showCreateSubtask', false);
        },
        CreateTask() {
            axios.post('/2subtask', {
                titleSubtask: this.titleSubtask,
                descriptionSubtask: this.descriptionSubtask
            })

            console.log('criou a subtarefa');
            console.log(`nome da subtarefa ${this.titleSubtask}`);
            console.log(`descrição de subtarefa ${this.descriptionSubtask}`);
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
    cursor: default;
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

input {
    border: transparent;
    outline: none;
}

.escrita {
    padding-left: 25px;
    padding-top: 11px;
    font-weight: 400;
    display: grid;
    gap: 15px;
}

.input1 {
    width: 180px;
    height: 30px;
    font-size: 16px;
    font-weight: 600;
    color: #81858E;
}

.input2 {
    height: 30px;
    font-size: 14px;
    width: 280px;
    color: #81858E;


}

.button {
    border: 1px solid #E5E5E5;
    width: 678px;
    height: 68px;
    margin-top: 63px;
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
</style>