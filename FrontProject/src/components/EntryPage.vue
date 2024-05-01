<template>
    <div class="bottom" v-if="showEntryPage">
        <p class="entry">Entrada</p>
        <div class="tasks" v-for="task in Tasks" :key="task.id" @click="selectedTask = task"
            :style="selectedTask.id === task.id ? 'background-color: #FAFAFA' : ''">
            <div class="flex">
                <div class="grid">

                    <div class="round">
                        <input type="checkbox" :name="'checkbox' + task.id" :id="'checkbox' + task.id"
                        :checked="IsTaskChecked(task)" v-on:change="updateTaskStatus(task)" />
                        <label :for="'checkbox' + task.id"></label>
                        <p class="title"> {{ task.title }}</p>
                    </div>

                    <p class="description">{{ task.description }}</p>

                    <div class="data">
                        <img src="../assets/calendar.svg" alt="calendar" class="calendar">
                        <p class="p-date">{{ task.due_date }}</p>
                    </div>
                </div>
                <div class="icons-hover">
                    <div class="edit">
                        <span id="edit">Editar tarefa</span>
                        <img src="../assets/edit.svg" alt="edit">
                    </div>
                    <div class="calendar-hover">
                        <span id="calendar">Definir vencimento</span>
                        <img src="../assets/calendar.svg" alt="calendar">
                    </div>
                    <div class="trash">
                        <span id="trash">Excluir tarefa</span>
                        <img src="../assets/trash.svg" alt="trash bucket" @click="deleteTask(task.id)">
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- <button @click="getTask()">vem titulo</button>
        <p>{{  this.Tasks}}</p> -->

</template>

<script>
import axios from 'axios';
export default {

    props: {
        showEntryPage: {
            type: Boolean,
            required: true
        },
    },
    data() {
        return {
            Tasks: [],
            selectedTask: {},
            taskIsChecked: [],

        }
    },
    methods: {
        getTask() {
            axios.get('/task')
                .then((response) => {
                    this.Tasks = response.data.data;
                    console.log(this.Tasks);
                });
        },
        deleteTask(index) {
            axios.delete(`/task/${index}`)
        },
        IsTaskChecked(task) {
            return this.taskIsChecked.some(item => item.id === task.id && item.status === 'completed');
        },
        updateTaskStatus(task) {
            const newStatus = task.status === 'completed' ? 'pending' : 'completed';

            axios.put(`/task/${task.id}`, { status: newStatus })
                .then( () => {
                    
                    const taskToUpdate = this.taskIsChecked.find(item => item.id === task.id);

                    if (taskToUpdate) {
                        taskToUpdate.status = newStatus;
                    } else {
                        this.taskIsChecked.push({ id: task.id, status: newStatus });
                    }

                    task.status = newStatus;
                })
                .catch(error => {
                    console.error(`Error updating task: ${error}`);
                });
        }
    },
    created() {
        this.getTask();
    },

}
</script>

<style scoped>
.flex {
    display: flex;
}

.grid {
    display: grid;
}

.icons-hover {
    position: relative;
    display: flex;
    margin-left: 310px;
    align-items: center;
    height: 109px;
    width: 105px;
    gap: 30px;
    transition: 0.3s opacity ease;
}

.edit,
.calendar-hover,
.trash img {
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
    position: absolute;
    margin-left: -65px;
    margin-top: -30px;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0s linear 0.3s;

}

#calendar {
    position: absolute;
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
    transition: opacity 0.3s ease, visibility 0s linear 0.3s;

}

#trash {
    position: absolute;
    margin-left: -70px;
    margin-top: -30px;
    background-color: #000000CC;
    color: #FFFFFF;
    padding: 5px;
    white-space: nowrap;
    font-size: 13px;
    font-weight: 500;
    line-height: 15.85px;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0s linear 0.3s;

}

.bottom {
    margin-top: -25%;
    height: 100%;
    /* margin-left: 20%; */
    overflow: hidden;
    display: grid;
    gap: 30px;
    /* position: fixed; */

}

.entry {
    width: 701px;
    height: 40px;
    margin-left: 170px;
    margin-top: 255px;
    font-family: Montserrat;
    font-size: 24px;
    font-weight: 800;
    line-height: 29.26px;
    text-align: left;
    color: black;

}

.tasks {

    width: 678px;
    height: 109px;
    margin-left: 170px;
    top: 179px;
    left: 458px;
    border: 1px solid #E5E5E5
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
    margin-top: 8px;
    color: #81858E;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 400;
    line-height: 17.07px;
    text-align: left;


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
    margin-top: 8px;
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