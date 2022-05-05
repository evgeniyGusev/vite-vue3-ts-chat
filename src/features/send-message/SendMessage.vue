<template>
  <form
    class="send-message"
    @submit.prevent="sendMessage"
  >
    <AppInput v-model="state.message" type="text" placeholder="Введите сообщение" />
    <AppButton />
  </form>
</template>

<script setup lang="ts">
import { reactive } from 'vue';
// Components
import { AppButton, AppInput } from "../../common/ui";

interface State {
  message: string
}

const emit = defineEmits(['send-message'])

const state: State = reactive({
  message: ''
});

const sendMessage = (): void => {
  emit('send-message', state.message);

  state.message = '';
}
</script>

<style lang="scss" scoped>
.send-message {
  display: flex;
}
</style>
