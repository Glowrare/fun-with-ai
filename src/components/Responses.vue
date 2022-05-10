<template>
  <div class="header-wrapper">
    <h2 class="sec-header">Responses</h2>
    <button
      type="button"
      class="button"
      aria-label="Click to delete reponse list"
      title="Delete all responses"
      @click="deleteResponses"
    >
      <img class="icon" src="@/assets/bin.svg" alt="" />
    </button>
  </div>
  <ul class="responses">
    <ResponseItem
      v-for="response in responseList"
      :key="response.id"
      :prompt="response.prompt"
      :choices="response.choices"
      :timeStamp="response.timeStamp"
    />
  </ul>
</template>

<script>
import ResponseItem from "./ResponseItem.vue";
export default {
  components: { ResponseItem },
  emits: ["delete-handler"],
  props: {
    responseList: Array,
  },
  methods: {
    deleteResponses() {
      if (
        confirm(
          "Are you sure you want to delete all responses? This action CANNOT be reversed!"
        )
      ) {
        this.$emit("delete-handler");
      } else return;
    },
  },
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
  overflow: hidden scroll;
}
/* CUSTOM SCROLLBAR */
::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-track {
  background: var(--light-color);
}
::-webkit-scrollbar-thumb {
  background: var(--light-grey);
}
::-webkit-scrollbar-thumb:hover {
  background: var(--dark-grey);
}
</style>