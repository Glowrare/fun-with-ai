<template>
  <div class="app-wrapper" :style="{ backgroundImage: heroImage }">
    <header>
      <h1 class="pry-header">Fun with AI</h1>
    </header>
    <main>
      <PromptForm @submit-handler="submitHandler" />
      <Responses :responseList="responseList" />
    </main>
  </div>
</template>

<script>
import PromptForm from "./components/PromptForm.vue";
import Responses from "./components/Responses.vue";
export default {
  name: "App",
  components: { PromptForm, Responses },
  data() {
    return {
      heroImage: `linear-gradient(90deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.5)), url(${require("@/assets/hero-bg-image.jpg")})`,
      responseList: [
        {
          id: 1,
          prompt: "Test 1",
          response: "Response 1",
        },
        {
          id: 2,
          prompt: "Test 2",
          response: "Response 2",
        },
      ],
    };
  },
  methods: {
    // submitHandler() {
    //   // e.preventDefault();

    //   const formData = {
    //     prompt: this.promptMessage,
    //     temperature: 0.5,
    //     max_tokens: 64,
    //     top_p: 1.0,
    //     frequency_penalty: 0.0,
    //     presence_penalty: 0.0,
    //   };
    //   console.log(formData);

    //   this.promptMessage = "";
    // },
    async submitHandler(data) {
      // const response = await fetch(
      //   "https://api.openai.com/v1/engines/text-curie-001/completions",
      //   {
      //     method: "POST",
      //     headers: {
      //       "Content-Type": "application/json",
      //       Authorization: `Bearer ${process.env.VUE_APP_API_KEY}`,
      //     },
      //     body: JSON.stringify(data),
      //   }
      // );

      // eslint-disable-next-line no-unused-vars
      // const responseData = await response.text();

      console.log(data);
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
</style>
