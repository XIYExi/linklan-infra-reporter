<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  src: { type: String, required: true },
  scale: { type: Number, default: 2.5 }
})

const containerRef = ref(null)
const loading = ref(true)

onMounted(async () => {
  // Load pdf.js from CDN
  await new Promise((resolve) => {
    if (window.pdfjsLib) return resolve()
    const script = document.createElement('script')
    script.src = 'https://cdn.jsdelivr.net/npm/pdfjs-dist@3.11.174/build/pdf.min.js'
    script.onload = resolve
    document.head.appendChild(script)
  })

  const pdfjsLib = window.pdfjsLib
  pdfjsLib.GlobalWorkerOptions.workerSrc =
    'https://cdn.jsdelivr.net/npm/pdfjs-dist@3.11.174/build/pdf.worker.min.js'

  const loadingTask = pdfjsLib.getDocument(props.src)
  const pdf = await loadingTask.promise

  for (let i = 1; i <= pdf.numPages; i++) {
    const page = await pdf.getPage(i)
    const viewport = page.getViewport({ scale: props.scale })
    const canvas = document.createElement('canvas')
    canvas.width = viewport.width
    canvas.height = viewport.height
    canvas.style.width = '100%'
    canvas.style.display = 'block'
    containerRef.value.appendChild(canvas)

    await page.render({
      canvasContext: canvas.getContext('2d'),
      viewport
    }).promise
  }

  loading.value = false
})
</script>

<template>
  <div ref="containerRef" class="w-full">
    <div v-if="loading" class="flex items-center justify-center h-96">
      <span class="text-sm opacity-50">Loading PDF...</span>
    </div>
  </div>
</template>
