<template>
  <section>
    <div class="top-div">
			<h2>ASK A QUESTION</h2>
    </div>
    <ScrollDialogCompanent
      :dialogsList = 'dialogsList'
      @selectDialog='selectCurDialog'
    />
    <div class="dialog-container">
			<li
        class="dialog-li"
        :class="{ 'right-side' : msg.src === 'ai',
                  'left-side' : msg.src === 'user'}"
        v-for="msg in dialogArr"
        :key="msg.curtime">
        <span class="answer-text">
          {{ msg.text }}
        </span>
        
      </li>
      <li
        class="dialog-li right-side-loading"
        v-if="isLoading === true"
      >
        <span class="answer-text loading">
          Ai is typing message...
        </span>
      </li>
    </div>
    <div class="answer-form">
      <textarea class="questionInput" type="text" v-model="questionText" @keyup.enter="loadingNewAnswer" placeholder="Enter your question here"></textarea>
			<button class="send-btn" @click="loadingNewAnswer">SEND</button>
    </div>
    
  </section>
</template>

<script>
import ScrollDialogCompanent from '@/components/dialogsList/ScrollDialogCompanent.vue'
import LoadingIndicator from '@/components/LoadingIndicator.vue'
import axios from 'axios'

export default {
  components: {
    LoadingIndicator,
    ScrollDialogCompanent
  },
	data() {
		return {
      curDialogIndex: 0,
      isLoading: false,
			questionText: '',
			res: null,
      dialogArr: [],
      parentMessageId: null,
      conversationId: null,
      requestBody: {},
      dialogsList: [{
        title: 'new dialog',
        dialog_id: 1,
        dialogMessages: []
      },]
		}
	},
  created() {
    if(localStorage.dialogsList) {
      this.dialogsList = JSON.parse(localStorage.dialogsList);
    }
  },
  watch: {
    dialogsList() {
      console.log('save to storage');
      localStorage.setItem('dialogsList', JSON.stringify(this.dialogsList));
    },
    deep: true
  },
	async mounted() {
    if (this.dialogsList.length != 0) {
      this.dialogArr = this.dialogsList[this.curDialogIndex].dialogMessages
    }
    
	},
	methods: {
    selectCurDialog(dialog_id) {
      this.dialogsList[this.curDialogIndex].dialogMessages = this.dialogArr

      this.curDialogIndex = this.dialogsList.findIndex(elem => elem.dialog_id === dialog_id)
      this.dialogArr = this.dialogsList[this.curDialogIndex].dialogMessages
    },
    async loadingNewAnswer() {
      if (this.questionText !== '') {
        this.isLoading = true;
        await this.sendQuestion(this.questionText);
      }
      
      
    },
		async sendQuestion(questionText){
      this.dialogArr.push({src: "user", text: this.questionText, curtime: Date.now()});
      
      if(this.dialogArr.length != 1) {
        console.log(this.dialogArr[this.dialogArr.length-2]);
        if (this.dialogArr[this.dialogArr.length-2].parentMessageId) {
          this.requestBody = {
            message: questionText,
            parentMessageId: this.dialogArr[this.dialogArr.length-2].parentMessageId,
            conversationId: this.dialogArr[this.dialogArr.length-2].conversationId
          };
          console.log('ask with context');
        }
      }
      else {
        this.requestBody = {
          message: questionText,
        };
        console.log('ask without context');
      }
      this.questionText = '';
      axios.post('https://chatgptapi-dandr212.b4a.run/conversation', this.requestBody)
        .then(response => {
          // console.log(response.data.response);
          this.dialogArr.push(
            {
              src: "ai",
              text: response.data.response,
              curtime: Date.now(),
              parentMessageId: response.data.messageId,
              conversationId: response.data.conversationId,
            });
          this.parentMessageId = response.data.messageId;
          this.conversationId = response.data.conversationId
          this.isLoading = false;
          // console.log(response.data);
          this.addTitle()
          localStorage.setItem('dialogsList', JSON.stringify(this.dialogsList));
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
		},
    async addTitle() {
      if(this.dialogsList[this.curDialogIndex].dialogMessages.length === 2 || this.dialogsList[this.curDialogIndex].title === 'new dialog')
        axios.post('https://chatgptapi-dandr212.b4a.run/conversation', {
          message: 'Reduce the question to two words'+this.dialogArr[0].text,
        })
        .then(response => {
          this.dialogsList[this.curDialogIndex].title = response.data.response
        })
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
  max-width: 80%;
  list-style:none;
  border-radius: 5px;
  word-break: break-word;
  font-size: 18px;

}
.answer-text {
  white-space: pre-wrap;
}
.left-side {
  margin: 5px 10px 5px auto;
}

.error-msg {
  border: 2px solid red;
}
.loading {
  clip-path: inset(0 1.8ch 0 0);
  animation: loading 1s steps(10) infinite;
}

@keyframes loading {
  to {
    clip-path: inset(0 -1ch 0 0)
  }
}
.right-side {
  color: hsl(200, 100%, 93%);
  border: 2px solid hsl(200, 50%, 55%);
  margin: 5px auto 5px 10px ;
}
.right-side-loading {
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