<template>
  <li class="response-item">
    <div>
      <h3>Prompt:</h3>
      <p>{{ prompt }}</p>
    </div>
    <div v-for="(choice, index) in choices" :key="index">
      <h3>Response:</h3>
      <p>{{ choice.text }}</p>
    </div>
    <p class="timestamp">
      <small>Generated on {{ timeStamp }}.</small>
    </p>
  </li>
</template>

<script>
export default {
  props: {
    prompt: {
      type: String,
      required: true,
    },
    choices: {
      type: Array,
      required: true,
    },
    timeStamp: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
.response-item {
  background: var(--light-grey);
  color: var(--dark-color);
  font-size: 14px;
  padding: 10px;
}
.response-item:not(:last-child) {
  margin-bottom: 10px;
}
.response-item > div:not(:last-child) {
  margin-bottom: 12px;
}
.response-item h3 {
  font-size: 13px;
}
.timestamp {
  font-style: italic;
  text-align: right;
}
.timestamp small {
  font-size: 10px;
}
</style>