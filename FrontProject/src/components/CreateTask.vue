<template>
    <div class="backdrop" v-if="showCreateTask">
        <div class="modal">
            <div class="escrita">
                <input type="text" class="input1" placeholder="Nome da tarefa" v-model="titleTask" />
                <input type="text" class="input2" placeholder="Descrição" v-model="descriptionTask">
                <div class="data">
                    <img src="../assets/calendar.svg" alt="calendar" class="calendar">
                    <input class="input-date" type="date" placeholder="Data de vencimento" v-model="dateTask">
                </div>
            </div>
            <div class="button">
                <button class="button-cancelar" @click="closeModal()">Cancelar</button>
                <button class="button-criar" @click="createTask(), closeModal()">Criar Tarefa</button>
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
            titleTask: '',
            descriptionTask: '',
            dateTask: ''
        }
    },
    props: {
        showCreateTask: {
            type: Boolean,
            required: true,
            
        },

        selectedProduct: {
            type: Object,
            required: false,
        }
    },
    methods: {
        closeModal() {
            console.log('fecha o modal');
            this.$emit('update:showCreateTask', false);
            this.titleTask = ''
            this.descriptionTask = ''
            this.dateTask = 0
        },
        createTask() {
            axios.post('/task', {
                'title': this.titleTask,
                'description': this.descriptionTask,
                'due_date': this.dateTask,
                'status': 'pending'
            });
        }
    },
    watch: {
        showCreateTask(newValue, oldValue) {
            if (newValue === true && this.selectedTask?.id) {
                this.titleTask = this.selectedTask.name;
                this.description = this.selectedTask.amount;
            } else if (newValue === true) {
                this.productName = '';
                this.productValue = 0;

            }
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
    cursor: default;
    z-index: 3;
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

input {
    border: transparent;
    outline: none;
}

.input1 {
    width: 180px;
    font-size: 16px;
    font-weight: 600;
    color: black;
}

.input2 {
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