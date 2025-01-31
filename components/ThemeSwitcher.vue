<script setup lang="ts">
import { Sun, Moon, Laptop } from 'lucide-vue-next'

const colorMode = useColorMode()

type ThemeOption = 'light' | 'dark' | 'system'

const themeOptions: Array<{
  label: string
  value: ThemeOption
  icon?: typeof Sun | typeof Moon | typeof Laptop
}> = [
  { label: 'Light', value: 'light', icon: Sun },
  { label: 'Dark', value: 'dark', icon: Moon },
  { label: 'System', value: 'system', icon: Laptop }
]

const handleThemeChange = (theme: ThemeOption) => {
  colorMode.preference = theme
}

const currentIcon = computed(() => {
  switch (colorMode.value) {
    case 'light':
      return Sun
    case 'dark':
      return Moon
    default:
      return Laptop
  }
})
</script>

<template>
  <ClientOnly>
    <DropdownMenu>
      <DropdownMenuTrigger as-child>
        <Button 
          variant="outline" 
          size="icon"
          class="w-10 h-10 bg-white dark:bg-gray-800"
        >
          <component
            :is="currentIcon"
            class="h-[1.2rem] w-[1.2rem] transition-all"
          />
          <span class="sr-only">Toggle theme</span>
        </Button>
      </DropdownMenuTrigger>

      <DropdownMenuContent 
        align="end"
        class="w-32 bg-white dark:bg-gray-800"
      >
        <DropdownMenuItem
          v-for="option in themeOptions"
          :key="option.value"
          @click="handleThemeChange(option.value)"
          class="flex items-center gap-2 cursor-pointer hover:bg-gray-100 dark:hover:bg-gray-700 dark:text-white"
        >
          <component
            v-if="option.icon"
            :is="option.icon"
            class="h-4 w-4"
          />
          {{ option.label }}
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  </ClientOnly>
</template>
