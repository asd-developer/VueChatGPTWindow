<template>
    <div class="chat-container">
        <header>
            <img src="gpt_logo.png"
                 alt="logo">
            <button onclick="minimizeChat()"><img src="minimize-window.svg"></button>
        </header>
        <div class="chat-window">
            <ul v-if="allMessages.length">
                <li v-for="(message, index) in allMessages"
                    :key="index"
                    :class="message.sender">
                    {{ message.text }}
                </li>
            </ul>
        </div>
        <footer>
            <textarea spellcheck="false"></textarea>
            <button onclick="sendMessage()"><img src="send.svg"></button>
        </footer>
    </div>
</template>
<script setup lang="ts">
import { ref } from "vue";

const allMessages = ref([
  { sender: "gpt", text: "Hello please write your question bellow." },
  { sender: "user", text: "Hello this is a test." },
] as Message[]);
const GPTApiKey = import.meta.env.VITE_GPT_API;

const sendToChatGPT = async (userMessage: string) => {
  const apiKey = GPTApiKey; // Replace with your actual API key
  const apiUrl = "https://api.openai.com/v1/engines/davinci-codex/completions"; // Adjust the engine as needed

  const response = await fetch(apiUrl, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${apiKey}`,
    },
    body: JSON.stringify({
      prompt: userMessage,
      max_tokens: 50, // Adjust as needed
    }),
  });

  const responseData = await response.json();
  return responseData.choices[0].text;
};

interface Message {
  sender: string;
  text: string;
}
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

  .chat-window {
    li {
      color: black;
    }

    .gpt {
      background-color: #75ac9d;
    }

    .user {
      background-color: white;
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
    button {
      margin-left: 15px;
    }
  }
}
</style>