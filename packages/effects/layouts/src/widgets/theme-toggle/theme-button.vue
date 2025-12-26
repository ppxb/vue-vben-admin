<script lang="ts" setup>
import { computed, nextTick } from 'vue';

import { IconifyIcon } from '@vben/icons';

import { VbenButton } from '@vben-core/shadcn-ui';

interface Props {
  /**
   * 类型
   */
  type?: 'icon' | 'normal';
}

defineOptions({
  name: 'ThemeToggleButton',
});

const props = withDefaults(defineProps<Props>(), {
  type: 'normal',
});

const isDark = defineModel<boolean>();

const theme = computed(() => {
  return isDark.value ? 'light' : 'dark';
});

const bindProps = computed(() => {
  const type = props.type;

  return type === 'normal'
    ? {
        variant: 'heavy' as const,
      }
    : {
        class: 'rounded-md',
        size: 'icon' as const,
        style: { padding: '7px' },
        variant: 'icon' as const,
      };
});

function toggleTheme(event: MouseEvent) {
  const isAppearanceTransition =
    // @ts-expect-error experimental API
    document.startViewTransition &&
    !window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  if (!isAppearanceTransition || !event) {
    isDark.value = !isDark.value;
    return;
  }

  const x = event.clientX;
  const y = event.clientY;
  const endRadius = Math.hypot(
    Math.max(x, innerWidth - x),
    Math.max(y, innerHeight - y),
  );
  const transition = document.startViewTransition(async () => {
    isDark.value = !isDark.value;
    await nextTick();
  });

  transition.ready.then(() => {
    const clipPath = [
      `circle(0px at ${x}px ${y}px)`,
      `circle(${endRadius}px at ${x}px ${y}px)`,
    ];
    const animate = document.documentElement.animate(
      {
        clipPath: isDark.value ? [...clipPath].toReversed() : clipPath,
      },
      {
        duration: 450,
        easing: 'ease-in',
        pseudoElement: isDark.value
          ? '::view-transition-old(root)'
          : '::view-transition-new(root)',
      },
    );
    animate.onfinish = () => {
      transition.skipTransition();
    };
  });
}
</script>

<template>
  <VbenButton
    :aria-label="theme"
    aria-live="polite"
    v-bind="bindProps"
    @click.stop="toggleTheme"
  >
    <IconifyIcon v-if="isDark" icon="lucide:sun" />
    <IconifyIcon v-else icon="lucide:moon" />
  </VbenButton>
</template>
