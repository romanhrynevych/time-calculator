<script setup lang="ts">
const expression = ref('')
const result = computed(() => {
  try {
    // Define a regex pattern to match time durations
    const timeRegex = /\b(?:(\d+)\s*h(?:our)?(?:s)?(?:\s+|$))?\s*(?:(\d+)\s*min(?:ute)?(?:s)?(?:\s+|$))?|\b(\d+)\s*m(?:in(?:ute)?(?:s)?)?\b/g
    const matches = [...expression.value.matchAll(timeRegex)]
    // Initialize total duration in minutes
    let totalMinutes = 0

    // Iterate over all matches using matchAll
    for (const match of matches) {
      const hours = match[1] ? Number.parseInt(match[1]) : 0
      const minutes = match[2] ? Number.parseInt(match[2]) : 0
      totalMinutes += hours * 60 + minutes
    }

    // Calculate total hours and minutes
    const totalHours = Math.floor(totalMinutes / 60)
    const remainingMinutes = totalMinutes % 60

    return `${totalHours}h ${remainingMinutes}min`
  }
  catch (e) {
    return '👋'
  }
})
</script>

<template>
  <div class="container grid gap-8">
    <h1 class="text-4xl font-bold">
      ETA Calculator
    </h1>

    <Textarea v-model="expression" class="min-h-96" />

    <span>Total: {{ result }}</span>
  </div>
</template>
