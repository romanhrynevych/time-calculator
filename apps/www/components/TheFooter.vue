<script setup lang="ts">
import { HeartIcon } from '@heroicons/vue/24/solid'
</script>

<template>
  <footer class="border-border bg-background mt-12 border-t py-6">
    <div class="container mx-auto flex justify-between">
      <p>© {{ new Date().getFullYear() }} Hrynevych Roman. All rights reserved.</p>

      <div class="flex items-center gap-2">
        Made with
        <HeartIcon class="size-5 fill-red-600" />
      </div>
    </div>
  </footer>
</template>
