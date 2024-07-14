<template>
  <div class="time">{{ currentTime }}</div>
  <div style="text-align: center">
    <el-input v-model="input3" style="max-width: 600px" placeholder="Please input" class="input-with-select">
      <template #prepend>
        <el-select v-model="select" placeholder="Select" style="width: 115px">
          <el-option label="Restaurant" value="1" />
          <el-option label="Order No." value="2" />
          <el-option label="Tel" value="3" />
        </el-select>
      </template>
      <template #append>
        <el-button :icon="Search" />
      </template>
    </el-input>
  </div>
</template>
<style lang="less" scoped>
.el-input {
  border-color: transparent;
}
.time {
  text-align: center;
  font-size: 80px;
  font-weight: 400;
  color: #fff;
  height: 80px;
  line-height: 80px;
}
</style>
<script>
export default {
  data() {
    return {
      currentTime: this.formatTime(new Date())
    }
  },
  mounted() {
    this.timer = setInterval(() => {
      this.updateTime()
    }, 1000)
  },
  beforeUnmount() {
    clearInterval(this.timer)
  },
  methods: {
    updateTime() {
      this.currentTime = this.formatTime(new Date())
    },
    formatTime(date) {
      return date.toLocaleTimeString('default', { hour: '2-digit', minute: '2-digit' })
    }
  }
}
</script>
