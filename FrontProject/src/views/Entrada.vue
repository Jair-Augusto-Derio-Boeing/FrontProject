<template>
    <div class="navbar-div">
        <nav class="navbar">
            <div class="rock-and-roll">
                <p>🤘</p>

            </div>
            <img src="../assets/profile.svg" alt="profile" class="profile">
            <div class="ring">
                <img src="../assets/ring.svg" alt="ring">
            </div>
            <div class="question">
                <img src="../assets/question.svg" alt="question">
            </div>

            <div class="plus" @mouseover="showOverCriarTarefa = true" @click="showCriarTarefa = true" @mouseout="showOverCriarTarefa = false">

                <img src="../assets/plus.svg" alt="plus" >

                <CriarTarefa v-model:showCriarTarefa="showCriarTarefa">
                    <!-- :showModal="showModal" @closeModal="showModal = false" -->
                </CriarTarefa>

                <!-- <p class="plus-P">Criar Tarefa</p> -->

                <OverCriarTarefa v-model:showOverCriarTarefa="showOverCriarTarefa"  >

                </OverCriarTarefa>

            </div>
        </nav>
    </div>
</template>

<script>
import CriarTarefa from '../components/CriarTarefa.vue'
import OverCriarTarefa from '../components/OverCriarTarefa.vue';
export default {

    methods: {


    },
    data() {
        return {
            showCriarTarefa: false,
            route: this.$route,
            showOverCriarTarefa: false,
        }
    },
    components: {
        CriarTarefa,
        OverCriarTarefa
    },


}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

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
    width: 28px;
    height: 28px;
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
    gap: 0px;
    border: 2px 0px 0px 0px;
    opacity: 0px;

}

.plus {
    display: flex;
    align-items: center;
    gap: 5px;
    /* background-color: hotpink; */
    width: 40px;
    height: 70px;
    padding-top: 0.5%;
    padding-left: -15%;
    margin-left: -5%;
    transition: 0s;
    cursor: pointer;
}

.plus:hover {
    width: 200px;
    transform: translateX(-90px);
}
</style>