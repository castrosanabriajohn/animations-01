<template>
  <div class="container">
    <div class="block" :class="{ animate: blockIsAnimated }"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <div class="container">
    <transition
      name="paragraph"
      @before-enter="beforeEnter"
      @enter="enter"
      @before-leave="beforeLeave"
      @leave="leave"
      @enter-cancelled="enterCancelled"
      @leave-cancelled="leaveCancelled"
    >
      <p v-if="paragraphIsVisible">Sometimes visible</p>
    </transition>
    <button @click="showParagraph">Toggle Paragraph</button>
  </div>
  <div class="container">
    <transition name="button" mode="out-in">
      <button @click="show" v-if="!visible">Show</button>
      <button @click="hide" v-else>Hide</button>
    </transition>
  </div>
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      dialogIsVisible: false,
      blockIsAnimated: false,
      paragraphIsVisible: false,
      enterInterval: null,
      leaveInterval: null,
    };
  },
  methods: {
    beforeEnter(el) {
      console.log('beforeEnter');
      console.log(el);
      el.style.opacity = 0;
    },
    enter(el, done) {
      let i = 1;
      this.enterInterval = setInterval(() => {
        el.style.opacity = i * 0.1;
        i++;
        if (i > 10) {
          clearInterval(this.enterInterval);
          done();
        }
      }, 60);
    },
    beforeLeave(el) {
      console.log('beforeLeave');
      el.style.opacity = 1;
    },
    leave(el, done) {
      let i = 1;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - i * 0.1;
        i++;
        if (i > 10) {
          clearInterval(this.leaveInterval);
          done();
        }
      }, 60);
    },
    enterCancelled() {
      clearInterval(this.enterInterval);
    },
    leaveCancelled() {
      clearInterval(this.leaveInterval);
    },
    show() {
      this.visible = true;
    },
    hide() {
      this.visible = false;
    },
    animateBlock() {
      this.blockIsAnimated = true;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    showParagraph() {
      this.paragraphIsVisible = !this.paragraphIsVisible;
    },
  },
};
</script>

<style>
.button-enter-from,
.button-leave-to {
  opacity: 0;
}

.button-enter-active {
  transition: opacity 0.6s ease-out;
}

.button-leave-active {
  transition: opacity 0.6s ease-in;
}

.button-enter-to,
.button-leave-from {
  opacity: 1;
}

* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  /* transition: transform 0.3s ease-out; */

  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}

.animate {
  /* transform: translateX(-50px); */
  animation: slide-scale 0.3s ease-out forwards;
  transform: translateY(-30px);
}

@keyframes slide-scale {
  0% {
    transform: translateX(0) scale(1);
  }
  70% {
    transform: translateX(-120px) scale(1.1);
  }
  100% {
    transform: translateX(-150px) scale(1);
  }
}
</style>
