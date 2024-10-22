<template>
  <div>
    <div class="chat-container" v-if="users.length >= 2">

      <div class="user-box">
        <div>
          <img :src="users[0].picture.large" alt="User Picture" />
          <div class="user-color" :class="{ 'bg-left': isRight, 'bg-right': !isRight }"></div>
          <h2>{{ users[0].name.first }} {{ users[0].name.last }}</h2>
          <div>

            <div><textarea v-model="messageUser1" placeholder="escriba su mensaje aqui"></textarea></div>
            <button @click="sendMessage(1)">Enviar</button>
          </div>
        </div>
      </div>

      <div class="bubble-container">
        <ChatBubble v-for="(msg, index) in messages" :key="index" :name="msg.name" :message="msg.message"
          :is-right="msg.isRight" />
      </div>
      <div class="user-box">
        <div>
          <img :src="users[1].picture.large" alt="User Picture" />
          <div class="user-color" :class="{ 'bg-left': !isRight, 'bg-right': isRight }"></div>
          <h2>{{ users[1].name.first }} {{ users[1].name.last }}</h2>
          <div>
            <div class="color"></div>
            <div><textarea v-model="messageUser2" placeholder="escriba su mensaje aqui"></textarea></div>
            <button @click="sendMessage(2)">Enviar</button>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Cargando usuarios...</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import ChatBubble from './components/ChatBubble.vue'

const users = ref([]);
const messages = ref([]);
const messageUser1 = ref('');
const messageUser2 = ref('');

onMounted(async () => {
  try {
    const response = await axios.get('https://randomuser.me/api/?results=2');
    users.value = response.data.results;
  } catch (error) {
    console.error('Error al obtener los usuarios:', error);
  }
});

const sendMessage = (user) => {
  if (user === 1 && messageUser1.value.trim() !== '') {
    messages.value.push({
      name: `${users.value[0].name.first} ${users.value[0].name.last}`,
      message: messageUser1.value,
      isRight: true,
    });
    messageUser1.value = '';
  } else if (user === 2 && messageUser2.value.trim() !== '') {
    messages.value.push({
      name: `${users.value[1].name.first} ${users.value[1].name.last}`,
      message: messageUser2.value,
      isRight: false,
    });
    messageUser2.value = '';
  }
};
</script>

<style scoped>
.chat-container {
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  flex-wrap: wrap;
  padding: 20px;
}

.user-box {
  width: 30%;
  padding: 20px;
  border: 1px solid #ccc;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  background-color: #f9f9f9;
  margin-bottom: 20px;

}

img {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 15px;
}

h2 {
  font-size: 1.2rem;
  margin-bottom: 5px;
}

p {
  font-size: 0.9rem;
  color: #666;
}

.bubble-container {
  width: 300px;
}

button {
  border-radius: 12px;
  padding: 8px;
  margin-top: 12px;
  background-color: rgb(233, 224, 100);
  cursor: pointer;
  width: 200px;

}

button:hover {
  background-color: #666;
}

textarea {
  height: 200px;
  width: 200px;
}
</style>
