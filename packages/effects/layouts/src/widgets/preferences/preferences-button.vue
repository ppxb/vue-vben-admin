<script lang="ts" setup>
import { IconifyIcon } from '@vben/icons';

import { VbenIconButton } from '@vben-core/shadcn-ui';

import Preferences from './preferences.vue';

const emit = defineEmits<{ clearPreferencesAndLogout: [] }>();

function clearPreferencesAndLogout() {
  emit('clearPreferencesAndLogout');
}
</script>
<template>
  <Preferences @clear-preferences-and-logout="clearPreferencesAndLogout">
    <VbenIconButton class="rounded-md">
      <IconifyIcon icon="lucide:settings" />
    </VbenIconButton>
  </Preferences>
</template>
