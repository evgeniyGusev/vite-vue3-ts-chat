<template>
  <ul class="messages-list">
    <li
      v-for="(message, i) in list"
      :key="i"
      :class="['message', !message.inbox && '_sent']"
    >
      {{ message.value }}
      <span class="time">
        {{ message.time }}
      </span>
    </li>
  </ul>
</template>

<script setup lang="ts">
defineProps<{
  list: []
}>();
</script>

<style lang="scss" scoped>
.messages-list {
  overflow-y: auto;
  padding: 5px;

  .message {
    position: relative;
    width: 48%;
    padding: 10px 15px 20px;
    box-shadow: 0 0 3px #444;
    border-radius: 10px;
    background: #fff;
    font-size: 14px;

    &:not(:last-child) {
      margin-bottom: 15px;
    }

    .time {
      position: absolute;
      right: 10px;
      bottom: 3px;
      font-size: 12px;
      font-style: italic;
      color: grey;
    }

    &._sent {
      margin-left: auto;
      background: rgb(0, 126, 204);
      color: #fff;

      .time {
        color: #ffffff90;
      }
    }
  }
}
</style>
