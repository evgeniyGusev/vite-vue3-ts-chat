<template>
  <button class="app-button">
    <slot>
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" data-v-28d3eb60=""><path d="M6.48 6.794l.49 1.963a1 1 0 0 1-1.94.486l-1-4a1 1 0 0 1 1.393-1.15l15 7a1 1 0 0 1 0 1.813l-15 7a1 1 0 0 1-1.385-1.18l2-7A1 1 0 0 1 7 11h4a1 1 0 0 1 0 2H7.754l-1.19 4.167L17.635 12 6.48 6.794z" fill="#000" fill-rule="nonzero" data-v-28d3eb60=""></path></svg>
    </slot>
  </button>
</template>

<style lang="scss" scoped>
.app-button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 30px;
  border-radius: 5px;
  background: #00d559;

  &:hover {
    transform: translate(3px, 0);
    background: #00a900;
  }
}
</style>
