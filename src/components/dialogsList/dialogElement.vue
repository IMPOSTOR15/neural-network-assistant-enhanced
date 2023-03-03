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

<style>
.dialog-element {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  min-width: 200px;
  height: 40px;
  border: 2px solid #fff;
  border-radius: 10px;
  margin: 0 5px;
  cursor: pointer;
}

.dialog-header {
  font-size: 20px;
  margin:8px 5px 8px 5px;
}

.del-ico {
  margin: auto 5px;
}
</style>