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
            <textarea spellcheck="false"
                      v-model="currentMessage"
                      placeholder=""></textarea>
            <button @click="sendToChatGPT(currentMessage)"><img src="send.svg"></button>
        </footer>
    </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

const allMessages = ref([
  { sender: "gpt", text: "Hello please write your question bellow." },
] as Message[]);

const currentMessage = ref("");

const GPTApiKey = import.meta.env.VITE_GPT_API;

async function sendToChatGPT(userMessage: string) {
  allMessages.value.push({ sender: "user", text: currentMessage.value });
  currentMessage.value = "";

  const apiKey = GPTApiKey;
  const apiUrl = "https://api.openai.com/v1/chat/completions"; // Adjust the engine as needed

  const postData = {
    model: "gpt-3.5-turbo",
    messages: [{ role: "user", content: userMessage }],
  };

  const headers = {
    "Content-Type": "application/json",
    Authorization: `Bearer ${apiKey}`,
  };

  try {
    const response = await axios.post(apiUrl, postData, { headers });

    const responseData = await response.data;
    allMessages.value.push({
      sender: "gpt",
      text: responseData.choices[0].message.content,
    });
    return;
  } catch (error) {
    console.error("Error:", error);
  }
}

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
    box-shadow: 2px 2px 10px 0 rgba(0, 0, 0, 0.5);
    z-index: 10;
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
    display: flex;
    justify-content: center;
    margin-top: 5px;
    overflow: auto;
    ul {
      list-style-type: disc;
      margin: 0;
      padding: 0;
      width: 95%;
    }

    li {
      color: black;
      list-style-type: none;
      padding: 10px;
      padding-left: 20px;
      margin: 10px 0;
    }

    .gpt {
      background-color: #75ac9d;
      border-radius: 25px 25px 0px 25px;
    }

    .user {
      background-color: white;
      border-radius: 25px 25px 25px 0px;
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