<script setup lang="ts">
import { RouterView, useRoute } from 'vue-router'
import { useHead } from '@unhead/vue'
import { computed } from 'vue'
import ErrorBoundary from '@/components/ErrorBoundary.vue'

const route = useRoute()

const SITE_NAME = 'vibe.j2team.org'
const SITE_URL = 'https://vibe.j2team.org'
const DEFAULT_TITLE = `${SITE_NAME} - J2TEAM Community Vibe Coding`
const DEFAULT_DESCRIPTION =
  'Cả nhóm J2TEAM Community vibe code cùng nhau! Mỗi thành viên tạo một trang con, vibe code thoải mái.'

const title = computed(() => route.meta.title || DEFAULT_TITLE)
const description = computed(() => route.meta.description || DEFAULT_DESCRIPTION)
const canonicalUrl = computed(() => `${SITE_URL}${route.path}`)
const author = computed(() => route.meta.author)

useHead(
  computed(() => ({
    title: title.value,
    link: [{ rel: 'canonical', href: canonicalUrl.value }],
    meta: [
      { name: 'description', content: description.value },
      ...(author.value ? [{ name: 'author', content: author.value }] : []),
      { property: 'og:title', content: title.value },
      { property: 'og:description', content: description.value },
      { property: 'og:url', content: canonicalUrl.value },
      { property: 'og:type', content: 'website' },
      { property: 'og:site_name', content: SITE_NAME },
      { name: 'twitter:card', content: 'summary' },
      { name: 'twitter:title', content: title.value },
      { name: 'twitter:description', content: description.value },
    ],
  })),
)
</script>

<template>
  <RouterView v-slot="{ Component }">
    <ErrorBoundary>
      <component :is="Component" />
    </ErrorBoundary>
  </RouterView>
</template>
