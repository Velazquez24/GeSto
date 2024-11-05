<template>
  <div class="home container text-center py-5">
    <div class="intro-section mb-4">
      <img alt="Vue logo" src="../assets/logo.png" class="app-logo mb-3 img-fluid" />
      <h1 class="display-4 text-white">Bienvenido a App Tarea</h1>
      <p class="intro-text text-muted lead">Tu gestor de tareas personal para mantenerte organizado y productivo.</p>
    </div>

    <div class="action-buttons d-flex justify-content-center gap-3">
      <button @click="goToAddTask" class="btn btn-success">Lista de tareas</button>
      <button @click="learnMore" class="btn btn-primary">Aprender más</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  methods: {
    goToAddTask() {
      this.$router.push({ path: '/tasks', query: { addTask: true } });
    },
    learnMore() {
      this.$router.push('/about');
    }
  }
};
</script>

<style scoped>
.app-logo {
  max-width: 100px;
}

h1 {
  color: #ffffff;
}

.home {
  background-color: #121212; /* Fondo oscuro */
  min-height: 100vh;
  padding: 3rem 0;
}

.intro-text {
  color: #bbbbbb;
}
</style>
