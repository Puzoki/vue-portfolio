<template>
    <div class="container">
        <div class="col-3 alert alert-danger mt-3 mx-auto text-center" role="alert" id="alert">{{this.apologies}}</div>
    </div>
</template>

<script>
    export default {
        name: 'pomodoroClock',
        data() {
            return {
                apologies: 'Извините, проект находится в процессе разработки!'
            }
        },
    }
</script>

<style>

</style>