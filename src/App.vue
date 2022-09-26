<template>
  <div class="container">
    <div class="block" :class="{ animate: blockIsAnimated }"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <div class="container">
    <transition name="paragraph">
      <p v-if="paragraphIsVisible">Sometimes visible</p>
    </transition>
    <button @click="showParagraph">Toggle Paragraph</button>
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
      dialogIsVisible: false,
      blockIsAnimated: false,
      paragraphIsVisible: false,
    };
  },
  methods: {
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

/* .v-enter-from {
  opacity: 0;
  transform: translateY(-30px);
} */

.paragraph-enter-active {
  /* transition: all 0.6s ease-out; */
  animation: slide-scale 0.6s ease-in;
}

/* .v-enter-to {
  opacity: 1;
  transform: translateY(0);
} */
/* 
.v-leave-from {
  opacity: 1;
  transform: translateY(0px);
} */

.paragraph-leave-active {
  /* transition: all 0.6s ease-in; */
  animation: slide-scale 0.6s ease-out;
}

/* .v-leave-to {
  opacity: 0;
  transform: translateY(30px);
} */

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
