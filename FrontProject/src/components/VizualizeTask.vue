<template>
    <div class="backdrop" v-if="showVizualizeTask">
        <div class="modal">
            <div class="top">
                <div :class="{
                    'term': !Past(selectedTask.due_date),
                    'term-expired': Past(selectedTask.due_date)
                }">
                    <img src="../assets/greenCalendar.svg" v-if="!Past(selectedTask.due_date)">
                    <img src="../assets/redCalendar.svg" v-if="Past(selectedTask.due_date)">
                    <p>{{ Past(selectedTask.due_date) ? 'Expirado' : 'No prazo' }}</p>
                </div>
                <img src="../assets/threePoints.svg" class="threepoints" @click="openMenuTask">
                <img src="../assets/x.svg" class="close" @click="closeVizualizeTask">
            </div>
            <div class="bar"></div>
            <div class="task">
                <div class="round">
                    <input type="checkbox" :name="'checkbox' + selectedTask.id" :id="'checkbox' + selectedTask.id"
                        :checked="IsTaskChecked(selectedTask)" v-on:change="updateTaskStatus(selectedTask)" />
                    <label :for="'checkbox' + selectedTask.id"></label>
                    <p class="title">{{ selectedTask.title }}</p>
                </div>
                <p class="description">{{ selectedTask.description }}</p>
            </div>
            <div class="subtasks">
                <p class="subtask-p">Subtarefas</p>
                <div class="bar2"></div>

                <div class="overflow">

                    <div class="round2" v-for="subtask in selectedTask.subtasks" :key="subtask.id + 1">
                        <input type="checkbox" :name="'checkbox' + (subtask.id + 1)" :id="'checkbox' + (subtask.id + 1)"
                        :checked="SubtaskTaskChecked(subtask)" v-on:change="updateSubtaskStatus(subtask)" />
                        <label :for="'checkbox' + (subtask.id + 1)"></label>
                        <p>{{ subtask.titleSubtask }}</p>
                        
                        <div class="icons-hover">
                            <div class="edit">
                                <span id="edit">Editar subtarefa</span>
                                <img src="../assets/edit.svg" alt="edit" @click="openEditTask(task)" @click.stop="''">
                            </div>
                            <div class="calendar-hover">
                                <span id="calendar">Definir vencimento</span>
                                <img src="../assets/calendar.svg" alt="calendar" @click="openCreateTask(task)"
                                @click.stop="''">
                            </div>
                            <div class="trash">
                                <span id="trash">Excluir subtarefa</span>
                                <img src="../assets/trash.svg" alt="trash bucket" @click="deleteTask(task.id)"
                                @click.stop="''">
                            </div>
                        </div>
                    </div>
                </div>


                <div class="create-subtask" @click="openCreateSubtask(selectedTask)">
                    <img src="../assets/plus.svg">
                    <p>Criar Subtarefa</p>
                </div>
            </div>
            <div class="right-side">
                <div class="create">
                    <p class="create-p">Criado em</p>
                    <div class="date-time">
                        <img src="../assets/blackCalendar.svg">
                        <p>{{ formatCreatedAt(selectedTask.created_at) }}</p>
                    </div>
                </div>
                <div class="date">
                    <p class="date-p">Data de vencimento</p>
                    <div class="date-flex">
                        <img src="../assets/greenCalendar.svg" v-if="!Past(selectedTask.due_date)">
                        <img src="../assets/redCalendar.svg" v-if="Past(selectedTask.due_date)">
                        <p class="date-flex-p1" :class="{
                            'date-flex-p1': !Past(selectedTask.due_date),
                            'date-flex-red': Past(selectedTask.due_date)
                        }">{{ formatdate(selectedTask.due_date) }}</p>
                    </div>
                </div>
                <div class="modified">
                    <p class="">Modificado em</p>
                    <div class="date-modified">
                        <img src="../assets/blackCalendar.svg">
                        <p>{{ formatCreatedAt(selectedTask.updated_at) }}</p>
                    </div>
                </div>
                <div class="id">
                    <p class="id-p1">ID da tarefa</p>
                    <p class="id-p2">{{ selectedTask.id }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
// import CreateSubtask from './CreateSubtask.vue';
// import MenuTask from '../components/MenuTask.vue'
export default {
    data() {
        return {
            Taks: [],
            subtask: [],
            taskChecked: [],

        }
    },
    props: {
        showVizualizeTask: {
            type: Boolean,
            required: true
        },
        selectedTask: {
            type: Object,
            required: false,
        },
        showCreateSubtask: {
            type: Boolean,
            required: true
        }
    },
    components: {
        // MenuTask,
    },
    methods: {
        getTask() {
            axios.get('/task')
                .then((response) => {
                    this.Tasks = response.data.data;
                    console.log(this.Tasks);
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        Today(date) {
            let today = new Date();
            let taskDate = new Date(date);
            return taskDate.getFullYear() === today.getFullYear() &&
                taskDate.getMonth() === today.getMonth() &&
                taskDate.getDate() === today.getDate();
        },
        Past(date) {
            let today = new Date();
            let taskDate = new Date(date);
            return taskDate < today && !this.Today(date);
        },
        closeVizualizeTask() {
            this.$emit('update:showVizualizeTask', false)
            this.$emit('closeMenuTask')
        },
        openMenuTask() {
            this.$emit('openMenuTask')
        },
        formatCreatedAt(created_at) {
            var created_at = created_at;
            var date = new Date(created_at);
            var day = date.getDate();
            var month = date.getMonth() + 1;
            var year = date.getFullYear();
            var hours = date.getHours();
            var minutes = date.getMinutes();
            var formatted_date = (day < 10 ? '0' : '') + day + '/' + (month < 10 ? '0' : '') + month + '/' + year + ' às ' + (hours < 10 ? '0' : '') + hours + ':' + (minutes < 10 ? '0' : '') + minutes;
            return formatted_date;
        },
        formatdate(date) {
            var date = date;
            var date = new Date(date);
            var day = date.getDate();
            var month = date.getMonth() + 1;
            var year = date.getFullYear();
            var formatted_date = (day < 10 ? '0' : '') + day + '/' + (month < 10 ? '0' : '') + month + '/' + year;
            return formatted_date;
        },
        SubtaskTaskChecked(subtask) {
            if (subtask.statusSubtask === 'completed') {
                return true
            } else {
                return false
            }
        },
        updateSubtaskStatus(subtask) {
            console.log(subtask.statusSubtask);
            let newStatus = subtask.statusSubtask === 'completed' ? 'pending' : 'completed';

            axios.put(`/task/${subtask.id}`, { statusSubtask: newStatus })
                .then(() => {

                    let taskToUpdate = this.taskChecked.find(item => item.id === subtask.id);

                    if (taskToUpdate) {
                        taskToUpdate.statusSubtask = newStatus;
                    } else {
                        this.taskChecked.push({ id: subtask.id, statusSubtask: newStatus });
                    }

                    subtask.statusSubtask = newStatus;
                })
        },
        openCreateSubtask(task) {
            this.$emit('openCreateSubtask', task)
        },
        IsTaskChecked(task) {
            if (task.status === 'completed') {
                return true
            } else {
                return false
            }
        },
        updateTaskStatus(task) {
            let newStatus = task.status === 'completed' ? 'pending' : 'completed';

            axios.put(`/task/${task.id}`, { status: newStatus })
                .then(() => {

                    let taskToUpdate = this.taskChecked.find(item => item.id === task.id);

                    if (taskToUpdate) {
                        taskToUpdate.status = newStatus;
                    } else {
                        this.taskChecked.push({ id: task.id, status: newStatus });
                    }

                    task.status = newStatus;
                })
        },
    },
    created() {
        this.getTask()
    },
}
</script>

<style scoped>
.overflow{
    overflow: auto;
    height: 34vh;
}
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
    width: 819px;
    height: 613px;
    align-items: center;
    justify-content: center;
    background-color: #FFFFFF;
    color: #81858E;
    position: relative;
    box-shadow: 2px 4px 10px 0px #0000000D;
}

.top {
    display: flex;
}

.bar {
    width: 819px;
    height: 1px;
    top: 46px;
    gap: 0px;
    opacity: 0px;
    position: relative;
    background: #D9D9D9;
    z-index: 0;
}

.term {
    width: 103px;
    height: 28px;
    top: 23px;
    left: 30px;
    gap: 0px;
    opacity: 0px;
    background: #0094881A;
    position: relative;
    display: flex;
}

.term img {
    width: 13px;
    height: 14.44px;
    top: 5px;
    left: 8px;
    position: relative;
}

.term p {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    color: #009488;
    position: relative;
    top: 2px;
    left: 14px;

}

.term-expired {
    width: 103px;
    height: 28px;
    top: 23px;
    left: 30px;
    gap: 0px;
    opacity: 0px;
    background: #D314081A;
    position: relative;
    display: flex;
}

.term-expired img {
    width: 13px;
    height: 14.44px;
    top: 5px;
    left: 8px;
    position: relative;
}

.term-expired p {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    color: #D31408;

    position: relative;
    top: 2px;
    left: 14px;

}

.threepoints {
    position: relative;
    width: 16px;
    height: 16px;
    top: 32px;
    left: 600px;
    cursor: pointer;

}

.close {
    position: relative;
    width: 13px;
    height: 13px;
    top: 32px;
    left: 640px;
    gap: 0px;
    border: 2px 0px 0px 0px;
    opacity: 0px;
    cursor: pointer;
}

.right-side {
    width: 246px;
    height: 538px;
    top: -162px;
    left: 573px;
    gap: 0px;
    opacity: 0px;
    position: relative;
    background: #F7F7F7;

}

.create {
    width: 160px;
    height: 44px;
    top: 37px;
    left: 30px;
    gap: 0px;
    opacity: 0px;
    position: relative;
    flex-direction: column;
}

.create-p {
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    color: #81858E;

}

.date-time {
    display: flex;
    gap: 5px;
    width: 187px;
    position: relative;
    top: 5px;
}

.date-time p {
    position: relative;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 600;
    color: #000000;

}

.date {
    position: relative;
    top: 77px;
    left: 30px;
}

.date-p {
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    color: #81858E;
}

.date-flex {
    position: relative;
    top: 5px;
    display: flex;
    gap: 5px;
}

.date-flex-p1 {
    color: #009488;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
}

.date-flex-red {
    color: #D31408;

    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
}

.modified {
    position: relative;
    top: 117px;
    left: 30px;

}

.modified p {
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    color: #81858E;
}

.date-modified {
    display: flex;
    gap: 5px;
    position: relative;
    top: 5px;
}

.date-modified p {
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 600;
    line-height: 17.07px;
    text-align: left;
    color: #000000;
}

.id {
    position: relative;
    top: 157px;
    left: 30px;
}

.id-p1 {
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    color: #81858E;
}

.id-p2 {
    position: relative;
    top: 3px;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 600;
    line-height: 17.07px;
    text-align: left;
    color: #000000;


}

.task {
    width: 510px;
    height: 80px;
    top: 107px;
    left: 30px;
    gap: 0px;
    opacity: 0px;
    position: relative;
}

.task-flex {
    display: flex;
}

.task-flex input {
    width: 20px;
    height: 20px;
    top: 98px;
    left: 30px;
    gap: 0px;
    border: 2px 0px 0px 0px;
    opacity: 0px;

}

.title {
    font-family: Montserrat;
    font-size: 18px;
    font-weight: 600;
    line-height: 21.94px;
    text-align: left;
    color: #000000;
    position: relative;
    left: 30px;
}

.description {
    position: relative;
    left: 51px;
    top: 15px;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 400;
    line-height: 21px;
    text-align: left;
    color: #81858E;

}

.subtasks {
    position: relative;
    width: 470px;
    height: 113px;
    top: 140px;
    left: 80px;
}

.bar2 {
    position: relative;
    width: 470px;
    height: 1px;
    top: 17px;
    background: #D9D9D9;



}

.subtask-p {
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 600;
    line-height: 17.07px;
    text-align: left;
    color: #000000;
    position: relative;
    top: 5px;
}


.round2 p {
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 400;
    line-height: 17.07px;
    text-align: left;
    color: #000000;
    position: relative;
    width: 150px;


}

.subtask input {
    width: 20px;
    height: 20px;

}

.round {
    position: relative;
    margin-top: 15px;
    display: flex;
    gap: 5.5px;

}

.round label {
    border: 1px solid#0005;
    border-radius: 50%;
    cursor: pointer;
    height: 20px;
    left: 0;
    position: absolute;
    top: 0;
    width: 20px;
}

.round label:after {
    border: 2px solid #fff;
    border-top: none;
    border-right: none;
    content: "";
    height: 7px;
    left: 4px;
    opacity: 0;
    position: absolute;
    top: 4px;
    transform: rotate(-45deg);
    width: 10px;
}

.round input[type="checkbox"] {
    visibility: hidden;
}

.round input[type="checkbox"]:checked+label {
    background-color: black;
    border-color: #66bb6a;
}

.round input[type="checkbox"]:checked+label:after {
    opacity: 1;
}



.round2 {
    position: relative;
    margin-top: 30px;
    display: flex;
    gap: 30px;
}

.round2 label {
    border: 1px solid#0005;
    border-radius: 50%;
    cursor: pointer;
    height: 20px;
    left: 0;
    position: absolute;
    top: 0;
    width: 20px;
}

.round2 label:after {
    border: 2px solid #fff;
    border-top: none;
    border-right: none;
    content: "";
    height: 7px;
    left: 4px;
    opacity: 0;
    position: absolute;
    top: 4px;
    transform: rotate(-45deg);
    width: 10px;
}

.round2 input[type="checkbox"] {
    visibility: hidden;
}

.round2 input[type="checkbox"]:checked+label {
    background-color: black;
    border-color: #66bb6a;
}

.round2 input[type="checkbox"]:checked+label:after {
    opacity: 1;
}

.create-subtask {
    position: relative;
    display: flex;
    top: 25px;
    width: 135px;
    height: 30px;
    gap: 15px;
    background-color: #0000007d;
    border-radius: 10px;
    cursor: pointer;
}

.create-subtask p {
    position: relative;
    top: 4px;
    font-size: 14px;
    font-weight: 600;
    color: white;
}

.create-subtask img {
    width: 16px;
    height: 16px;
    position: relative;
    top: 7px;
    left: 7px;
}

.icons-hover {
    position: relative;
    display: flex;
    left: 90px;
    align-items: center;
    height: 39px;
    width: 105px;
    gap: 20px;
    transition: 0s opacity ease;
}

.edit,
.calendar-hover,
.trash img {
    visibility: hidden;
    position: relative;
    top: -15px;
}
.trash img{
    position: relative;
    left: 10px;
}

.round2:hover .edit img {
    visibility: visible;
    cursor: pointer;
}

.round2:hover .calendar-hover img {
    visibility: visible;
    cursor: pointer;

}

.round2:hover .trash img {
    visibility: visible;
    cursor: pointer;

}

.edit:hover #edit,
.calendar-hover:hover #calendar,
.trash:hover #trash {
    opacity: 1;
    visibility: visible;
    transition-delay: 0s;
}

#edit {
    position: relative;
    margin-left: -65px;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    font-size: 13px;
    font-weight: 500;
    visibility: hidden;
}

#calendar {
    position: relative;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    margin-left: -100px;
    margin-top: -30px;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    opacity: 0;
    visibility: hidden;


}

#trash {
    position: relative;
    margin-left: -75px;
    top: -18px;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    visibility: hidden;
}
</style>