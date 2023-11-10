<script setup>
import HeadlessAutocompleteMultiple from "@/components/HeadlessAutocompleteMultiple.vue"
// import { CITIES } from "@/components/cities"
</script>

<template>
  <div class="back flex justify-center items-center bg-gray-950 min-h-screen">
    <div class="card w-full max-w-md flex justify-center items-center p-12">
      <fieldset class="w-full max-w-xs flex flex-col gap-1 -mt-2">
        <label class="text-white">Combobox Multiple</label>
        <HeadlessAutocompleteMultiple />
      </fieldset>
    </div>
  </div>
</template>

<style>
.card {
  backdrop-filter: blur(1px) saturate(180%);
  -webkit-backdrop-filter: blur(1px) saturate(180%);
  background-color: rgba(17, 25, 40, 0.75);
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.125);
}
</style>
