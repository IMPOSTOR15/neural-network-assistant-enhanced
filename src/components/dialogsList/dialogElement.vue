<template>
<div class="dialog-element" @click="selectDialog">
	<p class="dialog-header">{{ curDialog.title }}</p>
	<span class="del-ico" @click="delDialog">&#10006;</span>
</div>
	
</template>

<script>
export default {
	props: ['dialog'],
	data() {
		return {
			curDialog: this.dialog
		}
	},
	methods: {
		selectDialog() {
			this.$emit('selectDialog', this.curDialog.dialog_id)
		},
		delDialog() {
			this.$emit('delDialog', this.curDialog.dialog_id)
		}
	}
}
</script>

<style scoped>
.dialog-element {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  min-width: 200px;
  height: 40px;
  border: 2px solid #4a5461;
  border-radius: 10px;
  margin: 0 5px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}
.dialog-element:hover {
  border: 2px solid #4dc3ff;
}
.dialog-element:active{
  background: #4dc3ff;
}

.dialog-header {
  font-size: 14px;
  margin: auto;
  color: #fff;
  padding: 0 5px;
}

.del-ico {
  font-size: 20px;
	color: #ff9393;
  margin: auto 5px;
}
</style>