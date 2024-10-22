<script setup lang="ts">
import { computed, ref } from "vue";
import Iconsax from "@/components/iconsax/vue/VueIconsax.vue";
import { IconVariants } from "./types";
const search = ref("");
const iconType = ref<IconVariants>("outline");
const loading = ref(false);
function onChangeType(e: Event) {
  const target = <HTMLInputElement>e.target;
  loading.value = true;
  iconType.value = target.value as IconVariants;
  setTimeout(() => {
    loading.value = false;
  }, 2000);
}
</script>

<template>
  <main>
    <div class="text-center logo">
      <a href="https://iconsax.io/" target="_blank">
        <Iconsax icon="vuesax" size="120px" type="twotone" color="#ddd" />
      </a>
    </div>
    <div class="mb-5 text-center">
      <input v-model="search" placeholder="search vuesax icon ..." />
    </div>
    <div class="mb-5 text-center select-box">
      <select :value="iconType" @change="onChangeType">
        <option value="outline">outline</option>
        <option value="twotone">twotone</option>
        <option value="bold">bold</option>
        <option value="bulk">bulk</option>
      </select>
    </div>
    <div class="mb-5 text-center monospace">
      {{ `<Iconsax icon="convertshape" type="${iconType}" size="24px" color="#000" />` }}
    </div>
    <div class="grid" v-if="!loading">
      <div v-for="i in icons" :key="i" class="text-center">
        <p>
          <Iconsax :icon="i" :type="iconType" />
        </p>
        <p class="font-sm" style="color: grey">{{ i }}</p>
      </div>
    </div>
    <div class="loading" v-else>
      <Iconsax icon="vuesax" size="120px" type="twotone" />
    </div>
  </main>
</template>
