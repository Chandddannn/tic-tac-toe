<template>
  <div class="container">
    <TicTacToe />
  </div>
</template>

<script>
import TicTacToe from './components/TicTacToe.vue';

export default {
  components: {
    TicTacToe
  }
};
</script>

<style>
.container {
  width: 100%;
  height: 100vh; 
  margin: 0; 
  display: flex;
  align-items: center;
  justify-content: center;
  --color: rgba(255, 255, 255, 0.3);
  background-color: #191a1a;
  background-image: linear-gradient(0deg, transparent 24%, var(--color) 25%, var(--color) 26%, transparent 27%, transparent 74%, var(--color) 75%, var(--color) 76%, transparent 77%, transparent),
      linear-gradient(90deg, transparent 24%, var(--color) 25%, var(--color) 26%, transparent 27%, transparent 74%, var(--color) 75%, var(--color) 76%, transparent 77%, transparent);
  background-size: 55px 55px;
}
</style>
