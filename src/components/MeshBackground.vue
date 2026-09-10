<template>
  <div class="fixed inset-0 z-0 pointer-events-none w-full h-full">
    <!-- WebGL Shader Background -->
    <div ref="canvasContainer" class="absolute inset-0 z-0 opacity-100"></div>

    <!-- Static Grain Overlay -->
    <div class="absolute inset-0 z-10 opacity-30 mix-blend-overlay" style="background-image: url('data:image/svg+xml,%3Csvg viewBox=%220 0 200 200%22 xmlns=%22http://www.w3.org/2000/svg%22%3E%3Cfilter id=%22noiseFilter%22%3E%3CfeTurbulence type=%22fractalNoise%22 baseFrequency=%220.85%22 numOctaves=%223%22 stitchTiles=%22stitch%22/%3E%3C/filter%3E%3Crect width=%22100%25%22 height=%22100%25%22 filter=%22url(%23noiseFilter)%22/%3E%3C/svg%3E');"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Renderer, Program, Mesh, Triangle } from 'ogl'

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
    st.x *= uResolution.x / uResolution.y;

    float n1 = snoise(st * 1.2 + uTime * 0.05);
    float n2 = snoise(st * 2.5 - uTime * 0.08);
    float n = (n1 + n2) * 0.5;

    // Increased contrast so the fluid motion is more noticeable
    vec3 color1 = vec3(0.0, 0.0, 0.0);       // Pitch Black
    vec3 color2 = vec3(0.15, 0.15, 0.15);    // Lighter Silver/Gray for higher visibility

    vec3 finalColor = mix(color1, color2, n * 0.5 + 0.5);

    // Subtle vignette
    float dist = distance(gl_FragCoord.xy / uResolution.xy, vec2(0.5));
    finalColor *= smoothstep(1.0, 0.1, dist);

    fragColor = vec4(finalColor, 1.0);
  }
`

onMounted(() => {
  if (canvasContainer.value) {
    const renderer = new Renderer({ alpha: false, antialias: true })
    const gl = renderer.gl
    canvasContainer.value.appendChild(gl.canvas)

    const geometry = new Triangle(gl)
    const program = new Program(gl, {
      vertex: VERT,
      fragment: FRAG,
      uniforms: {
        uTime: { value: 0 },
        uResolution: { value: [window.innerWidth, window.innerHeight] }
      }
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

    onUnmounted(() => {
      window.removeEventListener('resize', resize)
      cancelAnimationFrame(rafId)
      if (gl.canvas.parentNode) gl.canvas.parentNode.removeChild(gl.canvas)
      gl.getExtension('WEBGL_lose_context')?.loseContext()
    })
  }
})
</script>
