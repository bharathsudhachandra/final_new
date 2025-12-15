<template>
  <header class="header">
    <h1 class="portal-name">{{ portalName }}</h1>
  </header>
</template>

<script>
export default {
  name: 'MyHeader',
  props: {
    portalName: {
      type: String,
      required: true
    },
    createdBy: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
.header {
  background-color: #2c3e50;
  color: #ecf0f1;
  padding: 20px 40px;
  text-align: center;
  border-radius: 0 0 12px 12px;
  margin-bottom: 20px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
}

.portal-name {
  font-size: 2.5rem;
  margin-bottom: 10px;
  color: #ecf0f1;
}

.created-by {
  font-size: 1rem;
  color: #bdc3c7;
}
</style>
