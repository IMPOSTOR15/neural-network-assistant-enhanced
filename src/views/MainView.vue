<template>
  <div class="gradient"></div>
  <section class="mainSection">
    <div class="gradient-border" id="box">
      <h1 class="header-title">neural network assistant</h1>
    </div>
    <select-component></select-component>
  </section>
  <disclaimerModalWindow :show="isModalShow" />
</template>

<script>
import selectComponent from '@/components/selectComponent.vue'
import disclaimerModalWindow from '@/components/DisclaimerComponent.vue'
export default {
  components: {
    selectComponent,
    disclaimerModalWindow
  },
  data() {
    return {
      isModalShow: null
    }
  },
  created() {
    let curShowValue = localStorage.getItem('isDisclaimerReaded');
    console.log(curShowValue);
    if (curShowValue) {
      this.isModalShow = false
    } else {
      this.isModalShow = true
    }
  }

}
</script>

<style scoped>

.header-title {
  font-size: 40px;
}

#box {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 50px auto;
  width: 400px;
  height: 100px;
  color: white;
  font-size: 2.5rem;
}
.gradient-border {
  --borderWidth: 3px;
  background: #1D1F20;
  position: relative;
  border-radius: var(--borderWidth);
}
.gradient-border:after {
  content: '';
  position: absolute;
  top: calc(-1 * var(--borderWidth));
  left: calc(-1 * var(--borderWidth));
  height: calc(100% + var(--borderWidth) * 2);
  width: calc(100% + var(--borderWidth) * 2);
  background: linear-gradient(60deg, #f79533, #f37055, #ef4e7b, #a166ab, #5073b8, #1098ad, #07b39b, #6fba82);
  border-radius: calc(2 * var(--borderWidth));
  z-index: -1;
  animation: animatedgradient 3s ease alternate infinite;
  background-size: 300% 300%;
}


@keyframes animatedgradient {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}

@media (max-width: 980px) {
  #box {

    margin: 10px auto;

  }
} 
@media (max-width: 750px) {
  .gradient-border {
    max-width: 80%;
  }
  .header-title {
    font-size: 35px;
  }
}
</style>