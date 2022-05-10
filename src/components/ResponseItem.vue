<template>
  <li class="response-item">
    <div>
      <h3>Prompt:</h3>
      <p>{{ prompt }}</p>
    </div>
    <div>
      <h3>Response:</h3>
      <p>{{ response }}</p>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    prompt: {
      type: String,
      required: true,
    },
    response: {
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
</style>