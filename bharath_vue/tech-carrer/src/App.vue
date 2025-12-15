<template>
  <div class="app-wrapper">
    <MyHeader portalName="Tech Jobs Hub" createdBy="Bharath Sudha Chandra Bachala" />
    <div class="main-container">
      <JobsList :jobs="jobs" />
    </div>
  </div>
</template>

<script>
import MyHeader from './components/MyHeader.vue'
import JobsList from './components/JobsList.vue'

export default {
  name: 'App',
  components: {
    MyHeader,
    JobsList
  },
  data() {
    return {
      jobs: []
    }
  },
  methods: {
    async fetchbook() {
      const res = await fetch("https://new-node-production.up.railway.app/api")
      const data = await res.json()
      console.log(data)
      return data
    }
  },
  async created() {
    this.jobs = await this.fetchbook()
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Montserrat', sans-serif;
  background-color: #f5f7fa;
  color: #333;
  line-height: 1.6;
}

.app-wrapper {
  min-height: 100vh;
  display: grid;
  grid-template-rows: auto 1fr;
}

.main-container {
  width: 100%;
  max-width: 1400px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 16px;
  background-color: #fff;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
}

@media (max-width: 1024px) {
  .main-container {
    margin: 30px auto;
    padding: 25px;
  }
}

@media (max-width: 768px) {
  .main-container {
    margin: 20px auto;
    padding: 20px;
    border-radius: 12px;
  }
}

@media (max-width: 480px) {
  .main-container {
    margin: 15px;
    padding: 15px;
    border-radius: 10px;
  }
}
</style>