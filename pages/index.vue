<template>
  <main class="main">
    <h1 class="main__title">Генератор sitemap</h1>
    <div class="main__input">
      <Form @sendedUrl="setTaskId" />
    </div>
    <div class="main__status">
      <Status :task="taskId" @mapGenerateFinished="setTaskState" />
    </div>
    <div class="main__link">
      <Download :state="taskFinished" :task="taskId" v-if="taskFinished" />
    </div>
  </main>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      taskId: 0,
      taskFinished: false,
    };
  },
  methods: {
    setTaskId(taskId) {
      this.taskId = taskId;
      this.taskFinished = false;
    },
    setTaskState() {
      this.taskFinished = true;
    }
  },
}
</script>

<style lang="css" scoped>
.main {
  margin: 0 auto;
  width: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  min-height: 100vh;
}
.main__title {
  font-size: 48px;
  color: #1C1C1C;
}
.main__input {
  width: 100%;
  margin-top: 40px;
}
.main__status {
  width: 90%;
  margin-top: 20px;
}
.main__link {
  margin-top: 40px;
}
</style>
