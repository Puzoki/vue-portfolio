<template>
  <div class="container-fuild bd-dark">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark px-5">
      <span class="navbar-brand" href="#">My projects</span>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item active">
            <a class="nav-link" href="#" @click="this.component='randomColors'">Random Colors</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#" @click="this.component='pomodoroClock'">Pomodoro Clock</a>
          </li>
        </ul>
      </div>
    </nav>
    <keep-alive>
      <component :is="component"></component>
    </keep-alive>
  </div>
</template>

<script>
  import randomColors from './components/randomColors.vue'
  import pomodoroClock from './components/pomodoroClock.vue'

  export default {
    name: '#app',
    components: {
      randomColors,
      pomodoroClock,
    },
    data() {
      return {
        component: 'randomColors'
      }
    },
  }
</script>

<style scoped>

</style>
