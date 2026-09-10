<template>
  <div class="min-h-screen text-white selection:bg-white selection:text-black font-mono relative">
    <Preloader />
    
    <!-- Global Animated Background -->
    <MeshBackground />
    
    <!-- Page Content -->
    <div class="relative z-10">
      <HeroSection />
      
      <TextMarquee text="C8OSHI THE LEGEND | UNDERC8VER MASTER | 8 IS THE WAY" />
      
      <LoreSection />

      <TextMarquee text="JOIN THE 8RMY | BEC8ME THE CULT" />

      <LinksSection />

      <TextMarquee text="THE WALL OF F8ME | 8RT IS HIST8RY" />

      <GallerySection />

      <FooterSection />
    </div>
  </div>
</template>

<script setup>
import Preloader from './components/Preloader.vue'
import MeshBackground from './components/MeshBackground.vue'
import HeroSection from './components/HeroSection.vue'
import TextMarquee from './components/TextMarquee.vue'
import LoreSection from './components/LoreSection.vue'
import LinksSection from './components/LinksSection.vue'
import GallerySection from './components/GallerySection.vue'
import FooterSection from './components/FooterSection.vue'
</script>
