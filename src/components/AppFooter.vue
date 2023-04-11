<template>
      <footer class="mt-5 navbar-dark bg-primary">
          <div class="container">
              <p>Copyright &copy {{ year }} Flask Inc.</p>
          </div>
      </footer>
</template>

<script setup>
import { ref } from "vue";

const year = (new Date).getFullYear();
</script>

<style>
/* Add any component specific styles here */
footer{
    padding: 10px;
}
</style>