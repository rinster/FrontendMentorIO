<template>
  <main>  
    <ring-loader :loading="loading" :color="color" :size="size"></ring-loader>
    <div class="adviceCard" v-if="loading === false">
      <p>Advice # {{ currentQuoteId }}</p>
       <h1>“{{ currrentQuote }}”</h1>
       <div class="cardBotton">
        <svg class="desktop" width="444" height="16" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><path fill="#4F5D74" d="M0 8h196v1H0zM248 8h196v1H248z"/><g transform="translate(212)" fill="#CEE3E9"><rect width="6" height="16" rx="3"/><rect x="14" width="6" height="16" rx="3"/></g></g></svg>
        <svg class="mobile" width="295" height="16" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><path fill="#4F5D74" d="M0 8h122v1H0zM173 8h122v1H173z"/><g transform="translate(138)" fill="#CEE3E9"><rect width="6" height="16" rx="3"/><rect x="14" width="6" height="16" rx="3"/></g></g></svg>
        <button @click="fetchAdvice"><svg width="24" height="24" xmlns="http://www.w3.org/2000/svg"><path d="M20 0H4a4.005 4.005 0 0 0-4 4v16a4.005 4.005 0 0 0 4 4h16a4.005 4.005 0 0 0 4-4V4a4.005 4.005 0 0 0-4-4ZM7.5 18a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm0-9a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm4.5 4.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm4.5 4.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm0-9a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Z" fill="#202733"/></svg></button>
      </div>
    </div>
    </main>
</template>

<script>
import RingLoader from 'vue-spinner/src/RingLoader.vue';
export default {
  name: 'App',
  components: {
    RingLoader
  },
  data() {
    return {
      loading: true,
      currrentQuote : "",
      currentQuoteId: null,
      size: 700,
      color: "#4DDD99"
    }
  },
  async created() {
    setTimeout(() => {
       this.fetchAdvice()
    }, 2000)
  },
  methods: {
    fetchAdvice: async function() {
      const resp = await fetch("https://api.adviceslip.com/advice");
      const data = await resp.json();
      this.currrentQuote = data.slip.advice;
      this.currentQuoteId = data.slip.id;
      this.loading = false;
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  background-color: #202732;
  font-size: 18px;
  font-family: Manrope, sans-serif;
  font-weight: 800;
}
main {
  display: flex;
  justify-content: center;
  position: relative;
  min-height: 100vh;
}
.v-spinner {
  position: absolute;
  top: 44%;
  left: 45%;
  transform: translate(-50%, -50%);
}
.adviceCard {
  position: relative;
  background-color: #303A48;
  color: #CEE2E9;;
  padding: 50px 50px 70px 50px;
  border-radius: 20px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px;
  max-height: 500px;
  max-width: 600px;
  text-align: center;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-self: center;
}
.adviceCard p {
  color: #4DDD99;
  text-transform: uppercase;
  letter-spacing: .25em;
  font-size: .9em;
  font-weight: 400;
  margin-bottom: 2rem;
}
.cardBottom {
  width: 100%;
  margin-bottom: 1.5rem;
}
h1 {
    margin-bottom: 2.5rem;
    font-weight: 400;
}

button {
  background-color: hsl(150, 100%, 66%);
  border-radius: 100%;
  padding: 20px;
  border: hsl(150, 100%, 66%);
  position: absolute;
  bottom: -40px;
  left: 45%;
  transition: all 0.2s ease-in-out;

}
@media(hover: hover) {
  button:hover {
    color: hsl(150, 100%, 66%);
    box-shadow: 0px 0px 26px rgba(82, 255, 168, .9);
    padding: 20px;
  }
}
/**Remove hover on mobile screens*/
@media(hover: none) {
  button:hover, button:active {border: 1px solid rgba(82, 255, 168, .9)}
}

@media screen and (min-width: 375px) {
  .mobile {
    display: none;
  }
}
@media screen and (max-width: 374px) {
  .desktop {
    display: none;
  }
  .mobile {
    display: block;
  }
  .adviceCard {
    max-width: 350px;
    padding: 30px 30px 50px 30px;
  }
  p {
    font-size: .6em;
  }
  h1 {
    font-size: 24px;
  }
  button {
    left: 42%;
  }
}
</style>
