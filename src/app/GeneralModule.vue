<script setup lang="ts">
import { reactive } from 'vue';
import { Contact } from '../types/interfaces';
import mockMessages from './messages';
// components
import { AppContacts } from '../widgets/contacts';
import { AppChat } from '../widgets/chat';

interface State {
  contacts: Contact[] | [],
  checkedContact: Contact | null,
}

// STATE
const data: State = reactive({
  contacts: [],
  checkedContact: null,
});

// METHODS
const setCheckedContact = (contact: Contact): void => {
  data.checkedContact = contact;
}
const pushMessage = (message: string): void => {
  data?.checkedContact?.messages?.push(message);
}

async function fetchContacts() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');

    if (response.ok) {
      const contacts = await response.json();

      data.contacts = contacts
      .map(({ id, name, username }: Contact): Contact => ({
        id,
        name,
        username,
        avatar: 'https://placekitten.com/g/35/35',
        isOnline: Math.random() >= 0.5,
        messages: new Array((Math.floor(Math.random() * 7)))
        .fill(undefined)
        .map(() => ({
          inbox: Math.random() >= 0.3,
          time: 'Вчера',
          value: mockMessages[Math.floor(Math.random() * mockMessages.length)],
        })),
      }));
    }
  } catch {}
}

// LOGIC
fetchContacts();
</script>

<template>
  <div class="app">
    <app-contacts
      :contacts="data.contacts"
      @set-contact="setCheckedContact"
    />
    <app-chat
      :checked-contact="data.checkedContact"
      @send-message="pushMessage"
    />
  </div>
</template>
