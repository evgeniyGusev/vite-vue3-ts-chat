<script setup lang="ts">
import { Contact } from '../../types/interfaces';
import { ContactsList } from '../../features/contacts-list';

defineProps<{
  contacts?: Contact[]
}>();

const emit = defineEmits(['set-contact'])

const setContact = (contact: Contact): void => {
  emit('set-contact', contact);
}
</script>

<template>
  <div class="app-contacts">
    <h3 class="title">
      Контакты
    </h3>

    <ContactsList
      v-if="contacts.length"
      :list="contacts"
      @set-contact="setContact"
    />

    <div v-else class="empty-contact-list">
      Нет контактов
    </div>
  </div>
</template>

<style lang="scss" scoped>
.app-contacts {
  overflow-y: auto;
  flex-basis: 30%;
  height: 100%;
  padding: 10px;
  border-radius: 10px 0 0 10px;
  box-shadow: 1px 0 3px #444;
  border-right: 1px solid #f6f6f6;

  .title {
    margin-bottom: 10px;
    padding-bottom: 10px;
    border-bottom: 1px solid #999;
  }
}
</style>
