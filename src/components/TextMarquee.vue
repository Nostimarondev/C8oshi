<template>
  <div class="overflow-hidden whitespace-nowrap bg-white text-black py-4 border-y-4 border-white select-none w-full" :class="customClass">
    <div class="inline-block animate-[marquee_60s_linear_infinite] will-change-transform">
      <span v-for="n in 10" :key="n" class="mx-8 text-2xl font-black uppercase tracking-widest">{{ text }}</span>
    </div>
    <!-- Duplicate for seamless loop -->
    <div class="inline-block animate-[marquee_60s_linear_infinite] will-change-transform" aria-hidden="true">
      <span v-for="n in 10" :key="`dup-${n}`" class="mx-8 text-2xl font-black uppercase tracking-widest">{{ text }}</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  text: {
    type: String,
    required: true
  },
  customClass: {
    type: String,
    default: ''
  }
})
</script>
