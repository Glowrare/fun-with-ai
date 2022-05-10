<template>
  <div
    class="app-wrapper"
    :class="{ disabled: loading }"
    :style="{ backgroundImage: heroImage }"
  >
    <header>
      <h1 class="pry-header">Fun with AI</h1>
    </header>
    <main>
      <PromptForm @submit-handler="submitHandler" />
      <Responses :responseList="responseList" />
    </main>
  </div>
  <Spinner v-if="loading" />
</template>

<script>
import PromptForm from "./components/PromptForm.vue";
import Responses from "./components/Responses.vue";
import Spinner from "./components/Spinner.vue";
export default {
  name: "App",
  components: { PromptForm, Responses, Spinner },
  data() {
    return {
      heroImage: `linear-gradient(90deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.5)), url(${require("@/assets/hero-bg-image.jpg")})`,
      responseList: [
        {
          id: 1,
          prompt: "Test 1",
          response: "Response 1",
          timeStamp: "5/10/2022, 3:00:56 PM",
        },
        {
          id: 2,
          prompt: "Test 2",
          response: "Response 2",
          timeStamp: "5/10/2022, 2:00:56 PM",
        },
      ],
      loading: false,
    };
  },
  watch: {
    loading(newVal) {
      if (newVal) {
        // Disable keyboard interaction when API is fetching Response
        document.onkeydown = function () {
          return false;
        };
      }
    },
  },
  methods: {
    async submitHandler(data) {
      this.loading = true;
      const response = await fetch(
        "https://api.openai.com/v1/engines/text-curie-001/completions",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${process.env.VUE_APP_API_KEY}`,
          },
          body: JSON.stringify(data),
        }
      );

      const responseData = await response.json();
      const { id, choices } = responseData;
      console.log(responseData);
      console.log(`response data => ${responseData}`);
      this.loading = false;

      const newResponse = {
        id: id,
        timeStamp: new Date().toLocaleString(),
        prompt: data.prompt,
        response: choices.text,
      };
      console.log(`new response => ${newResponse}`);
      console.log(newResponse);
      this.responseList.unshift(newResponse);

      // console.log(data);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,500;0,700;1,500&display=swap");

:root {
  --light-color: #ffffff;
  --dark-color: #080909;
  --light-grey: #9aa3a3;
  --dark-grey: #636767;

  --body-font: "Montserrat", sans-serif;
}
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
#app {
  font-family: var(--body-font);
  color: var(--light-color);
  font-weight: 500;
}
.app-wrapper {
  background-repeat: no-repeat;
  background-size: cover;
  background-attachment: fixed;
  background-position: center top;
  min-height: 100vh;

  padding: 20px 40px;
}
.app-wrapper.disabled {
  pointer-events: none;
  user-select: none;
}
.app-wrapper.disabled button {
  cursor: not-allowed;
  opacity: 0.4;
}
.pry-header {
  font-weight: 700;
  font-size: 54px;
  margin-bottom: 50px;
  text-transform: uppercase;
}
.sec-header {
  font-size: 16px;
  text-transform: uppercase;
  /* margin-bottom: 10px; */
  /* font-weight: 500; */
}
.sr-only {
  position: absolute;
  width: 0;
  height: 0;
  overflow: hidden;
}
</style>
