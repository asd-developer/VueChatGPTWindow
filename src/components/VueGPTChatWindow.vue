<template>
    <div class="chat-container">
        <header>
            <img src="gpt_logo.png"
                 alt="logo">
            <button onclick="minimizeChat()"><img src="minimize-window.svg"></button>
        </header>

    </div>
</template>
<script setup lang="ts">
</script>
<style lang="scss">
.chat-container {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 30px 1fr 50px;
  height: 600px;
  width: 400px;
  background: red;

  header {
    display: grid;
    grid-template-columns: 1fr auto;
    height: 35px;
    justify-content: center;
    align-items: center;
    background-color: #75ac9d;
    img {
      max-width: 100%;
      width: 35px;
      height: 35px;
      margin-left: 5px;
    }
    button {
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: transparent;
      border: none;
      color: white;
      margin-right: 5px;
      img {
        width: 15px;
        height: 5px;
      }
    }
  }
}
</style>