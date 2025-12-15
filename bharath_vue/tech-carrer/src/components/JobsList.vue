<template>
  <div class="jobs-list">
    <div v-for="job in jobs" :key="job.id" class="job-card-container">
      <SingleJob :job="job" />
    </div>
  </div>
</template>

<script>
import SingleJob from './SingleJob.vue';

export default {
  name: 'JobsList',
  components: {
    SingleJob
  },
  props: {
    jobs: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>
.jobs-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 30px; /* Increased gap for better vertical separation */
  width: 100%;
  transition: all 0.3s ease-in-out;
}

.no-jobs {
  grid-column: 1 / -1; /* Spans all columns */
  text-align: center;
  font-size: 1.2rem;
  color: #7f8c8d;
  padding: 40px 20px;
  background-color: #ecf0f1;
  border-radius: 8px;
}

.job-card-container {
  height: 100%;
  width: 100%;
  display: grid;
}

/* Mobile Responsiveness */
@media (max-width: 1024px) {
  .jobs-list {
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
  }
}

@media (max-width: 768px) {
  .jobs-list {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 24px;
    padding: 20px;
  }

  .no-jobs {
    font-size: 1rem;
    padding: 30px 15px;
  }
}

@media (max-width: 480px) {
  .jobs-list {
    grid-template-columns: 1fr;
    gap: 20px;
    padding: 15px;
  }
}
</style>