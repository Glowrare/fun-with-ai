<template>
  <div class="header-wrapper">
    <h2 class="sec-header">Responses</h2>
    <button
      type="button"
      class="button"
      aria-label="Click to delete reponse list"
      title="Delete all responses"
    >
      <img class="icon" src="@/assets/bin.svg" alt="" />
    </button>
  </div>
  <ul class="responses">
    <ResponseItem
      v-for="response in responseList"
      :key="response.id"
      :prompt="response.prompt"
      :response="response.response"
      :timeStamp="response.timeStamp"
    />
  </ul>
</template>

<script>
import ResponseItem from "./ResponseItem.vue";
export default {
  components: { ResponseItem },
  props: {
    responseList: Array,
  },
  // data() {
  //   return {
  //     responseList: [
  //       {
  //         id: 1,
  //         prompt: "Test 1",
  //         response: "Response 1",
  //       },
  //       {
  //         id: 2,
  //         prompt: "Test 2",
  //         response: "Response 2",
  //       },
  //     ],
  //   };
  // },
};
</script>

<style scoped>
.header-wrapper {
  max-width: 500px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.button {
  background: transparent;
  border: none;
  cursor: pointer;
  width: 20px;
}
.button .icon {
  max-width: 100%;
}
.button:hover .icon {
  animation: binShake 0.3s 2 linear;
}
@keyframes binShake {
  0% {
    transform: rotate(0deg) translateY(0);
  }
  25% {
    transform: rotate(30deg) translateY(3px);
  }
  50% {
    transform: rotate(0deg) translateY(0);
  }
  75% {
    transform: rotate(-30deg) translateY(3px);
  }
  100% {
    transform: rotate(0deg) translateY(0);
  }
}
.responses {
  height: 500px;
  border: 2px solid var(--light-color);
  max-width: 500px;

  list-style: none;
  padding: 10px;
}
</style>