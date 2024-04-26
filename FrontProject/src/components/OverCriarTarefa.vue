<template>
    <div v-if="showOverCriarTarefa" >
        <p>Criar Tarefa</p>
    </div>
</template>

<script>
export default {
    props: {
        showOverCriarTarefa: {
            type: Boolean,
            requied: true,
        },
    }
}
</script>

<style scoped></style>