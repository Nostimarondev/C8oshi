<template>
  <section class="py-24 px-6 md:px-12 max-w-5xl mx-auto border-b border-gray-800" id="links-section">
    <div class="text-center mb-16 links-header opacity-0 translate-y-10">
      <h2 class="text-4xl md:text-5xl font-black mb-4 uppercase tracking-tighter">C8 LINKS</h2>
      <p class="text-gray-500 uppercase tracking-widest text-sm">Connect to the Network</p>
    </div>

    <!-- Contract Address Bar (Large Horizontal) -->
    <div class="ca-bar opacity-0 translate-y-10 mb-12 group cursor-pointer" @click="copyToClipboard">
      <div class="border-2 border-white p-6 relative overflow-hidden flex flex-col md:flex-row items-center justify-between hover:bg-white hover:text-black transition-colors duration-300">
        <div class="flex items-center gap-4 mb-4 md:mb-0">
          <span class="uppercase font-bold tracking-widest text-sm md:text-base">Contract</span>
          <span class="w-2 h-2 bg-current rounded-full animate-pulse"></span>
        </div>
        <div class="font-mono text-sm md:text-xl font-bold truncate max-w-full">
          {{ contractAddress }}
        </div>
        <div class="absolute inset-0 bg-white text-black flex items-center justify-center font-black tracking-widest uppercase text-xl translate-y-full group-hover:translate-y-0 transition-transform duration-300">
          {{ copied ? 'COPIED TO CLIPBOARD' : 'CLICK TO COPY' }}
        </div>
      </div>
    </div>

    <!-- Social Links Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 social-links">
      <SpecularButton
        v-for="link in socialLinks"
        :key="link.name"
        :href="link.url"
        target="_blank"
        customClass="!h-32 group"
        size="lg"
        :radius="20"
        tint="#ffffff"
        :tintOpacity="0"
        :blur="10"
        textColor="#f5f5f5"
        lineColor="#ffffff"
        baseColor="#525252"
        :intensity="1"
        :shineSize="25"
        :shineFade="40"
        :thickness="1"
        :speed="0.15"
        :followMouse="true"
        :proximity="250"
        :autoAnimate="true"
      >
        <span class="text-2xl font-black uppercase tracking-wider group-hover:scale-110 transition-transform duration-300">{{ link.name }}</span>
        <span class="text-xs text-gray-500 tracking-widest uppercase group-hover:text-white transition-colors duration-300">Access</span>
      </SpecularButton>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'
import SpecularButton from './SpecularButton.vue'

const contractAddress = "0x0000000000C8OSHI0000000000NAKAM8TO0000"
const copied = ref(false)

const socialLinks = [
  { name: 'Dexscreener', url: '#' },
  { name: 'Telegram', url: '#' },
  { name: 'X', url: '#' },
  { name: 'c8ntinuum', url: 'https://c8ntinuum.com' },
]

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(contractAddress)
    copied.value = true
    setTimeout(() => { copied.value = false }, 2000)
  } catch (err) {
    console.error('Failed to copy', err)
  }
}

onMounted(() => {
  gsap.to('.links-header', {
    scrollTrigger: { trigger: '#links-section', start: 'top 80%' },
    y: 0, opacity: 1, duration: 1, ease: 'power3.out'
  })

  gsap.to('.ca-bar', {
    scrollTrigger: { trigger: '#links-section', start: 'top 75%' },
    y: 0, opacity: 1, duration: 1, delay: 0.2, ease: 'power3.out'
  })

  gsap.from('.social-btn', {
    scrollTrigger: { trigger: '.social-links', start: 'top 80%' },
    y: 50, opacity: 0, duration: 0.8, stagger: 0.1, ease: 'power3.out'
  })
})
</script>
