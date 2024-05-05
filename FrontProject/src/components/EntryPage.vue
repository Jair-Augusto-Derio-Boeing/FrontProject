<template>
    <div class="back" v-if="showEntryPage">
        <p class="entry">Entrada</p>
        <div class="cards">
            <div class="tasks" v-for="task in Tasks" :key="task.id" @click="openVizualizeTask(task), selectedTask = task "
                :style="selectedTask.id === task.id ? 'background-color: #FAFAFA' : ''">
                <div class="flex">
                    <div class="grid">

                        <div class="round">
                            <input type="checkbox" :name="'checkbox' + task.id" :id="'checkbox' + task.id"
                                :checked="IsTaskChecked(task)" v-on:change="updateTaskStatus(task)" @click.stop="''"/>
                            <label :for="'checkbox' + task.id" @click.stop="''"></label>
                            <p class="title"> {{ task.title }}</p>
                        </div>

                        <p class="description">{{ task.description }}</p>

                        <div :class="{
                            'today': Today(task.due_date),
                            'future': !Today(task.due_date) && !Past(task.due_date),
                            'past': Past(task.due_date)
                        }">
                            <img src="../assets/greenCalendar.svg" v-if="!Past(task.due_date)">
                            <img src="../assets/redCalendar.svg" v-if="Past(task.due_date)">
                            <p>{{ Today(task.due_date) ? "Hoje" : formatDate(task.due_date) }}</p>
                        </div>
                    </div>

                    <div class="icons-hover">
                        <div class="edit">
                            <span id="edit">Editar tarefa</span>
                            <img src="../assets/edit.svg" alt="edit" @click="openEditTask(task)" @click.stop="''">
                        </div>
                        <div class="calendar-hover">
                            <span id="calendar">Definir vencimento</span>
                            <img src="../assets/calendar.svg" alt="calendar" @click="openCreateTask(task)" @click.stop="''">
                        </div>
                        <div class="trash">
                            <span id="trash">Excluir tarefa</span>
                            <img src="../assets/trash.svg" alt="trash bucket" @click="deleteTask(task.id)" @click.stop="''">
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {

    props: {
        showEntryPage: {
            type: Boolean,
            required: true
        },
        showCreateTask: {
            type: Boolean,
            required: true
        },
        
    },
    data() {
        return {
            Tasks: [],
            selectedTask: {},
            taskChecked: [],
            showCreateTask: false

        }
    },
    methods: {
        openCreateTask() {
            this.$emit('openCreateTask')
        },
        openEditTask(task){
            this.$emit('openEditTask', task)
        },
        openVizualizeTask(task){ 
            this.$emit('openVizualizeTask', task)
        },
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
        deleteTask(index) {
            axios.delete(`/task/${index}`)
                .then(() => {
                    let deletedTask = this.Tasks.findIndex(item => item.id ===
                        index)
                    this.Tasks.splice(deletedTask, 1);
                })
                .catch((error) => {
                    console.log(error);
                });
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
        formatDate(dueDate) {
            let date = new Date(dueDate);
            let formatDate = date.toLocaleDateString('pt-BR', { timeZone: 'UTC' });
            return formatDate;
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
    },
    created() {
        this.getTask();
    },


}
</script>

<style scoped>
.today {
    background: #0094881A;
    color: #00948800 !important;
    font-weight: 500;
    width: 72px;
    height: 25px;
    margin-left: 68.5px;
    margin-top: -30px;
    display: flex;


}

.today p {
    color: #009488;
    position: relative;
    left: 12px;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 500;
    line-height: 17.07px;
    text-align: left;
    top: 4px;
}

.today img {
    width: 13;
    height: 14;
    top: -3px;
    left: 5px;
    margin-top: 6px;
    position: relative;

}


.past {
    background: #D314081A;
    color: #D31408 !important;
    font-weight: 500;
    width: 130px;
    height: 30px;
    position: relative;
    left: 65px;
    display: flex;
    top: -15px;
}

.past img {
    width: 13;
    height: 14;
    top: -6px;
    left: 5px;
    margin-top: 12px;
    position: relative;
}

.past p {
    color: #D31408;
    position: relative;
    left: 16px;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    top: 4px;
}

.future {
    background: #0094881A;
    color: #009488 !important;
    font-weight: 500;
    width: 130px;
    height: 30px;
    position: relative;
    left: 65px;
    top: -15px;
    display: flex;

}

.future img {
    width: 13;
    height: 14;
    top: -6px;
    left: 5px;
    margin-top: 12px;
    position: relative;

}

.future p {
    color: #009488;
    position: relative;
    left: 16px;
    font-family: Montserrat;
    font-size: 15px;
    font-weight: 500;
    top: 4px;

}

.back {
    margin-top: 10px;
    margin-left: 900px;

}

.flex {
    display: flex;

}

.grid {
    display: grid;

}

.icons-hover {
    display: flex;
    left: 900px;
    margin-top: 25px;
    height: 109px;
    width: 105px;
    gap: 35px;
    transition: 0s opacity ease;
}

.edit,
.calendar-hover,
.trash img {
    visibility: hidden;

}

.tasks:hover .edit img {
    visibility: visible;
}

.tasks:hover .calendar-hover img {
    visibility: visible;
}

.tasks:hover .trash img {
    visibility: visible;
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
    margin-left: -65px;
    margin-top: -30px;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    visibility: hidden;
}

.cards {
    height: 65vh;
    margin-top: 8%;
    margin-left: -58.8%;
    display: flex;
    flex-direction: column;
    gap: 30px;
    /* position: fixed; */
    overflow: auto;

}

.entry {
    width: 701px;
    height: 40px;
    margin-left: -392px;
    /* margin-top: -400px; */
    font-family: Montserrat;
    font-size: 24px;
    font-weight: 800;
    line-height: 29.26px;
    text-align: left;
    color: black;


}

.tasks {
    cursor: pointer;
    width: 678px;
    height: 109px;
    margin-left: 10px;
    top: 179px;
    left: 458px;
    border: 1px solid #E5E5E5;


}

.tasks:hover {
    background-color: #FAFAFA
}

.title {
    font-family: Montserrat;
    font-size: 16px;
    font-weight: 600;
    line-height: 19.5px;
    text-align: left;
    color: black;

}

.description {
    margin-left: 68px;
    margin-top: -21px;
    color: #81858E;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 400;
    line-height: 17.07px;
    text-align: left;
    width: 430px;


}

.round {
    position: relative;
    margin-left: 25px;
    margin-top: 15px;
    display: flex;
    gap: 30px;
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

.data {
    width: 150px;
    height: 30px;
    margin-top: -30px;
    margin-left: 68px;
    padding-left: 25px;
    border: 1px solid #E5E5E5;
    display: flex;
    align-items: center;
    gap: 12px;
}

.calendar {
    width: 18px;
    height: 18px;
    top: 106px;
    margin-left: -18px;
    font-size: 14px;

}

p {
    border: transparent;
}

.p::-webkit-calendar-picker-indicator {
    display: none;

}
</style>