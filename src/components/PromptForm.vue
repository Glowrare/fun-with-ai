<template>
  <form class="app-form" @submit.prevent="submitForm">
    <div class="form-group">
      <label for="prompt-box" class="form-label">Enter Prompt</label>
      <textarea
        v-model.trim="promptMessage"
        id="prompt-box"
        class="form-control"
        placeholder="Enter text prompt and have fun with GPT-3. e.g. 'Who is batman'"
        autofocus
        rows="5"
      ></textarea>
      <small class="error-message" v-if="this.error"
        >Input cannot be empty!</small
      >
    </div>
    <button class="button">Submit</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      promptMessage: "",
      error: false,
    };
  },
  emits: ["submit-handler"],
  methods: {
    submitForm() {
      this.error = false;

      if (this.promptMessage === "") {
        this.error = true;
        return;
      }

      const formData = {
        prompt: this.promptMessage,
        temperature: 0.5,
        max_tokens: 64,
        top_p: 1.0,
        frequency_penalty: 0.0,
        presence_penalty: 0.0,
      };

      this.$emit("submit-handler", formData);

      this.promptMessage = "";
    },
  },
};
</script>

<style scoped>
.app-form {
  margin-bottom: 50px;
}
.form-group {
  margin-bottom: 20px;
}
.form-label {
  font-size: 14px;
  display: block;
  margin-bottom: 5px;
}
.form-control {
  width: 100%;
  max-width: 500px;
  background: var(--dark-grey);
  border: 2px solid var(--light-color);
  color: var(--light-color);
  padding: 5px;
}
::placeholder {
  color: #fafafa;
}
.form-control:hover,
.form-control:focus {
  background: var(--light-grey);
  color: var(--dark-color);
}
.form-control:focus {
  outline: dotted 2px var(--light-color);
}
.error-message {
  color: #ff0000;
  display: block;
  font-size: 12px;
  font-style: italic;
}
.button {
  background: var(--light-color);
  border: 2px solid var(--light-color);
  border-radius: 10px;
  padding: 10px 25px;
  text-transform: uppercase;
  font-weight: 700;
  font-size: 16px;
  font-family: var(--body-font);

  transition: all 0.3s ease-in-out;
  cursor: pointer;
}
.button:hover,
.button:focus {
  background: var(--dark-color);
  color: var(--light-color);
}
.button:focus {
  outline: dotted 2px currentColor;
}
</style>