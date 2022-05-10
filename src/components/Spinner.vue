<template>
  <div className="spinner-cycle" role="status">
    <span className="sr-only">Loading...</span>
  </div>
</template>

<style scoped>
.spinner-cycle {
  display: inline-block;
  width: 50px;
  height: 50px;
  vertical-align: 2px;
  color: var(--light-color);
  border: 4px solid var(--light-color);
  border-right-color: transparent;
  border-radius: 50%;
  animation: 800ms linear infinite cycle;
  z-index: 1000;
  position: fixed;
  inset: 0;
  margin: auto;
  pointer-events: none;
  user-select: none;
}
@keyframes cycle {
  to {
    transform: rotate(360deg);
  }
}
</style>