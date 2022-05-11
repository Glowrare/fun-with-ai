<template>
  <div class="bg" :style="{ backgroundImage: heroImage }"></div>
  <div class="app-wrapper" :class="{ disabled: loading }">
    <header class="app-header">
      <h1 class="pry-header">Fun with AI</h1>
      <img src="@/assets/robot.svg" alt="" class="app-header-icon" />
    </header>
    <main>
      <PromptForm @submit-handler="submitHandler" />

      <small class="error-message" v-if="this.error"
        >Oops! Something went wrong. Please try again.</small
      >
      <Responses
        v-if="responseList.length >= 1"
        :responseList="responseList"
        @delete-handler="deleteHandler"
      />
    </main>
  </div>
  <button
    v-show="showButton"
    @click.prevent="scrollToTop"
    class="back-to-top-btn"
    aria-label="Click button to go back to top of the page"
  >
    <img src="@/assets/cil_chevron-top.svg" alt="" />
  </button>
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
      showButton: false,
      error: false,
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
  mounted() {
    window.addEventListener("scroll", () => {
      setTimeout(() => {
        this.scrollHandler();
      }, 100);
    });
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.scrollHandler);
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
      this.error = false;
      try {
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

        const newResponse = {
          id: id,
          timeStamp: new Date().toLocaleString(),
          prompt: data.prompt,
          choices: choices,
        };
        this.responseList.unshift(newResponse);

        //update to local storage
        localStorage.setItem(
          "localResponseList",
          JSON.stringify(this.responseList)
        );
      } catch (e) {
        console.error(e);
        this.error = true;

        // Remove error message after three seconts
        setTimeout(() => {
          this.error = false;
        }, 3000);
      }
      this.loading = false;
    },
    deleteHandler() {
      this.responseList = [];
      localStorage.removeItem("localResponseList");
      setTimeout(() => {
        alert("Response List emptied!");
      }, 0);
    },
    scrollHandler() {
      if (window.scrollY > 20) {
        this.showButton = true;
      } else {
        this.showButton = false;
      }
    },
    scrollToTop() {
      window.scrollTo({ top: 0, behavior: "smooth" });
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
body {
  background: var(--dark-color);
}
button {
  font-family: var(--body-font);
}
#app {
  font-family: var(--body-font);
  color: var(--light-color);
  font-weight: 500;
}
.bg {
  position: fixed;

  left: 0;
  right: 0;
  top: 0;
  bottom: 0;

  height: 0;
  padding: 0;
  padding-bottom: 56.25%;
  background-position: center center;
  background-size: 100%;
  background-repeat: no-repeat;
}
.app-wrapper {
  position: relative;
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
.back-to-top-btn {
  /* display: none; */

  background: var(--light-grey);
  color: var(--dark-color);
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  font-size: 30px;

  padding: 10px;

  position: fixed;
  bottom: 50px;
  right: 20px;

  cursor: pointer;
}
.back-to-top-btn:hover,
.back-to-top-btn:focus {
  box-shadow: 1px 2px 16px 4px rgba(255, 255, 255, 0.65);
}
.back-to-top-btn img {
  max-width: 100%;
}
.sr-only {
  position: absolute;
  width: 0;
  height: 0;
  overflow: hidden;
}
.error-message {
  color: #ff0000;
  display: block;
  font-size: 12px;
  font-style: italic;
  position: relative;
  top: -40px;
}
@media screen and (max-width: 480px) {
  .app-wrapper {
    padding-left: 20px;
    padding-right: 20px;
  }
}
</style>
