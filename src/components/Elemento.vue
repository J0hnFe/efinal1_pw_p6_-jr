
<template>
    <div class="imagen">
        <img :src="urlImg" alt="Error imagen">
        <label>{{ nombre }}</label>
    </div>
</template>

<script>
export default {
    props: {
        nombre: {
            type: String,
        },
        urlImg: {
            type: String,
        }
    }
}
</script>

<style>
.imagen {
    display: grid;
    flex-direction: column;
    justify-content: center;
    text-align: center;
}
</style>
