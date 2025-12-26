<script setup lang="ts">
import type { AboutProps, DescriptionItem } from './about';

import { computed, h } from 'vue';

import {
  VBEN_DOC_URL,
  VBEN_GITHUB_URL,
  VBEN_PREVIEW_URL,
} from '@vben/constants';

import { VbenRenderContent } from '@vben-core/shadcn-ui';

import { Page } from '../../components';

interface Props extends AboutProps {}

defineOptions({
  name: 'AboutUI',
});

withDefaults(defineProps<Props>(), {
  description:
    '是一个现代化开箱即用的中后台解决方案，采用最新的技术栈，包括 Vue 3.0、Vite、TailwindCSS 和 TypeScript 等前沿技术，代码规范严谨，提供丰富的配置选项，旨在为中大型项目的开发提供现成的开箱即用解决方案及丰富的示例，同时，它也是学习和深入前端技术的一个极佳示例。',
  name: 'Vben Admin',
  title: '关于项目',
});

declare global {
  const __VBEN_ADMIN_METADATA__: {
    authorEmail: string;
    authorName: string;
    authorUrl: string;
    buildTime: string;
    dependencies: Record<string, string>;
    description: string;
    devDependencies: Record<string, string>;
    homepage: string;
    license: string;
    repositoryUrl: string;
    version: string;
  };
}

const renderLink = (href: string, text: string) =>
  h(
    'a',
    { href, target: '_blank', class: 'vben-link' },
    { default: () => text },
  );

const {
  authorName,
  authorUrl,
  buildTime,
  dependencies = {},
  devDependencies = {},
  homepage,
  license,
  version,
  // vite inject-metadata 插件注入的全局变量
} = __VBEN_ADMIN_METADATA__ || {};

const vbenBasicDescriptionItems: DescriptionItem[] = [
  {
    content: version,
    title: '版本号',
  },
  {
    content: license,
    title: '开源许可协议',
  },
  {
    content: buildTime,
    title: '最后构建时间',
  },
  {
    content: renderLink(authorUrl, `@${authorName}`),
    title: '作者',
  },
];

const vbenLinkDescriptionItems: DescriptionItem[] = [
  {
    content: renderLink(homepage, '点击查看'),
    title: '主页',
  },
  {
    content: renderLink(VBEN_DOC_URL, '点击查看'),
    title: '文档地址',
  },
  {
    content: renderLink(VBEN_PREVIEW_URL, '点击查看'),
    title: '预览地址',
  },
  {
    content: renderLink(VBEN_GITHUB_URL, '点击查看'),
    title: 'Github',
  },
];

const dependenciesItems = computed(() =>
  Object.entries(dependencies).map(([title, content]) => ({ title, content })),
);

const devDependenciesItems = computed(() =>
  Object.entries(devDependencies).map(([title, content]) => ({
    title,
    content,
  })),
);
</script>

<template>
  <Page :title="title">
    <template #description>
      <p class="text-muted-foreground text-lg leading-relaxed">
        <a
          :href="VBEN_GITHUB_URL"
          class="vben-link hover:underline"
          target="_blank"
        >
          {{ name }}
        </a>
        {{ description }}
      </p>
    </template>

    <div
      class="mb-6 grid grid-cols-1 gap-4 sm:grid-cols-2 lg:mb-8 lg:grid-cols-4"
    >
      <div
        v-for="item in vbenBasicDescriptionItems"
        :key="item.title"
        class="border-border bg-card rounded-xl border"
      >
        <div class="flex h-full flex-col justify-between p-5 sm:p-6">
          <div
            class="text-muted-foreground mb-3 text-xs font-semibold uppercase tracking-widest"
          >
            {{ item.title }}
          </div>
          <span class="text-2xl font-bold">
            <VbenRenderContent :content="item.content" />
          </span>
        </div>
      </div>
    </div>

    <div class="border-border bg-card mb-6 rounded-xl border lg:mb-8">
      <div class="p-5 sm:p-6 lg:p-8">
        <div class="mb-6">
          <h2 class="text-foreground text-2xl font-bold sm:text-3xl">
            项目链接
          </h2>
          <p class="text-muted-foreground mt-2 text-sm">访问项目相关资源</p>
        </div>
        <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-4">
          <div
            v-for="item in vbenLinkDescriptionItems"
            :key="item.title"
            class="border-border hover:border-primary/50 rounded-xl border transition-all hover:cursor-pointer"
          >
            <div class="flex h-full flex-col justify-between p-5">
              <div
                class="text-muted-foreground mb-3 text-xs font-semibold uppercase tracking-widest"
              >
                {{ item.title }}
              </div>
              <span class="text-xl font-bold">
                <VbenRenderContent :content="item.content" />
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <template
      v-for="{ title, desc, items } in [
        {
          title: '生产环境依赖',
          desc: '项目生产核心依赖包及版本信息',
          items: dependenciesItems,
        },
        {
          title: '开发环境依赖',
          desc: '项目开发核心依赖包及版本信息',
          items: devDependenciesItems,
        },
      ]"
      :key="title"
    >
      <div
        class="border-border bg-card mb-6 rounded-xl border last:mb-0 lg:mb-8"
      >
        <div class="p-5 sm:p-6 lg:p-8">
          <div class="mb-6">
            <h2 class="text-foreground text-2xl font-bold sm:text-3xl">
              {{ title }}
            </h2>
            <p class="text-muted-foreground mt-2 text-sm">{{ desc }}</p>
          </div>
          <div
            class="grid grid-cols-1 gap-3 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4"
          >
            <div
              v-for="item in items"
              :key="item.title"
              class="border-border flex flex-col gap-2 rounded-lg border p-4"
            >
              <div
                class="text-foreground truncate text-sm font-semibold"
                :title="item.title"
              >
                {{ item.title }}
              </div>
              <div
                class="bg-muted/50 text-muted-foreground w-fit rounded-md px-2.5 py-1 text-xs"
              >
                <VbenRenderContent :content="item.content" />
              </div>
            </div>
          </div>
        </div>
      </div>
    </template>
  </Page>
</template>
