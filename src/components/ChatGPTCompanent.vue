<template>
  <section>
    <div class="top-div">
			<h2>ASK A QUESTION</h2>
    </div>
    <div class="dialog-container">
			<li
        class="dialog-li"
        :class="{ 'right-side' : msg.src === 'ai',
                  'left-side' : msg.src === 'user'}"
        v-for="msg in dialogArr"
        :key="msg.curtime">
        {{ msg.text }}
      </li>
      <LoadingIndicator v-if="isLoading === true"/>
    </div>
    <div class="answer-form">
      <textarea class="questionInput" type="text" v-model="questionText" placeholder="Enter your question here"></textarea>
			<button class="send-btn" @click="loadingNewAnswer">SEND</button>
    </div>
    
  </section>
</template>

<script>
import LoadingIndicator from '@/components/LoadingIndicator.vue'
import axios from 'axios'

export default {
  components: {
    LoadingIndicator,
  },
	data() {
		return {
      isLoading: false,
			questionText: '',
			res: null,
      dialogArr: [],
      parentMessageId: null,
      conversationId: null,
      requestBody: {}
		}
	},
	async mounted() {

	},
	methods: {
    async loadingNewAnswer() {
      if (this.questionText !== '') {
        this.isLoading = true;
        await this.sendQuestion();
      }
      
      
    },
		async sendQuestion(){
      this.dialogArr.push({src: "user", text: this.questionText, curtime: Date.now()});
      if (this.parentMessageId) {
        this.requestBody = {
          message: this.questionText,
          parentMessageId: this.parentMessageId,
          conversationId: this.conversationId
        };
        console.log('ask with context');
      } else {
        this.requestBody = {
          message: this.questionText,
        };
        console.log('ask without context');
      }
      this.questionText = '';
      axios.post('https://chatgptapi-dandr212.b4a.run/conversation', this.requestBody)
        .then(response => {
          console.log(response.data.response);
          this.dialogArr.push({src: "ai", text: response.data.response, curtime: Date.now()});
          this.parentMessageId = response.data.messageId;
          this.conversationId = response.data.conversationId
          this.isLoading = false;
          console.log(response.data);
        })
        .catch(error => {
          console.error(error);
          this.dialogArr.push({src: "er", text: 'error, pls try again', curtime: Date.now()});
          this.isLoading = false;
          
        });
		},
		addDialogHistory() {
			this.dialogArr.push({src: "ai", text: this.res.text, curtime: Date.now()});
			this.questionText = '';
		}
	}
}
</script>

<style>
.answer-form {
  width: 75%;
  display: flex;
  flex-direction: row;
  margin: 0 auto;
  margin-bottom: 100px;
  justify-content: center;
}
.send-btn {
  background: transparent;
  color: #fff;
  width: 10%;
  margin: 0 10px;
  border: 2px solid #9bafc9;
  border-radius: 5px;
  font-weight: bold;
  transition: all 0.2s ease-in;
  cursor: pointer;
}
.send-btn:hover{
  background: #9bafc9;
  color: #1d1f20;
}
.questionInput {
  width: 80%;
  height: 50px;
  resize: none;
  border: 2px solid gray;
  border-radius: 10px;
  padding: 5px;
  background: transparent;
  color: #fff;
  font-family: 'Roboto', sans-serif;
}
.dialog-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  max-width: 70%;
  margin: 10px auto;
  border: 2px solid #2d333b;
  font-family: 'Roboto', sans-serif;
  
}
.dialog-li {
  color: #fff;
  border: 2px solid #4a5461;
  padding: 10px;
  text-align: left;
  max-width: 90%;
  list-style:none;
  border-radius: 5px;
  word-break: break-word;

}

.left-side {
  margin: 5px 10px 5px auto;
  
}

.error-msg {
  border: 2px solid red;
}


.right-side {
  color: hsl(200, 100%, 93%);
  border: none;
  margin: 5px auto 5px 10px ;
  /* border: 2px solid hsl(200, 100%, 93%); */
  --border-size: 2px;
  --border-angle: 0turn;
  background-image: conic-gradient(
      from var(--border-angle),
      transparent 0%,
      #1d1f20,
      #1d1f20 50%,
      #1d1f20
    ),
    conic-gradient(from var(--border-angle), transparent 20%, hsl(200 100% 65%), hsl(200, 100%, 50%));
  background-size: calc(100% - (var(--border-size) * 2))
    calc(100% - (var(--border-size) * 2)),
    cover;
  background-position: center center;
  background-repeat: no-repeat;

  animation: bg-spin 10s linear infinite;
  
}
@keyframes bg-spin {
    to {
      --border-angle: 1turn;
    }
}
@property --border-angle {
  syntax: "<angle>";
  inherits: true;
  initial-value: 0turn;
}

@media (max-width: 980px) {
  .dialog-container {
    max-width: 95%;
  }
  .send-btn {
    width: 30%;
    margin: 0 5px;
  }
  .answer-form {
    width: 100%;
    display: flex;
    flex-direction: row;
    margin: 0 auto;
    margin-bottom: 50px;
    justify-content: center;
  }
  .questionInput {
    margin: 0 5px;
  }
}  
</style>