<template>
    <div class="chat-container">
        <header>
            <img src="gpt_logo.png"
                 alt="logo">
            <button onclick="minimizeChat()"><img src="minimize-window.svg"></button>
        </header>
        <main>

        </main>
        <footer>
            <textarea spellcheck="false"></textarea>
            <button onclick="sendMessage()"><img src="send.svg"></button>
        </footer>
    </div>
</template>
<script setup lang="ts">
</script>
<style lang="scss">
.chat-container {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 30px 1fr 65px;
  justify-content: center;
  height: 600px;
  width: 400px;
  background: #dcdcdc;

  button {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: transparent;
    border: none;
  }

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
      color: white;
      margin-right: 5px;
      img {
        width: 15px;
        height: 5px;
      }
    }
  }

  footer {
    display: grid;
    grid-template-columns: 1fr auto;
    height: 45px;
    width: 90%;
    justify-content: center;
    align-items: center;
    background-color: white;
    border-radius: 25px;
    place-self: center;

    textarea {
      border: none;
      outline: none;
      margin-left: 20px;
      height: 25px;
      width: 100%;
      resize: none;
      :focus {
        border: none;
        outline: none;
      }
      :active {
        border: none;
        outline: none;
      }

    }
    button{
        margin-left: 15px;
    }
  }
}
</style>