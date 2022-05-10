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
      <Responses
        v-if="responseList.length > 1"
        :responseList="responseList"
        @delete-handler="deleteHandler"
      />
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
          id: "cmpl-56PtKK0UPT5wG0IhvYpMLWrABWjme",
          timeStamp: "5/10/2022, 3:32:58 PM",
          prompt: "Write a poem about life",
          choices: [
            {
              text: "\n\nLife is a never-ending cycle\n\nWe go through highs and lows\n\nBut in the end, we all die\n\nAnd that's okay\n\nBecause life is a journey\n\nAnd it's all about learning\n\nAnd growing\n\nAnd experiencing new things\n\nAnd sometimes, we",
              index: 0,
              logprobs: null,
              finish_reason: "length",
            },
          ],
        },
        {
          id: "aaaad",
          prompt: "Test 1",
          choices: [{ text: "Response 1" }, { text: "Response 1b" }],
          timeStamp: "5/10/2022, 3:00:56 PM",
        },
        {
          id: "dddeee",
          prompt: "Test 2",
          choices: [{ text: "Response 2" }],
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
      } else {
        document.onkeydown = function () {
          return true;
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
        choices: choices,
      };
      console.log(`new response => ${newResponse}`);
      console.log(newResponse);
      this.responseList.unshift(newResponse);

      // console.log(data);
    },
    deleteHandler() {
      this.responseList = [];
      setTimeout(() => {
        alert("Response List emptied!");
      }, 0);
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
