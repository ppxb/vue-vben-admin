<script lang="ts" setup>
import { IconifyIcon } from '@vben-core/icons';

import { useFullscreen } from '@vueuse/core';

import { VbenIconButton } from '../button';

defineOptions({
  name: 'FullScreen',
});

const { isFullscreen, toggle } = useFullscreen();

// 重新检查全屏状态
isFullscreen.value = !!(
  document.fullscreenElement ||
  // @ts-ignore
  document.webkitFullscreenElement ||
  // @ts-ignore
  document.mozFullScreenElement ||
  // @ts-ignore
  document.msFullscreenElement
);
</script>

<template>
  <VbenIconButton class="rounded-md" @click="toggle">
    <IconifyIcon v-if="isFullscreen" icon="lucide:minimize" />
    <IconifyIcon v-else icon="lucide:maximize" />
  </VbenIconButton>
</template>
