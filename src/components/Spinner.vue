<template>
  <div>
    <div id="preloader"></div>
  </div>
</template>

<script>
export default {
  name: "Spinner",
  props: {
    showSpinner: {
      type: Boolean,
    },
  },
};
</script>

<style scoped>
#preloader:before {
  content: "";
  position: fixed;
  width: 100%;
  height: 100%;
  border: 6px solid #222831;
  border-top-color: green;
  border-bottom-color: green;
  border-radius: 50%;
  width: 60px;
  height: 60px;
  -webkit-animation: animate-preloader 1s linear infinite;
  animation: animate-preloader 1s linear infinite;
}

@-webkit-keyframes animate-preloader {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes animate-preloader {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
