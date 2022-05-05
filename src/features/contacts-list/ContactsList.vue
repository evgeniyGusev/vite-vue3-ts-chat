<template>
  <ul class="contacts-list">
    <li
      v-for="contact in list"
      :key="contact.id"
      :class="['contacts-item', contact.name === checkedContactName && '_checked', contact.isOnline && '_online']"
      @click="contactHandler(contact)"
    >
      <img :src="contact.avatar" :alt="contact.name" class="avatar">
      <div class="contact-data">
        <div class="name">
          {{ contact.name }}
        </div>
        <div class="last-message">
          {{ contact?.messages?.length ? contact.messages.at(-1).value : 'Нет сообщений' }}
        </div>
      </div>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { ref, Ref } from 'vue';
import { Contact } from '../../types/interfaces';

defineProps<{
  list: Contact[]
}>();

const emit = defineEmits(['set-contact'])
const checkedContactName: Ref<string | undefined> = ref('');

const contactHandler = (contact: Contact): void => {
  checkedContactName.value = contact.name;
  emit('set-contact', contact);
}
</script>

<style lang="scss" scoped>
.contacts-list {
  .contacts-item {
    position: relative;
    display: flex;
    align-items: flex-start;
    padding: 10px 0;
    border-bottom: 1px solid #dadada;
    cursor: pointer;

    &._online::after {
      content: "";
      position: absolute;
      top: 10px;
      left: 0;
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: #00a900;
    }

    &._checked {
      background: #dadada;
    }

    &:hover {
      transform: translateX(3px);
      background: #f6f6f6;
      box-shadow: 0 2px 2px #dadada;
    }

    .avatar {
      position: relative;
      margin-right: 7px;
      border-radius: 50%;
    }

    .contact-data {
      max-width: calc(100% - 42px);

      .name {
        margin-bottom: 3px;
        font-size: 14px;
        font-weight: 600;
      }

      .last-message {
        overflow: hidden;
        font-size: 12px;
        font-style: italic;
        white-space: nowrap;
        text-overflow: ellipsis;
      }
    }
  }
}
</style>
