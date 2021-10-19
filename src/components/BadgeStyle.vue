<script setup lang="ts">
import { Listbox, ListboxButton, ListboxOption, ListboxOptions } from '@headlessui/vue'
import { CheckIcon, SelectorIcon } from '@heroicons/vue/solid'
import { computed, ref } from 'vue'

const styles = ref([
  { id: 'for-the-badge', name: 'for-the-badge' },
  { id: 'flat', name: 'flat' },
  { id: 'flat-square', name: 'flat-square' },
  { id: 'plastic', name: 'plastic' },
  { id: 'popout', name: 'popout' },
  { id: 'popout-square', name: 'popout-square' },
  { id: 'social', name: 'social' }
])
const selected = ref(styles.value[2])
const props = defineProps({
  parent_style: Object,
})
const emit = defineEmits(['change'])

const currentStyle = computed<{name: string; id: string}>({

  get() {
    return props.parent_style
  },
  set(style) {
    emit('change', style)
  }

})

</script>

<!-- This example requires Tailwind CSS v2.0+ -->
<template>
  <Listbox v-model="currentStyle" as="div">
    <div class="mt-1 relative">
      <ListboxButton class="bg-white relative w-full border border-gray-300 rounded-md shadow-sm pl-3 pr-10 py-2 text-left cursor-default focus:outline-none focus:ring-1 focus:ring-orange-500 focus:border-orange-500 sm:text-sm">
        <span class="block truncate">{{ currentStyle.name }}</span>
        <span class="absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
          <SelectorIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
        </span>
      </ListboxButton>

      <transition leave-active-class="transition ease-in duration-100" leave-from-class="opacity-100" leave-to-class="opacity-0">
        <ListboxOptions class="absolute z-10 mt-1 w-full bg-white shadow-lg max-h-60 rounded-md py-1 text-base ring-1 ring-black ring-opacity-5 overflow-auto focus:outline-none sm:text-sm ">
          <ListboxOption
            v-for="style in styles"
            :key="style.name"
            v-slot="{ active, selected }"
            as="template"
            :value="style"
          >
            <li style="list-style:none" :class="[active ? 'text-white bg-orange-500' : 'text-gray-900', 'text-left -ml-4 list-style-none cursor-default select-none relative py-2 pl-2 pr-9']">
              <span :class="[selected ? 'font-semibold' : 'font-normal', 'block truncate']">
                {{ style.name }}
              </span>

              <span v-if="selected" :class="[active ? 'text-white' : 'text-orange-600', 'absolute inset-y-0 left-36 flex items-center pl-1.5']">
                <CheckIcon class="h-5 w-5" aria-hidden="true" />
              </span>
            </li>
          </ListboxOption>
        </ListboxOptions>
      </transition>
    </div>
  </Listbox>
</template>
