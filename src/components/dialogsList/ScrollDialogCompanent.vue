<template>
  <div class="scroll-container">
    <div class="add-dialog-element" @click="addNewDialog">
      <p class="add-dialog-ico">&#10010;</p>
    </div>
    <div
      v-for="dialog in dialogsList"
      :key="dialog.dialog_id">
      <dialogElemet
        active-class="highlighted-dialog"
        @selectDialog="selectDialog"
        @delDialog="delSelectedDialog"
        :dialog='dialog'
      />
    </div>
  </div>
</template>

<script>
import dialogElemet from '@/components/dialogsList/dialogElement.vue'

export default {
  components: {
    dialogElemet
  },
  props: ['dialogsList'],
  data() {
    return {

    }
  },
  methods: {
    addNewDialog(){
      if (this.dialogsList.length === 0 || this.dialogsList[this.dialogsList.length - 1].title !== 'new dialog') {
          this.dialogsList.push({
          title: 'new dialog',
          dialog_id: this.dialogsList.length + 1,
          dialogMessages: []
        })
        console.log(this.dialogsList);
      }

    },
    delSelectedDialog(dialog_id) {
      console.log(dialog_id);
      let del_index = this.dialogsList.findIndex(elem => elem.dialog_id === dialog_id)
      this.dialogsList.splice(del_index, 1)
      localStorage.setItem('dialogsList', JSON.stringify(this.dialogsList));
    },
    selectDialog(dialog_id) {
      this.$emit('selectDialog', dialog_id)

    }
  }
}
</script>

<style scoped>
.scroll-container {
  max-width: 70%;
  overflow-x: auto;
  margin: 2px auto;
  overflow-y: hidden;
  width: 90%;
  height: 60px;
  display: flex;
  flex-direction: row;
  padding-bottom: 7px;
  /* justify-content: center; */
}

.scroll-container::-webkit-scrollbar {
  width: 12px;              /* ширина scrollbar */
}
.scroll-container::-webkit-scrollbar-track {
  background: #2d333b; 
  border-radius: 10px;       /* цвет дорожки */
}
.scroll-container::-webkit-scrollbar-thumb {
  background-color: #3c495b;    /* цвет плашки */
  border-radius: 20px;       /* закругления плашки */
  border: 3px solid #2d333b;
}

.highlighted-dialog {
  border: 2px solid red;
}
.add-dialog-element {
  min-width: 40px;
  height: 40px;
  border: 2px solid #4dc3ff;
  border-radius: 25px;
  margin: 0 5px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.add-dialog-element:hover {
  background: #4dc3ff;
}
.add-dialog-element:active {
  border-color: #fff;
}

.add-dialog-ico {
  font-size: 35px;
  color: #4dc3ff;
  margin: 0 auto;
  transition: all 0.3s ease-in-out;
}
.add-dialog-ico:hover {
  color: #fff;
}

@media (max-width: 980px) {
  .scroll-container {
    max-width: 95%;
  }
}  
</style>