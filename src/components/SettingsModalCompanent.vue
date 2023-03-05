<template>
    <transition name="modal">
      <div v-if="showModal" class="modal-shadow" @click.self="closeModal">
        <div class="modal">
          <slot name="title">
            <h3 class="modal-title">SETTINGS</h3>
          </slot>
          <slot name="body">
            <div class="modal-content">
              <p>Enter you OPENAI Api-key</p>
              <input class="key-input" v-model="openAIapiKey" type="text" >
              <p>Get it <a target="_ blank" href="https://platform.openai.com/account/api-keys">here</a></p>
            </div>
          </slot>
          <slot name="footer">
            <div class="modal-footer">
              <p v-if="keyIsEmpty" class="error-p">The api key value cannot be empty</p>
              <button class="modal-footer__button" @click="closeModalAndSaveSettings">SAVE</button>
            </div>
          </slot>
        </div>
      </div>
    </transition>
  </template>
  
  <script>
  export default {
    name: "ModalWindow",
    props: ['show'],
    data: function () {
      return {
        showModal: false,
        isChecked: false,
        keyIsEmpty: false,
        openAIapiKey: '',
      }
    },
    watch: {
      show(newValue) {
        this.showModal = newValue
      },
    },
    mounted() {
      this.openAIapiKey = localStorage.OPENAIApiKey
    },
    methods: {
      closeModalAndSaveSettings() {
        if (this.openAIapiKey) {
          localStorage.setItem('OPENAIApiKey', this.openAIapiKey)
          this.showModal = false
          this.$emit('closeModal')
        } else {
          this.keyIsEmpty = true
        }
        
        
      }
    },
    
    
  }
  </script>
  
  <style>
  .modal-shadow {
    position: fixed;
    top: 0;
    left: 0;
    min-height: 100%;
    width: 100%;
    background: rgba(0, 0, 0, 0.59);
    z-index: 9;
  }
  
  .modal {
    color: #fff;
    box-sizing: border-box;
    background-size: cover;
    background: #1D1F20;
    border-radius: 8px;
    width: 50%;
    /* max-width: 680px; */
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .modal-enter-active,
  .modal-leave-active {
    transition: opacity 0.5s ease
  }
  
  .modal-enter-from,
  .modal-leave-to {
    opacity: 0
  }
  .modal-close {
    border-radius: 50%;
    color: #fff;
    background: #1D1F20;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 7px;
    right: 7px;
    width: 30px;
    height: 30px;
    cursor: pointer;
  }
  
  .modal-content {
    margin: 30px;
  }
  .modal-text {
    font-family:'Roboto', sans-serif;
    text-align: left;
  }
  .modal-footer__button {
    background: #1D1F20;
    border: 2px solid #fff;
    border-radius: 10px;
    padding: 5px 20px;
    margin-bottom: 20px;
    font-size: 14px;
    font-weight: bold;
    color: #fff;
    cursor: pointer;
    transition: all 0.2s ease-in;
  }
  
  .modal-footer__button:hover {
    color: #000;
    background: #fff;
  }
  .key-input {
    width: 90%;
    background: transparent;
    border: 1px solid #4a5461;
    border-radius: 5px;
    color: #fff;
    -webkit-text-security: square;
  }
  .error-p {
    color: rgb(255, 135, 135);
  }
  .key-input:focus {
    border: 4px solid #4a5461;
  }
  
  @media (max-width: 750px) {
    .modal-text {
      font-size: 13px;
    }
    .modal-content {
      margin: 15px;
    }
    .modal {
  
      min-width: 90%;
  
    }
  }
  </style>