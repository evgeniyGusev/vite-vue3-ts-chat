<script setup lang="ts">
import { reactive } from 'vue';
import AppContacts from 'components/AppContacts.vue';
import AppChat from 'components/AppChat.vue';
import mockMessages from './assets/messages';

interface Contact {
  id?: number,
  name?: string,
  username?: string,
  avatar?: string,
  isOnline?: boolean,
  messages?: any[],
}

interface State {
  contacts: Contact[] | [],
  checkedContact: Contact | null,
  isLoadContactsError: boolean,
}

// STATE
const data: State = reactive({
  contacts: [],
  checkedContact: null,
  isLoadContactsError: false,
});

// METHODS
const setCheckedContact = (contact: Contact): void => {
  data.checkedContact = contact;
}
const pushMessage = (message: string): void => {
  data?.checkedContact?.messages?.push(message);
}

async function fetchContacts() {
  data.isLoadContactsError = false;

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
    } else {
      data.isLoadContactsError = true;
    }
  } catch {
    data.isLoadContactsError = true;
  }
}

// LOGIC
fetchContacts();
</script>

<template>
  <h1>жопа</h1>
</template>
