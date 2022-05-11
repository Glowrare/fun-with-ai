<template>
  <div
    class="app-wrapper"
    :class="{ disabled: loading }"
    :style="{ backgroundImage: heroImage }"
  >
    <header class="app-header">
      <h1 class="pry-header">Fun with AI</h1>
      <img src="@/assets/robot.svg" alt="" class="app-header-icon" />
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
      responseList: [],
      loading: false,
    };
  },
  created() {
    // Check if entry already exists in local storage
    if (localStorage.getItem("localResponseList") !== null) {
      // Retrieve the object from storage
      const retrievedList = localStorage.getItem("localResponseList");

      this.responseList = JSON.parse(retrievedList);
    }
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
      console.log("response data =>");
      this.loading = false;

      const newResponse = {
        id: id,
        timeStamp: new Date().toLocaleString(),
        prompt: data.prompt,
        choices: choices,
      };
      console.log("new response =>");
      console.log(newResponse);
      this.responseList.unshift(newResponse);

      //update to local storage
      localStorage.setItem(
        "localResponseList",
        JSON.stringify(this.responseList)
      );
    },
    deleteHandler() {
      this.responseList = [];
      localStorage.removeItem("localResponseList");
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
  /* background-attachment: fixed; */
  background-position: center top;
  min-height: 100vh;

  padding: 20px 40px;
}
.app-wrapper.disabled {
  pointer-events: none;
  user-select: none;
}
.app-wrapper.disabled button {
  opacity: 0.4;
}
.app-header {
  display: flex;
  margin-bottom: 50px;
  align-items: center;
}
.app-header-icon {
  width: 30px;
  margin-left: 10px;
}
.pry-header {
  font-weight: 700;
  font-size: 54px;
  font-size: clamp(24px, calc(1.5rem + 2vw), 54px);
  text-transform: uppercase;
}
.sec-header {
  font-size: 16px;
  text-transform: uppercase;
}
.sr-only {
  position: absolute;
  width: 0;
  height: 0;
  overflow: hidden;
}
@media screen and (max-width: 480px) {
  .app-wrapper {
    padding-left: 20px;
    padding-right: 20px;
  }
}
</style>
