<template>
    <div>
      <div v-for="message in messages" :key="message.id">
        <p>{{ message.text }}</p>
        <p>{{ message.response }}</p>
      </div>
      <input type="text" v-model="newMessage" @keyup.enter="sendMessage">
      <button @click="senMsg">Try some</button>
    </div>
  </template>
  
  <script>
  const { Configuration, OpenAIApi } = require("openai");

  const configuration = new Configuration({
    apiKey: process.env.VUE_APP_OPENAI_API_KEY,
  });
  const openai = new OpenAIApi(configuration);

  export default {
    data() {
      return {
        messages: [],
        newMessage: "",
      }
    },
    methods: {
      async senMsg() {
        this.messages.push({
          "role": "user",
          "content": this.newMessage
        })
        let answer = await this.sendMessageToOpenAI()
        console.log(answer);
      },
      async sendMessageToOpenAI() {
        const completion = await openai.createChatCompletion({
          model: 'gpt-3.5-turbo',
          messages: this.messages,
        });
        console.log('completion: ', completion);
        return completion.data.choices[0].message.content;
      }
    }
  }
  </script>
  