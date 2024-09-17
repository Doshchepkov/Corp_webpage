<template>
  <div class="grid-container">
    <div class="sidebar">
      <p>Новости</p>
    </div>
    <div class="content">
      <h1>alfasatcom</h1>
    </div>
  </div>
</template>


<script>
export default {
  name: 'Main_page'
};
</script>



<style scoped>
.grid-container {
  display: grid;
  grid-template-columns: 200px 1fr;
  height: 100vh;
}

.sidebar {
  background-color: #f0f0f0;
  padding: 20px;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
}

.content {
  display: flex;
  align-items: center;
  justify-content: center;
}

h1 {
  font-size: 4rem;
  font-weight: bold;
}
</style>
