<template>
  <div>
    <h2>Saved Jobs</h2>

    <div v-for="job in bookmarks" :key="job._id">
      <SingleJob :job="job" />
    </div>
  </div>
</template>

<script>
import SingleJob from "./SingleJob.vue";

export default {
  components: { SingleJob },
  data() {
    return { bookmarks: [] };
  },
  async created() {
    const res = await fetch("http://localhost:5000/api/bookmarks");
    this.bookmarks = await res.json();
  }
};
</script>
