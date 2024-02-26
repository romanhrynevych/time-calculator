<script setup lang="ts">
import { Input } from '~/components/ui/input'

function secondsToHms(d: number): string {
  const h = Math.floor(d / 3600)
  const m = Math.floor(d % 3600 / 60)
  const s = Math.floor(d % 3600 % 60)

  const hDisplay = h > 0 ? h + (h === 1 ? ' hour ' : ' hours ') : ''
  const mDisplay = m > 0 ? m + (m === 1 ? ' minute ' : ' minutes ') : ''
  const sDisplay = s > 0 ? s + (s === 1 ? ' second' : ' seconds') : ''
  return hDisplay + mDisplay + sDisplay
}

const expression = ref('')
const result = computed(() => {
  try {
    return secondsToHms(evaluate(expression.value) * 60)
  }
  catch (e) {
    return '👋'
  }
})

function evaluate(expr: string): number {
  const regex = /(\d+\s*(?:y(?:ear)?|mo(?:nth)?|w(?:eek)?|d(?:ay)?|h(?:our)?|m(?:in(?:ute)?|)|s(?:ec(?:ond)?|))|\+|-|\*|\/)/g
  const tokens = expr.match(regex)

  if (!tokens)
    throw new Error('Invalid expression')

  let totalMinutes = 0
  let operator = '+'

  for (const token of tokens) {
    if (token.match(/\d+\s*(?:y(?:ear)?|mo(?:nth)?|w(?:eek)?|d(?:ay)?|h(?:our)?|m(?:in(?:ute)?|)|s(?:ec(?:ond)?|))/)) {
      const [quantity, unit] = token.split(/(?<=\d)\s*(?=[a-z])/i)
      totalMinutes = applyOperation(totalMinutes, convertToMinutes(Number.parseInt(quantity), unit), operator)
    }
    else {
      operator = token
    }
  }

  return totalMinutes
}

function convertToMinutes(quantity: number, unit: string): number {
  const conversionFactors: Record<string, number> = {
    year: 525600,
    y: 525600,
    month: 43800,
    mo: 43800,
    week: 10080,
    w: 10080,
    day: 1440,
    d: 1440,
    hour: 60,
    h: 60,
    minute: 1,
    min: 1,
    m: 1,
    second: 1 / 60,
    sec: 1 / 60,
    s: 1 / 60,
  }
  return quantity * conversionFactors[unit]
}

function applyOperation(a: number, b: number, operator: string): number {
  switch (operator) {
    case '+':
      return a + b
    case '-':
      return a - b
    case '*':
      return a * b
    case '/':
      if (b === 0)
        throw new Error('Division by zero')
      return a / b
    default:
      throw new Error('Invalid operator')
  }
}
</script>

<template>
  <div class="container grid gap-8">
    <h1 class="text-4xl font-bold">
      Time Calculator
    </h1>

    <div class="flex flex-col gap-4">
      <Input v-model="expression" />

      <div>
        Sum: {{ result }}
      </div>
    </div>
  </div>
</template>
