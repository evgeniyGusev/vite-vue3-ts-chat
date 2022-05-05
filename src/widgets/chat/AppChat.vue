<script setup lang="ts">
import { reactive, computed, ComputedRef } from 'vue';
import { Contact } from '../../types/interfaces';
import { EmptyMessage, ContactStatus } from '../../types/types';
// Components
import { SendMessage } from '../../features/send-message';
import { MessagesList } from '../../features/messages-list';

interface State {
  isContactTyping: boolean,
}

const props = defineProps<{
  checkedContact?: Contact | null
}>();

const emit = defineEmits(['send-message'])

//STATE
const data: State = reactive({
  message: '',
  isContactTyping: false,
});

//COMPUTED
const getIsListVisible: ComputedRef<number | undefined> = computed(() => props.checkedContact?.messages?.length);
const getEmptyMessage: ComputedRef<EmptyMessage> = computed(() => props.checkedContact && (!props.checkedContact.messages || !props.checkedContact.messages.length) ? 'Отправьте первое сообщение' : '⇽ Выберите, кому хотите написать');
const getContactStatus: ComputedRef<ContactStatus> = computed(() => data.isContactTyping ? 'Печатает...' : (props.checkedContact?.isOnline ? 'Онлайн' : 'Был в сети недавно'));

//methods
const sendMessage = (message: string): void => {
  if (message.length) {
    let date = new Date();

    emit('send-message', { value: message, time: date.toLocaleTimeString() });

    if (props.checkedContact?.isOnline) {
      setTimeout(() => {
        data.isContactTyping = true;

        setTimeout(() => {
          date = new Date();

          emit('send-message', {
            inbox: true,
            time: date.toLocaleTimeString(),
            value: 'Извини, отвечу позже :(',
          });
          data.isContactTyping = false;
        }, 1500);
      }, 700);
    }
  }
}
</script>

<template>
  <div class="app-chat">
    <section v-if="checkedContact" class="head">
      <img :src="checkedContact.avatar" :alt="checkedContact.name" class="avatar">
      <div class="contact-data">
        <div class="name">
          {{ checkedContact.name }}
        </div>
        <div class="status">
          {{ getContactStatus }}
        </div>
      </div>
    </section>

    <section class="body">
      <div v-if="!getIsListVisible" class="not-checked">
        {{ getEmptyMessage }}
      </div>

      <MessagesList v-else :list="checkedContact.messages" />
    </section>

    <section v-if="checkedContact" class="footer">
      <SendMessage @send-message="sendMessage" />
    </section>
  </div>
</template>

<style lang="scss" scoped>
.app-chat {
  flex-basis: 70%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
  padding: 10px;

  .head {
    display: flex;
    align-items: center;
    padding-bottom: 10px;
    border-bottom: 1px solid #dadada;

    .avatar {
      margin-right: 10px;
      border-radius: 50%;
    }

    .contact-data {
      .name {
        margin-bottom: 7px;
      }

      .status {
        font-size: 12px;
        font-style: italic;
      }
    }
  }

  .body {
    overflow-y: hidden;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    padding-top: 10px;
    padding-bottom: 10px;

    .not-checked {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100%;
      font-size: 2em;
    }
  }

  .footer {
    padding: 7px;
    border-top: 1px solid #dadada;
  }
}
</style>
