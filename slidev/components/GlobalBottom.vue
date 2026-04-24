<script setup>
import { useNav } from '@slidev/client'

const { currentPage, total } = useNav()
</script>

<template>
  <div class="fixed bottom-3 right-4 flex items-center gap-1.5 text-xs text-gray-500 select-none pointer-events-none z-50">
    <span class="font-mono tabular-nums">{{ currentPage }}</span>
    <span class="text-gray-700">/</span>
    <span class="font-mono tabular-nums text-gray-600">{{ total }}</span>
  </div>
</template>
