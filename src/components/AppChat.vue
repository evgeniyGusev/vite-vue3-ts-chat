<script setup lang="ts">
import {reactive, computed, ComputedRef} from 'vue';
import { Contact } from '../types/interfaces';
import { EmptyMessage, ContactStatus } from '../types/types';

interface State {
  message: string
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
const sendMessage = (): void => {
  if (data.message) {
    let date = new Date();

    emit('send-message', { value: data.message, time: date.toLocaleTimeString() });
    data.message = '';

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

      <ul v-else class="messages">
        <li
            v-for="(message, i) in checkedContact?.messages"
            :key="i"
            :class="['message', !message.inbox && '_sent']"
        >
          {{ message.value }}
          <span class="time">
            {{ message.time }}
          </span>
        </li>
      </ul>
    </section>

    <section v-if="checkedContact" class="footer">
      <form
          class="chat-form"
          @submit.prevent="sendMessage"
      >
        <input v-model="data.message" type="text" placeholder="Введите сообщение">
        <button class="button">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M6.48 6.794l.49 1.963a1 1 0 0 1-1.94.486l-1-4a1 1 0 0 1 1.393-1.15l15 7a1 1 0 0 1 0 1.813l-15 7a1 1 0 0 1-1.385-1.18l2-7A1 1 0 0 1 7 11h4a1 1 0 0 1 0 2H7.754l-1.19 4.167L17.635 12 6.48 6.794z" fill="#000" fill-rule="nonzero"/></svg>
        </button>
      </form>
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

    .messages {
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
  }

  .footer {
    padding: 7px;
    border-top: 1px solid #dadada;

    .chat-form {
      display: flex;

      input {
        width: 100%;
        margin-right: 20px;
        padding: 0 10px;
        border: 1px solid #dadada;
        border-radius: 5px;
      }

      .button {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 40px;
        height: 30px;
        border-radius: 5px;
        background: #00d559;

        &:hover {
          background: #00a900;
        }
      }
    }
  }
}
</style>
