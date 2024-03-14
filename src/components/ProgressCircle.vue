<template>
  <div class="flex justify-center items-center">
    <input class="mr-6" type="number" v-model="value" min="0" max="1" step="0.05" />
    <svg
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      width="200"
      height="200"
      viewBox="0 0 120 120"
    >
      <mask id="mask">
        <circle
          cx="60"
          cy="60"
          r="50"
          fill="none"
          stroke="white"
          stroke-width="8"
          :stroke-dasharray="dashArray"
        >
          <animate
            attributeName="stroke-dashoffset"
            dur="3s"
            :values="`${dashArray};0`"
            fill="freeze"
            repeatCount="one"
          />
        </circle>
      </mask>
      <circle cx="60" cy="60" r="50" fill="none" stroke="#4F4466" stroke-width="8" />
      <circle
        cx="60"
        cy="60"
        r="50"
        transform="rotate(-90 60 60)"
        fill="none"
        ref="circle"
        stroke="#EBE424"
        stroke-width="8"
        mask="url(#mask)"
        style="transition: stroke-dashoffset 0.3s"
      ></circle>
      <text id="count" x="50%" y="50%" fill="#EBE424" text-anchor="middle" dy="7" font-size="20">
        {{
          value <= 1 && value > 0 ? Math.round(value * 100) : value < 0 ? 0 : value > 1 ? 100 : 0
        }}%
      </text>
    </svg>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'ProgressCircle',
  data: () => ({
    value: 0.7,
    dashArray: 0
  }),
  methods: {
    setProgress(val: number) {
      const circle = this.$refs.circle as SVGGeometryElement
      const radius = circle.getAttribute('r') as any
      const circumference = radius * 2 * Math.PI
      this.dashArray = circumference
      circle.style.strokeDasharray = String(circumference)
      circle.style.strokeDashoffset = String(
        circumference - (val <= 1 && val > 0 ? val : val < 0 ? 0 : val > 1 ? 1 : 0) * circumference
      )
    }
  },
  watch: {
    value: function (val) {
      this.setProgress(val)
    }
  },
  mounted() {
    this.setProgress(this.value)
  }
})
</script>
<style scoped></style>
