<template>
    <div class="menu" v-if="showMenuTask" >
        <div class="copy">
            <img src="../assets/copy.svg">
            <p>Copiar link da tarefa</p>
        </div>
        <div class="duplicate">
            <img src="../assets/duplicate.svg">
            <p>Duplicar tarefa</p>
        </div>
        <div class="printer">
            <img src="../assets/printer.svg">
            <p>Imprimir tarefa</p>
        </div>
        <div class="delete">
            <img src="../assets/delete.svg" @click="deleteTask">
            <p @click="deleteTask">Excluir tarefa</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            Task: this.selectedTask,
        }
    },
    props: {
        selectedTask: {
            type: Object,
            required: false,
        },
        showMenuTask: {
            type: Boolean,
            required: true
        }

    },
    methods: {
        deleteTask() {
            axios.delete(`/task/${this.selectedTask.id}`)
                .then(() => {
                    // this.selectedTask.splice(selectedTask.id, 1);
                    this.$emit('closeMenuTask')

                })
        },
    }

}
</script>

<style scoped>
.menu {
    width: 246px;
    height: 212px;
    top: 100px;
    left: 925px;
    padding: 30px 38px 24px 30px;
    gap: 30px;
    border: 1px 0px 0px 0px;
    opacity: 0px;
    border: 1px solid #E5E5E5;
    background: #FFFFFF;
    box-shadow: 2px 4px 10px 0px #0000000D;
    position: absolute;
    z-index: 3;
    display: grid;
}

.copy {
    display: flex;
    gap: 15px;

}

.copy img {
    width: 18px;
    height: 18px;
    top: 30px;
    left: 30px;
    gap: 0px;
    border: 1.5px 0px 0px 0px;
    opacity: 0px;
    cursor: pointer;


}

.copy p {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    color: black;
    cursor: pointer;

}

.duplicate {
    display: flex;
    gap: 15px;

}

.duplicate img {
    width: 18px;
    height: 18px;
    top: 77px;
    left: 30px;
    gap: 0px;
    border: 1.5px 0px 0px 0px;
    opacity: 0px;
    cursor: pointer;


}

.duplicate p {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    color: black;
    cursor: pointer;

}


.printer {
    display: flex;
    gap: 15px;

}

.printer img {
    width: 18px;
    height: 18px;
    top: 124px;
    left: 30px;
    gap: 0px;
    border: 1.5px 0px 0px 0px;
    opacity: 0px;
    cursor: pointer;


}

.printer p {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    color: black;
    cursor: pointer;

}

.delete {
    display: flex;
    color: red;
    gap: 15px;
}

.delete img {
    width: 17px;
    height: 18px;
    top: 171px;
    left: 30px;
    gap: 0px;
    border: 1.5px 0px 0px 0px;
    opacity: 0px;
    cursor: pointer;

}

.delete p {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    cursor: pointer;
}
</style>