<template>
  <transition name="modal">
    <div v-if="showModal" class="modal-shadow" @click.self="closeModal">
      <div class="modal">
        <slot name="title">
            <h3 class="modal-title">DISCLAIMER</h3>
        </slot>
        <slot name="body">
            <div class="modal-content">
              <p class="modal-text">Disclaimer: This chat application is equipped with artificial intelligence capabilities that allow it to engage in conversations with users in a human-like manner. However, please note that while our AI technology is designed to provide helpful and informative responses, it may not always be accurate or completely relevant to your specific situation.</p>
              <p class="modal-text">We cannot guarantee that the information provided by the AI in this chat application is error-free, complete, or up-to-date. Therefore, we strongly advise that you do not rely solely on the information provided by this chat application for important decisions or actions.</p>
              <p class="modal-text">Additionally, please be aware that the AI in this chat application may not always understand or correctly interpret your messages, particularly if they are ambiguous, contain errors, or use language that is not supported by our system. We encourage you to use clear and concise language when communicating with the AI to ensure the best possible experience.</p>
              <p class="modal-text">Finally, we want to emphasize that this chat application is not intended to replace professional advice or consultation from qualified experts. If you have a serious or urgent issue, we recommend seeking assistance from a trained professional who can provide personalized and reliable guidance.</p>
              <p class="modal-text">By using this chat application, you acknowledge and agree to the above disclaimer and understand that the AI technology is provided "as is" without any warranties, express or implied.</p>
            </div>
        </slot>
        <slot name="check">
          <div class="checkbox-container">
            <input type="checkbox" class="check-input" v-model="isChecked"/>
            <label class="check-label">Don't show this again</label>
          </div>
            
        </slot>
        <slot name="footer">
          <div class="modal-footer">
            <button class="modal-footer__button" @click="closeModal">ОK</button>
          </div>
        </slot>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "ModalWindow",
  props: {
    show: Boolean
  },
  data: function () {
    return {
      // show: true,
      showModal: this.show,
      isChecked: false
    }
  },
  methods: {
    closeModal: function () {
      this.showModal = false
      if (this.isChecked) {
        localStorage.setItem('isDisclaimerReaded', this.isChecked)
      }
    }
  }
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
  min-width: 420px;
  max-width: 480px;
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

.checkbox-container {
  max-height: 20px;
  margin: 20px 30px;
  display: flex;
}
.check-label {
  margin: 0 30px;
  position: fixed;

}

input[type="checkbox"] {
  margin: 0;
  appearance: none;
  -webkit-appearance: none;
  background-color: #444;
  border-radius: 3px;
  border: 2px solid #222;
  box-sizing: border-box;
  height: 20px;
  width: 20px;
  
  transition: background-color 0.2s ease-in-out;
}


input[type="checkbox"]:checked {
  background-color: #fff;
  margin: 0;
}


input[type="checkbox"]:checked::before {
  margin: 0;
  content: "\2714";
  display: block;
  text-align: center;
  font-size: 14px;
  color: #222;
}

</style>