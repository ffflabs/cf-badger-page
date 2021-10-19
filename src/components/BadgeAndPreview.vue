<script setup lang="ts">

const props = defineProps({
  href: String,
  src: String
})

</script>
<template>
  <form-line>
    <div class="col-span-6 sm:col-span-3">
      <label
        class="block text-md  text-left text-gray-800 cursor-default"
      >Badge Style</label>

      <div class="mb-0 ">
        <slot></slot>
      </div>
    </div>
    <div class="col-span-6 sm:col-span-3">
      <label
        class="block text-md  text-left text-gray-800 cursor-default"
      >Preview your badge</label>
      <div
        class="clear-both relative text-base text-left box-border p-1"
      >
        <div class="mb-0">
          <a
            :href="props.href"
            target="_blank"
            class="text-orange-600 no-underline cursor-pointer hover:text-gray-800"
          >
            <img
              alt="Build Status"
              :src="props.src"
              class="max-w-full h-auto"
            />
          </a>
        </div>
      </div>
    </div>
  </form-line>
</template>
