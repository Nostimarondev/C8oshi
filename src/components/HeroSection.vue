<template>
  <section class="relative min-h-screen flex flex-col justify-center items-center overflow-hidden border-b border-gray-800 bg-black">
    
    <!-- WebGL Wavy Dots Background -->
    <div ref="canvasContainer" class="absolute inset-0 z-0 pointer-events-none opacity-40"></div>

    <!-- Content -->
    <div class="z-20 text-center flex flex-col items-center select-none">
      <div class="text-xs md:text-sm tracking-[0.5em] mb-4 text-gray-400 uppercase hero-reveal font-bold">
        System Initialized
      </div>
      
      <!-- Blurred Headline Reveal -->
      <h1 class="text-7xl md:text-[9rem] font-black tracking-tighter hero-title leading-none mb-2">
        C8oshi
      </h1>
      
      <h2 class="text-2xl md:text-5xl font-bold tracking-widest uppercase mb-8 text-gray-300 hero-reveal">
        Nakam8to
      </h2>
      
      <div class="flex items-center gap-4 mt-8 hero-reveal">
        <span class="px-6 py-2 border border-white text-white font-bold tracking-widest text-sm bg-white/5 backdrop-blur-md hover:bg-white hover:text-black transition-colors duration-300 cursor-pointer">
          TRADE $C8
        </span>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Renderer, Program, Mesh, Triangle } from 'ogl'
import gsap from 'gsap'

const canvasContainer = ref(null)

const VERT = `#version 300 es
  in vec2 position;
  void main() {
    gl_Position = vec4(position, 0.0, 1.0);
  }
`

const FRAG = `#version 300 es
  precision highp float;
  uniform float uTime;
  uniform vec2 uResolution;

  out vec4 fragColor;

  // Simplex 2D noise
  vec3 mod289(vec3 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
  vec2 mod289(vec2 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
  vec3 permute(vec3 x) { return mod289(((x*34.0)+1.0)*x); }

  float snoise(vec2 v) {
    const vec4 C = vec4(0.211324865405187, 0.366025403784439, -0.577350269189626, 0.024390243902439);
    vec2 i  = floor(v + dot(v, C.yy));
    vec2 x0 = v -   i + dot(i, C.xx);
    vec2 i1;
    i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
    vec4 x12 = x0.xyxy + C.xxzz;
    x12.xy -= i1;
    i = mod289(i);
    vec3 p = permute(permute( i.y + vec3(0.0, i1.y, 1.0 )) + i.x + vec3(0.0, i1.x, 1.0 ));
    vec3 m = max(0.5 - vec3(dot(x0,x0), dot(x12.xy,x12.xy), dot(x12.zw,x12.zw)), 0.0);
    m = m*m;
    m = m*m;
    vec3 x = 2.0 * fract(p * C.www) - 1.0;
    vec3 h = abs(x) - 0.5;
    vec3 ox = floor(x + 0.5);
    vec3 a0 = x - ox;
    m *= 1.79284291400159 - 0.85373472095314 * ( a0*a0 + h*h );
    vec3 g;
    g.x  = a0.x  * x0.x  + h.x  * x0.y;
    g.yz = a0.yz * x12.xz + h.yz * x12.yw;
    return 130.0 * dot(m, g);
  }

  void main() {
    vec2 st = gl_FragCoord.xy / uResolution.xy;
    vec2 aspectUv = gl_FragCoord.xy / uResolution.y;

    // Create topographical waves using combined noise layers
    float n1 = snoise(aspectUv * 1.5 - uTime * 0.1);
    float n2 = snoise(aspectUv * 4.0 + uTime * 0.05);
    float height = (n1 * 0.7 + n2 * 0.3) * 0.15;

    // Displace UVs to simulate 3D waves
    vec2 displacedUv = aspectUv;
    displacedUv.y -= height * 1.2;
    displacedUv.x += height * 0.5;

    // Create the grid of dots
    float gridScale = 70.0;
    vec2 grid = fract(displacedUv * gridScale);
    
    // Distance from cell center
    float dist = distance(grid, vec2(0.5));
    
    // Dynamic dot radius based on the wave height (closer = bigger)
    float baseRadius = 0.10;
    float dynamicRadius = baseRadius + height * 0.8;
    
    // Smooth edges for dots
    float dotAlpha = 1.0 - smoothstep(dynamicRadius - 0.05, dynamicRadius + 0.05, dist);
    
    // Mask out the edges to keep focus on the center
    float vignette = 1.0 - distance(st, vec2(0.5)) * 1.2;
    vignette = smoothstep(0.0, 0.6, vignette);

    // Dim the dots that are deeper in the valleys
    float depthShade = 0.3 + (height + 0.15) * 2.5; 
    
    // Render white dots with dynamic transparency
    fragColor = vec4(vec3(1.0), dotAlpha * vignette * depthShade);
  }
`

onMounted(() => {
  // --- WebGL Wavy Dots Setup ---
  if (canvasContainer.value) {
    const renderer = new Renderer({ alpha: true, antialias: true })
    const gl = renderer.gl
    gl.clearColor(0, 0, 0, 0)
    
    // Enable blending for transparent dots
    gl.enable(gl.BLEND)
    gl.blendFunc(gl.SRC_ALPHA, gl.ONE_MINUS_SRC_ALPHA)

    canvasContainer.value.appendChild(gl.canvas)

    const geometry = new Triangle(gl)
    const program = new Program(gl, {
      vertex: VERT,
      fragment: FRAG,
      uniforms: {
        uTime: { value: 0 },
        uResolution: { value: [window.innerWidth, window.innerHeight] }
      },
      transparent: true
    })

    const mesh = new Mesh(gl, { geometry, program })

    const resize = () => {
      renderer.setSize(window.innerWidth, window.innerHeight)
      program.uniforms.uResolution.value = [window.innerWidth, window.innerHeight]
    }
    window.addEventListener('resize', resize)
    resize()

    let rafId
    const update = (t) => {
      rafId = requestAnimationFrame(update)
      program.uniforms.uTime.value = t * 0.001
      renderer.render({ scene: mesh })
    }
    rafId = requestAnimationFrame(update)

    // Cleanup
    onUnmounted(() => {
      window.removeEventListener('resize', resize)
      cancelAnimationFrame(rafId)
      if (gl.canvas.parentNode) gl.canvas.parentNode.removeChild(gl.canvas)
      gl.getExtension('WEBGL_lose_context')?.loseContext()
    })
  }

  // --- Blurred Headline Reveal Animation (GSAP) ---
  // Delay timeline by 2.6s so it fires exactly when the preloader slides up
  const tl = gsap.timeline({ delay: 2.6 })
  
  tl.fromTo('.hero-title', 
    { filter: 'blur(40px)', opacity: 0, scale: 1.2 },
    { filter: 'blur(0px)', opacity: 1, scale: 1, duration: 2.5, ease: 'power3.out' }
  )
  
  tl.fromTo('.hero-reveal',
    { filter: 'blur(15px)', opacity: 0, y: 30 },
    { filter: 'blur(0px)', opacity: 1, y: 0, duration: 1.5, stagger: 0.2, ease: 'power3.out' },
    "-=1.5"
  )
})
</script>

<style scoped>
.hero-title, .hero-reveal {
  will-change: filter, opacity, transform;
}
</style>
