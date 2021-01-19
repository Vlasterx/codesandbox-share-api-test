<template>
  <div id="app">
    <p>
      Native web share is <b>{{ shareEnabled }}</b> in this browser.
    </p>
    <button @click="openShare" :disabled="!isSharePresent">Share page</button>
  </div>
</template>

<script>
export default {
  name: "App",
  computed: {
    isSharePresent() {
      return Boolean(navigator.share);
    },
    shareEnabled() {
      return this.isSharePresent ? "available" : "not available";
    },
  },
  methods: {
    openShare() {
      if (navigator.share) {
        // Native Web Share API is supported
        this.nativeShare();
      } else {
        // Opening custom share component
        this.fallbackShare();
      }
    },
    nativeShare() {
      // Works only on HTTPS
      navigator
        .share({
          title: "Share from Vue",
          url: window.location.href,
        })
        .then(() => {
          console.log("Thanks for sharing");
        })
        .catch((error) => {
          console.error(error);
        });
    },
    fallbackShare() {
      console.info("Fallback share opened", {
        title: "Share from Vue",
        url: window.location.href,
      });
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.clickable {
  cursor: pointer;
}
</style>
