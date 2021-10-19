<template>
  <Header />
  <main style="flex:1" class="px-4 py-1 text-center text-gray-700 dark:text-gray-200">
    <router-view v-bind="$attrs" />
  </main>
  <Footer />
</template>
