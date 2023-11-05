<script setup>
import { RouterLink } from 'vue-router'
import Button from '../components/Button.vue'
</script>

<template>
    <div class="home">
      <h1>Potential Homebuyer's Portal</h1>

      <router-link to="/single">
        <Button class="btn">Do I Qualify?</Button>
      </router-link>
      
      <router-link to="/bulk">
        <Button class="btn">Generate Report</Button>
      </router-link>
    </div>
</template>

<style scoped>
.home h1 {
    font-size: 25px;
    color: #1e78a6;
    font-weight: 700;
    margin-bottom: 0;
}

.home {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    margin: 50px 0;
}

.btn {
  font-weight: bold;
}

</style>