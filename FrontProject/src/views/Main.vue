<template>

        <div class="navbar-div">
            <nav class="navbar">
                <img class="rock-and-roll" src="../assets/rockandroll.png" alt="rockhand">
                
                <img src="../assets/profile.svg" alt="profile" class="profile">
                <div class="ring">
                    <img src="../assets/ring.svg" alt="ring">
                </div>
                <div class="question">
                    <img src="../assets/question.svg" alt="question">
                </div>
                
                <div class="plus" @click="showCreateTask = true">
                    <img src="../assets/plus.svg" alt="plus">
                </div>
                
            <CreateTask v-model:showCreateTask="showCreateTask">
            </CreateTask>
            
        </nav>
    </div>
    
    <div class="fundo">

        <div class="side-left">
            <div class="items-left">
                <div class="entrada" @click="showEntryPage = true">
                    <img class="img-entrada" src="../assets/entradaLeft.svg" alt="imagem da entrada">
                    <strong class="strong-entrada">Entrada</strong>
                </div>
                <div class="tarefa-hoje" @click="showEntryPage = false">
                    <img src="../assets/calendarHoje.svg" alt="calendario hoje" class="calendar">
                    <strong class="strong-calendar">Tarefas de hoje</strong>
                </div>
                <div class="vencidos" @click="showEntryPage = false">
                    <img src="../assets/alert.svg" alt="alerta" class="alert">
                    <strong class="strong-alert">Vencidos</strong>
                </div>
            </div>
        </div>
        <EntryPage v-model:showEntryPage="showEntryPage">
            
        </EntryPage>
        <!-- <button class="teste" @click="showCreateSubtask = true">AbreSub</button>    
        <CreateSubtask v-model:showCreateSubtask="showCreateSubtask"></CreateSubtask> -->
    </div>
    </template>


<script>

import CreateTask from '../components/CreateTask.vue'
import CreateSubtask from '../components/CreateSubtask.vue'
import EntryPage from '@/components/EntryPage.vue'
import axios from 'axios'
export default {
    
    data() {
        return {
            showCreateTask: false,
            showCreateSubtask: false,
            showEntryPage: true,
        }
    },

    methods: {


    },

    components: {
        CreateTask,
        CreateSubtask,
        EntryPage,
    },
    created () {
        axios.defaults.baseURL = 'http://127.0.0.1:8000/api';
    },

}

</script>

<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

/* .teste{
    height: 40px;
    width: 80px;
    margin-left: 700px;
} */
.fundo{
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    display: flex;
    align-items: center;
}
* {
    margin: 0px;
    padding: 0px;
}

.navbar {
    width: 100%;
    height: 70px;
    background-color: black;
    color: white;
    display: flex;
}

.rock-and-roll {
    font-size: 28px;
    font-family: Montserrat;
    font-weight: 600;
    width: 32px;
    height: 32px;
    margin-top: 1%;
    margin-left: 2%;
}

.profile {
    width: 30px;
    height: 30px;
    margin-top: 1.5%;
    margin-left: 91%;
}

.ring {
    width: 18px;
    height: 20px;
    margin-top: 1.8%;
    margin-left: -7%;
    border: 2px;
}

.question {
    width: 20px;
    height: 20px;
    margin-top: 1.8%;
    margin-left: -6%;
    border: 2px 0px 0px 0px;
}

.plus {
    display: flex;
    align-items: center;
    gap: 5px;
    width: 40px;
    height: 70px;
    padding-top: 0.5%;
    padding-left: -15%;
    margin-left: -5%;
    transition: 0s;
    cursor: pointer;
}

.plus:hover {
    transform: translateX(-90px);
    width: 200px;
}

.plus:hover:after {
    content: "Criar Tarefa";
}

.side-left {
    width: 293px;
    height: 720px;
    margin-top: 0px;
    background-color: #FAFAFA;
}

.items-left {
    padding-top: 10px;
    width: 231px;
    height: 159.1px;
    margin-left: 31px;
}

.entrada {
    display: flex;
    align-items: end;
    font-size: 17px;
    color: black;

}

.img-entrada {
    padding-top: 60px;
    padding-left: 25px;
    height: 81px;
    cursor: pointer;

}

.strong-entrada {
    padding-left: 27px;
    font-weight: 600;
    cursor: pointer;

}

.tarefa-hoje {
    gap: 15px;
    padding-left: 26.2px;
    padding-top: 35px;
}

.strong-calendar {
    color: black;
    font-weight: 600;
    font-size: 17px;
    margin-left: 31px;
    cursor: pointer;

}
.calendar{
    cursor: pointer;

}

.vencidos {
    display: flex;
    padding-top: 30px;
    padding-left: 25px;
}

.strong-alert {
    color: black;
    font-weight: 600;
    font-size: 17px;
    margin-left: 31px;
    cursor: pointer;

}
.alert{
    cursor: pointer;

}
</style>