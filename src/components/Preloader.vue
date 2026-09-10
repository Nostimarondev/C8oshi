<template>
  <div class="fixed inset-0 z-50 flex flex-col items-center justify-center bg-black text-white preloader-container">
    <div class="overflow-hidden mb-4">
      <div class="text-xs md:text-sm tracking-[0.5em] text-gray-500 font-bold uppercase preloader-text">
        Initializing Network
      </div>
    </div>
    
    <div class="overflow-hidden">
      <div class="text-7xl md:text-9xl font-black tracking-tighter preloader-counter">
        {{ progress }}
      </div>
    </div>
    
    <div class="w-48 md:w-64 h-[2px] bg-gray-900 mt-8 relative overflow-hidden preloader-bar-container">
      <div class="absolute left-0 top-0 bottom-0 bg-white preloader-bar" style="width: 0%;"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import gsap from 'gsap'

const progress = ref(0)

onMounted(() => {
  const tl = gsap.timeline()
  
  // Ensure the body cannot be scrolled while loading
  document.body.style.overflow = 'hidden'
  
  // Progress counter animation
  tl.to({ val: 0 }, {
    val: 100,
    duration: 1.8,
    ease: 'power3.inOut',
    onUpdate: function() {
      // Format number to always have 3 digits (e.g., 001, 045, 100) for a digital feel
      let val = Math.round(this.targets()[0].val)
      progress.value = val.toString().padStart(3, '0')
    }
  }, 0)

  // Progress bar animation
  tl.to('.preloader-bar', {
    width: '100%',
    duration: 1.8,
    ease: 'power3.inOut'
  }, 0)

  // Text glitch/fade in at start
  gsap.fromTo('.preloader-text', 
    { opacity: 0, filter: 'blur(5px)' },
    { opacity: 1, filter: 'blur(0px)', duration: 0.5, ease: 'power2.out' }
  )

  // Exit animations
  tl.to('.preloader-text, .preloader-counter, .preloader-bar-container', {
    y: -30,
    opacity: 0,
    duration: 0.5,
    stagger: 0.1,
    ease: 'power2.in'
  }, "+=0.3")

  // Slide the whole preloader up
  tl.to('.preloader-container', {
    yPercent: -100,
    duration: 0.9,
    ease: 'power4.inOut',
    onComplete: () => {
      // Restore scrolling
      document.body.style.overflow = 'auto'
    }
  })
})
</script>
