<script setup lang="ts">
import {
  ComputerDesktopIcon,
  MoonIcon,
  SunIcon,
} from '@heroicons/vue/24/solid'

import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '~/components/ui/dropdown-menu'

const colorMode = useColorMode()

type ColorMode = 'system' | 'light' | 'dark'

function handleColorModeChange(type: ColorMode) {
  colorMode.preference = type
}
</script>

<template>
  <header
    class="border-b-border sticky top-0 z-50 mb-12 flex w-full items-center justify-between border-b p-4 backdrop-blur-2xl"
  >
    <NuxtLink to="/">
      <Logo class="text-primary max-h-8 max-w-fit sm:max-h-10" />
    </NuxtLink>

    <div class="flex gap-4">
      <NuxtLink to="/eta-calc" class="text-sm font-medium text-foreground ">
        ETA Calc
      </NuxtLink>
    </div>

    <div class="flex items-center gap-4">
      <DropdownMenu>
        <DropdownMenuTrigger as-child>
          <Button variant="ghost" size="icon" class="transition-none">
            <ComputerDesktopIcon v-if="colorMode.preference === 'system'" class="size-5" />
            <MoonIcon v-else-if="colorMode.preference === 'dark'" class="size-5" />
            <SunIcon v-else class="size-5" />
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent class="w-36">
          <DropdownMenuGroup>
            <DropdownMenuItem @click="handleColorModeChange('system')">
              <ComputerDesktopIcon class="me-2 size-4" /> System
            </DropdownMenuItem>
            <DropdownMenuItem @click="handleColorModeChange('light')">
              <SunIcon class="me-2 size-4" /> Light
            </DropdownMenuItem>
            <DropdownMenuItem @click="handleColorModeChange('dark')">
              <MoonIcon class="me-2 size-4" /> Dark
            </DropdownMenuItem>
          </DropdownMenuGroup>
        </DropdownMenuContent>
      </DropdownMenu>
    </div>
  </header>
</template>
