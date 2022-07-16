<template>
  <div class="status" v-if="showStatus">
    <div class="status__progress" :style="`width: ${progress}%;max-width: 100%;`"></div>
  </div>
</template>

<script>
export default {
  name: 'Status',
  props: {
    task: {
      required: true,
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      state: {},
    };
  },
  computed: {
    showStatus() {
      if (this.state.hasOwnProperty('current') && this.state.hasOwnProperty('totalCount')) {
        return this.state.current !== this.state.totalCount;
      }
      return false;
    },
    progress() {
      if (this.state.hasOwnProperty('current') && this.state.hasOwnProperty('totalCount')) {
        return Math.round(this.state.current / this.state.totalCount * 100);
      }
    }
  },
  methods: {
    getStatus(taskId) {
      const interval = setInterval(async () => {
        const data = await fetch(`https://api.sitemap-generator.ru/task/stats/${taskId}`);
        const resp = await data.json();
        if (resp.finished) {
          this.$emit('mapGenerateFinished');
          this.state.current = this.state.totalCount;
          this.state.finished = true;
          clearInterval(interval);
          return;
        } else {
          this.state = resp;
        }
      }, 500);
    },
  },
  watch: {
    task(newValue, oldValue) {
      if (newValue !== oldValue) {
        this.state = {};
        this.getStatus(newValue);
      }
    }
  },
}
</script>

<style lang="css" scoped>
.status {
  width: 100%;
  height: 28px;
  border: 1px solid #1C1C1C;
}
.status__progress {
  background-color: rgba(28, 28, 28, 0.5);
  height: 26px;
  width: 0;
  transition: width 0.5s;
}
</style>
