<template>
  <div
    ref="rootRef"
    :class="['flex w-full max-w-full [perspective:1400px] max-[520px]:!flex-col max-[520px]:[perspective:none]', vertical ? 'flex-col' : 'flex-row', customClass]"
    :style="{ gap: `${gap}px`, height: vertical ? `${Math.round(height * 1.6)}px` : `${height}px` }"
    role="list"
    aria-label="Image accordion gallery"
  >
    <component
      :is="item.link ? 'a' : 'div'"
      v-for="(item, i) in items"
      :key="i"
      :ref="el => setPanelRef(el, i)"
      class="group relative block min-w-0 min-h-0 flex-[1_1_0] cursor-pointer overflow-hidden bg-black no-underline outline-none [transform-style:preserve-3d] [transform-origin:center] [box-shadow:0_10px_30px_-18px_rgba(0,0,0,0.8)] focus-visible:[box-shadow:0_0_0_2px_var(--ag-accent),0_10px_30px_-18px_rgba(0,0,0,0.8)] max-[520px]:min-h-[84px] max-[520px]:!transform-none"
      :style="{ borderRadius: `${radius}px`, '--ag-accent': accentColor, willChange: 'flex-grow, transform' }"
      :href="item.link || undefined"
      @click="handleClick(i, $event)"
      @mouseenter="handleEnter(i)"
      @focus="setActive(i)"
      @keydown="handleKeyDown(i, $event)"
      role="listitem"
      tabindex="0"
      :aria-current="i === active ? 'true' : undefined"
      :aria-label="item.label"
    >
      <span class="absolute inset-0 overflow-hidden [border-radius:inherit]">
        <span
          :ref="el => setMediaRef(el, i)"
          class="absolute top-1/2 left-1/2 [filter:grayscale(var(--ag-gray,1))]"
          :style="{
            width: vertical ? '100%' : 'var(--ag-media-size, 320px)',
            height: vertical ? 'var(--ag-media-size, 320px)' : '100%',
            willChange: 'transform, filter'
          }"
        >
          <img
            :src="item.image"
            :alt="item.alt || item.label || ''"
            draggable="false"
            class="block h-full w-full select-none object-cover [-webkit-user-drag:none]"
          />
        </span>
        <span
          class="pointer-events-none absolute inset-0"
          :style="{ background: overlayBg }"
          aria-hidden="true"
        />
      </span>
      <span
        v-if="showLabels"
        class="pointer-events-none absolute bottom-5 left-5 right-5 z-[2] flex items-center gap-3"
        aria-hidden="true"
      >
        <span
          :ref="el => setBarRef(el, i)"
          class="h-[26px] w-[3px] flex-none rounded-[3px] opacity-0"
          :style="{
            background: accentColor,
            boxShadow: `0 0 12px color-mix(in srgb, ${accentColor} 60%, transparent)`
          }"
        />
        <span
          :ref="el => setTextRef(el, i)"
          class="overflow-hidden text-ellipsis whitespace-nowrap text-[clamp(1rem,1.4vw,1.4rem)] font-semibold tracking-[0.01em] opacity-0 [text-shadow:0_2px_14px_rgba(0,0,0,0.55)]"
          :style="{ color: textColor }"
        >
          {{ item.label }}
        </span>
      </span>
    </component>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch, computed } from 'vue';
import { gsap } from 'gsap';

const props = defineProps({
  items: {
    type: Array,
    default: () => []
  },
  defaultIndex: { type: Number, default: 2 },
  accentColor: { type: String, default: '#ffffff' },
  overlayColor: { type: String, default: '#000000' },
  textColor: { type: String, default: '#ffffff' },
  height: { type: Number, default: 460 },
  gap: { type: Number, default: 10 },
  radius: { type: Number, default: 0 },
  expandRatio: { type: Number, default: 0.52 },
  orientation: { type: String, default: 'horizontal' },
  duration: { type: Number, default: 0.6 },
  ease: { type: String, default: 'power3.out' },
  parallax: { type: Number, default: 0.5 },
  tilt: { type: Number, default: 8 },
  stagger: { type: Number, default: 0.06 },
  trigger: { type: String, default: 'hover' },
  showLabels: { type: Boolean, default: true },
  grayscale: { type: Boolean, default: true },
  customClass: { type: String, default: '' }
});

const rootRef = ref(null);
const panelRefs = ref([]);
const mediaRefs = ref([]);
const barRefs = ref([]);
const textRefs = ref([]);
let tl = null;
let firstRun = true;
let mediaSize = 320;

const vertical = computed(() => props.orientation === 'vertical');
const count = computed(() => props.items.length);
const active = ref(Math.min(Math.max(props.defaultIndex, 0), count.value - 1));

const setPanelRef = (el, i) => { if (el) panelRefs.value[i] = el.$el || el; };
const setMediaRef = (el, i) => { if (el) mediaRefs.value[i] = el; };
const setBarRef = (el, i) => { if (el) barRefs.value[i] = el; };
const setTextRef = (el, i) => { if (el) textRefs.value[i] = el; };

const prefersReduced =
  typeof window !== 'undefined' && window.matchMedia
    ? window.matchMedia('(prefers-reduced-motion: reduce)').matches
    : false;

const overlayBg = computed(() => `linear-gradient(180deg, transparent 45%, color-mix(in srgb, ${props.overlayColor} 78%, transparent) 100%), color-mix(in srgb, ${props.overlayColor} calc(var(--ag-dim, 0.35) * 100%), transparent)`);

const applyLayout = (animate) => {
  const panels = panelRefs.value;
  if (!panels.length) return;

  const r = Math.min(Math.max(props.expandRatio, 0.2), 0.9);
  const grow = count.value > 1 ? (r * (count.value - 1)) / (1 - r) : 1;

  if (tl) tl.kill();
  const dur = animate && !prefersReduced ? props.duration : 0;
  tl = gsap.timeline();

  panels.forEach((panel, i) => {
    if (!panel) return;
    const isActive = i === active.value;
    const media = mediaRefs.value[i];
    const bar = barRefs.value[i];
    const text = textRefs.value[i];

    const rot = isActive ? 0 : i < active.value ? props.tilt : -props.tilt;
    const rotProp = vertical.value ? { rotateX: -rot } : { rotateY: rot };

    tl.to(panel, { flexGrow: isActive ? grow : 1, ...rotProp, duration: dur, ease: props.ease }, 0);

    if (media) {
      const drift = Math.max(-1.5, Math.min(1.5, active.value - i));
      const shift = drift * props.parallax * mediaSize * 0.06;
      const gray = props.grayscale ? (isActive ? 0 : 1) : 0;
      tl.to(
        media,
        {
          xPercent: -50,
          yPercent: -50,
          x: vertical.value ? 0 : isActive ? 0 : shift,
          y: vertical.value ? (isActive ? 0 : shift) : 0,
          '--ag-gray': gray,
          '--ag-dim': isActive ? 0 : 0.35,
          duration: dur,
          ease: props.ease
        },
        0
      );
    }

    if (props.showLabels && bar && text) {
      if (isActive) {
        tl.to([bar, text], { opacity: 1, x: 0, duration: dur, ease: props.ease, stagger: prefersReduced ? 0 : props.stagger }, 0);
      } else {
        tl.to([bar, text], { opacity: 0, x: -14, duration: dur * 0.6, ease: props.ease }, 0);
      }
    }
  });
};

watch(active, () => {
  applyLayout(true);
});

let ro = null;

onMounted(() => {
  const el = rootRef.value;
  if (!el) return;

  const measure = () => {
    const rect = el.getBoundingClientRect();
    const total = vertical.value ? rect.height : rect.width;
    const usable = Math.max(total - props.gap * (count.value - 1), 120);
    const size = Math.max(140, usable * Math.min(Math.max(props.expandRatio, 0.2), 0.9) * 1.22);
    mediaSize = size;
    el.style.setProperty('--ag-media-size', `${size}px`);
    applyLayout(!firstRun);
  };

  measure();
  ro = new ResizeObserver(measure);
  ro.observe(el);

  applyLayout(!firstRun);
  firstRun = false;
});

onUnmounted(() => {
  if (ro && rootRef.value) ro.disconnect();
  if (tl) tl.kill();
});

const setActive = (i) => {
  active.value = i;
};

const handleEnter = (i) => {
  if (props.trigger === 'hover') setActive(i);
};

const handleClick = (i, e) => {
  if (i !== active.value) {
    e.preventDefault();
    setActive(i);
  }
};

const handleKeyDown = (i, e) => {
  if (e.key === 'ArrowRight' || e.key === 'ArrowDown') {
    e.preventDefault();
    setActive((i + 1) % count.value);
  } else if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') {
    e.preventDefault();
    setActive((i - 1 + count.value) % count.value);
  }
};
</script>
