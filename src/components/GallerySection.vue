<template>
  <section class="py-24 px-6 md:px-12 max-w-7xl mx-auto border-b border-gray-800" id="gallery-section">
    <div class="text-center mb-16 gallery-header opacity-0 translate-y-10">
      <h2 class="text-4xl md:text-5xl font-black mb-4 uppercase tracking-tighter">C8 GALLERY</h2>
      <p class="text-gray-500 uppercase tracking-widest text-sm">Visual Logs & Memes</p>
    </div>

    <div class="gallery-content opacity-0 scale-95 flex justify-center">
      <AccordionGallery
        :items="galleryItems"
        :defaultIndex="2"
        :expandRatio="0.52"
        trigger="hover"
        accentColor="#ffffff"
        overlayColor="#000000"
        textColor="#ffffff"
        :grayscale="true"
        :showLabels="true"
        :duration="0.6"
        ease="power3.out"
        :parallax="0.5"
        :tilt="8"
        :stagger="0.06"
        :height="460"
        :gap="10"
        :radius="16"
        orientation="horizontal"
      />
    </div>
  </section>
</template>

<script setup>
import { onMounted } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'
import AccordionGallery from './AccordionGallery.vue'

const galleryItems = [
  { image: 'https://picsum.photos/id/1015/900/1200', label: 'C8OSHI V1', link: '#' },
  { image: 'https://picsum.photos/id/1018/900/1200', label: 'N8WORK', link: '#' },
  { image: 'https://picsum.photos/id/1039/900/1200', label: '8RMY RISE', link: '#' },
  { image: 'https://picsum.photos/id/1043/900/1200', label: 'UNDERC8VER', link: '#' },
  { image: 'https://picsum.photos/id/1044/900/1200', label: 'THE CULT', link: '#' }
]

onMounted(() => {
  gsap.to('.gallery-header', {
    scrollTrigger: { trigger: '#gallery-section', start: 'top 80%' },
    y: 0, opacity: 1, duration: 1, ease: 'power3.out'
  })

  gsap.to('.gallery-content', {
    scrollTrigger: { trigger: '#gallery-section', start: 'top 70%' },
    scale: 1, opacity: 1, duration: 0.8, ease: 'power3.out'
  })
})
</script>
