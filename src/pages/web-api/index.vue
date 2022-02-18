<script setup lang="ts">
const WebApiComponents = import.meta.globEager('../../components/web-api/*.vue')
const modules: Record<string, any> = reactive({})
for (const path in WebApiComponents) {
  modules[path] = WebApiComponents[path].default
}
</script>

<template lang="pug">
template(v-for="(module) in modules")
  component(:is="module")
</template>

<route lang="yaml">
meta:
  layout: home
</route>
