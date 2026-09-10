<template>
  <section class="py-24 px-6 md:px-12 max-w-7xl mx-auto border-b border-gray-800" id="lore-section">
    <div class="flex flex-col md:flex-row gap-12 md:gap-24 relative">
      
      <!-- Left: Sticky Narrative -->
      <div class="w-full md:w-1/3 relative">
        <div class="sticky top-32">
          <h2 class="text-4xl md:text-5xl font-black mb-8 uppercase tracking-tighter">C8OSHI'S PATH</h2>
          <div class="space-y-6 text-gray-400 leading-relaxed font-medium">
            <p>
              C8OSHI visually inherits the naming convention from <span class="text-white font-black">C8NTINUUM</span>. 
            </p>
            <p>
              The 8 represents the octopus. The <span class="text-white">-oshi</span> suffix feels like a character, honoring the obvious Satoshi association.
            </p>
            <p>
              C8oshi is not merely the coin name. He is the actual octopus mascot.
            </p>
            <p class="text-white font-bold border-l-4 border-white pl-4 my-8 uppercase tracking-widest text-sm">
              The full meme name C8OSHI NAKAM8TO is the absurd version used for lore, while C8oshi is what everyone actually calls him.
            </p>
          </div>
        </div>
      </div>

      <!-- Right: Timeline Rail & Milestones -->
      <div class="w-full md:w-2/3 relative mt-12 md:mt-0">
        
        <!-- The Vertical Rail Line -->
        <div class="absolute left-3 md:left-6 top-2 bottom-0 w-[2px] bg-gray-900" id="timeline-rail">
           <!-- Animated Progress Line -->
           <div class="w-full bg-white origin-top" id="timeline-progress" style="height: 100%; transform: scaleY(0);"></div>
        </div>

        <!-- Milestones -->
        <div class="flex flex-col gap-24 relative z-10 pl-12 md:pl-20 py-2">
          
          <div class="milestone relative opacity-0 translate-y-10" v-for="(milestone, index) in milestones" :key="index">
            <!-- Interactive Dot -->
            <div class="absolute -left-[2.65rem] md:-left-[3.95rem] top-1 w-5 h-5 bg-black border-2 border-gray-600 rounded-full milestone-dot transition-colors duration-500"></div>
            
            <div class="text-xs text-gray-500 font-bold tracking-widest uppercase mb-3">{{ milestone.date }}</div>
            <h3 class="text-3xl font-black uppercase tracking-tighter mb-4">{{ milestone.title }}</h3>
            <p class="text-gray-400 mb-8 font-medium leading-relaxed max-w-lg">{{ milestone.text }}</p>
            
            <!-- Milestone Image -->
            <div class="overflow-hidden border-2 border-gray-900 max-w-lg group cursor-pointer" @click="openModal(index)">
              <img 
                :src="milestone.image" 
                :alt="milestone.title" 
                class="w-full h-auto aspect-video object-cover object-top grayscale opacity-70 group-hover:grayscale-0 group-hover:opacity-100 transition-all duration-700 group-hover:scale-105" 
              />
            </div>
          </div>

        </div>
      </div>

    </div>

    <!-- Lore Lightbox Modal -->
    <Teleport to="body">
      <Transition
        enter-active-class="transition duration-300 ease-out"
        enter-from-class="opacity-0 scale-95"
        enter-to-class="opacity-100 scale-100"
        leave-active-class="transition duration-200 ease-in"
        leave-from-class="opacity-100 scale-100"
        leave-to-class="opacity-0 scale-95"
      >
        <div
          v-if="selectedImageIndex !== null"
          class="fixed inset-0 z-[9999] flex flex-col items-center justify-center bg-black/80 backdrop-blur-xl p-4 md:p-8 select-none"
          @click.self="closeModal"
        >
          <!-- Close Button -->
          <button
            @click="closeModal"
            class="absolute top-6 right-6 z-10 p-3 rounded-full bg-white/10 hover:bg-white/20 text-white border border-white/20 transition-all duration-300 hover:scale-110 cursor-pointer focus:outline-none"
            aria-label="Close modal"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>

          <!-- Prev Button -->
          <button
            v-if="milestones.length > 1"
            @click.stop="prevImage"
            class="absolute left-4 md:left-8 z-10 p-3 md:p-4 rounded-full bg-white/10 hover:bg-white/20 text-white border border-white/20 transition-all duration-300 hover:scale-110 cursor-pointer focus:outline-none"
            aria-label="Previous image"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
            </svg>
          </button>

          <!-- Next Button -->
          <button
            v-if="milestones.length > 1"
            @click.stop="nextImage"
            class="absolute right-4 md:right-8 z-10 p-3 md:p-4 rounded-full bg-white/10 hover:bg-white/20 text-white border border-white/20 transition-all duration-300 hover:scale-110 cursor-pointer focus:outline-none"
            aria-label="Next image"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
          </button>

          <!-- Image Container -->
          <div class="relative max-w-5xl max-h-[85vh] flex flex-col items-center justify-center p-2" @click.stop>
            <img
              :src="milestones[selectedImageIndex].image"
              :alt="milestones[selectedImageIndex].title || 'Lore Image'"
              class="max-h-[75vh] max-w-[90vw] object-contain rounded-2xl border-2 border-white/20 shadow-[0_0_60px_rgba(255,255,255,0.2)]"
            />
            
            <div class="mt-4 text-center max-w-xl">
              <div class="text-xs text-gray-400 font-bold uppercase tracking-widest mb-1">
                {{ milestones[selectedImageIndex].date }}
              </div>
              <h3 class="text-xl md:text-2xl font-black uppercase tracking-widest text-white">
                {{ milestones[selectedImageIndex].title }}
              </h3>
            </div>
          </div>
        </div>
      </Transition>
    </Teleport>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'

import img1 from '../assets/1.webp'
import img2 from '../assets/2.webp'
import img3 from '../assets/3.webp'

gsap.registerPlugin(ScrollTrigger)

const selectedImageIndex = ref(null)

const openModal = (i) => {
  selectedImageIndex.value = i
  if (typeof document !== 'undefined') {
    document.body.style.overflow = 'hidden'
  }
}

const closeModal = () => {
  selectedImageIndex.value = null
  if (typeof document !== 'undefined') {
    document.body.style.overflow = ''
  }
}

const nextImage = () => {
  if (selectedImageIndex.value !== null) {
    selectedImageIndex.value = (selectedImageIndex.value + 1) % milestones.length
  }
}

const prevImage = () => {
  if (selectedImageIndex.value !== null) {
    selectedImageIndex.value = (selectedImageIndex.value - 1 + milestones.length) % milestones.length
  }
}

const handleGlobalKeydown = (e) => {
  if (selectedImageIndex.value === null) return
  if (e.key === 'Escape') closeModal()
  else if (e.key === 'ArrowRight') nextImage()
  else if (e.key === 'ArrowLeft') prevImage()
}

const milestones = [
  { 
    date: 'Phase 01', 
    title: 'The Genesis', 
    text: 'C8OSHI emerges from the depths of the C8NTINUUM network. Visually, a connected network of tentacles, laying the foundation for what is to come.',
    image: img1
  },
  { 
    date: 'Phase 02', 
    title: '8RMY Assemble', 
    text: 'The tentacles reach out. Nodes begin to connect. A cult forms around the UNDERC8VER master, cementing the $C8 ticker in the ecosystem.',
    image: img2
  },
  { 
    date: 'Phase 03', 
    title: 'The Nakam8to Legacy', 
    text: 'With the lore fully established, C8OSHI NAKAM8TO becomes more than a meme. It becomes the ultimate interoperable mascot.',
    image: img3
  }
]

onMounted(() => {
  window.addEventListener('keydown', handleGlobalKeydown)

  // Animate the rail progress line as the user scrolls
  gsap.to('#timeline-progress', {
    scrollTrigger: {
      trigger: '#timeline-rail',
      start: 'top 50%',
      end: 'bottom 60%',
      scrub: 0.5
    },
    scaleY: 1,
    ease: 'none'
  })

  // Animate each milestone block and its dot
  gsap.utils.toArray('.milestone').forEach((milestone) => {
    // Reveal text and image
    gsap.to(milestone, {
      scrollTrigger: {
        trigger: milestone,
        start: 'top 85%',
        toggleActions: 'play none none reverse'
      },
      y: 0,
      opacity: 1,
      duration: 0.8,
      ease: 'power3.out'
    })
    
    // Light up the dot when the section hits the middle of the screen
    const dot = milestone.querySelector('.milestone-dot')
    ScrollTrigger.create({
      trigger: milestone,
      start: 'top 60%',
      onEnter: () => dot.classList.add('border-white', 'bg-white', 'shadow-[0_0_15px_rgba(255,255,255,0.5)]'),
      onLeaveBack: () => dot.classList.remove('border-white', 'bg-white', 'shadow-[0_0_15px_rgba(255,255,255,0.5)]')
    })
  })
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleGlobalKeydown)
})
</script>
