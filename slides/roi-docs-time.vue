<template>
  <div class="py-16">
    <p class="text-sm font-semibold uppercase tracking-widest text-olive-400 mb-4">ROI</p>
    <h2 class="font-display text-5xl md:text-6xl text-olive-950 mb-6">How much do manual doc updates cost you?</h2>
    <p class="text-xl text-olive-600 max-w-3xl leading-relaxed mb-10">
      Every release, someone on your team stops building to update docs. That time adds up fast. Adjust the sliders to see your real cost.
    </p>

    <!-- Sliders -->
    <div class="grid md:grid-cols-3 gap-6 mb-10">
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Hourly dev cost</label>
        <input type="range" v-model.number="hourlyRate" min="50" max="200" step="10"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">&euro;{{ hourlyRate }}<span class="text-sm font-normal text-olive-400">/hour</span></div>
      </div>
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Hours per docs update</label>
        <input type="range" v-model.number="hoursPerUpdate" min="1" max="24" step="1"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">{{ hoursPerUpdate }} hour{{ hoursPerUpdate > 1 ? 's' : '' }}</div>
      </div>
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Releases per year</label>
        <input type="range" v-model.number="releasesPerYear" min="4" max="52" step="1"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">{{ releasesPerYear }} release{{ releasesPerYear > 1 ? 's' : '' }}<span class="text-sm font-normal text-olive-400">/year</span></div>
      </div>
    </div>

    <!-- Comparison -->
    <div class="grid md:grid-cols-2 gap-8 mb-10">
      <div class="bg-olive-950 rounded-2xl p-8 text-olive-100">
        <h3 class="font-display text-2xl mb-4">Without askmanu</h3>
        <div class="space-y-3 text-olive-300">
          <div class="flex justify-between border-b border-olive-800 pb-2">
            <span>Time per release</span>
            <span class="font-semibold text-olive-100">{{ hoursPerUpdate }}h</span>
          </div>
          <div class="flex justify-between border-b border-olive-800 pb-2">
            <span>Cost per release</span>
            <span class="font-semibold text-olive-100">&euro;{{ costPerRelease.toLocaleString() }}</span>
          </div>
          <div class="flex justify-between">
            <span>Annual docs cost</span>
            <span class="font-semibold text-red-400">&euro;{{ totalBefore.toLocaleString() }}</span>
          </div>
        </div>
      </div>
      <div class="bg-olive-50 rounded-2xl p-8 border border-olive-200">
        <h3 class="font-display text-2xl text-olive-950 mb-4">With askmanu</h3>
        <div class="space-y-3 text-olive-700">
          <div class="flex justify-between border-b border-olive-200 pb-2">
            <span>Time per release</span>
            <span class="font-semibold text-olive-950">{{ hoursAfter }}h <span class="text-sm font-normal text-olive-400">(review only)</span></span>
          </div>
          <div class="flex justify-between border-b border-olive-200 pb-2">
            <span>Cost per release</span>
            <span class="font-semibold text-olive-950">&euro;{{ costAfter.toLocaleString() }}</span>
          </div>
          <div class="flex justify-between">
            <span>Annual docs cost</span>
            <span class="font-semibold text-green-700">&euro;{{ totalAfter.toLocaleString() }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Savings -->
    <div class="bg-olive-200 rounded-2xl p-8 text-center">
      <p class="text-olive-600 text-lg mb-2">Annual savings on documentation work</p>
      <p class="font-display text-6xl text-olive-950">&euro;{{ totalSaved.toLocaleString() }}</p>
      <p class="text-olive-500 mt-3 text-sm">{{ hoursPerUpdate - hoursAfter }}h saved per release &times; {{ releasesPerYear }} releases/year &times; &euro;{{ hourlyRate }}/h</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return { hourlyRate: 80, hoursPerUpdate: 16, releasesPerYear: 20 }
  },
  computed: {
    hoursAfter() { return Math.max(1, Math.round(this.hoursPerUpdate * 0.2)) },
    costPerRelease() { return this.hourlyRate * this.hoursPerUpdate },
    costAfter() { return this.hourlyRate * this.hoursAfter },
    totalBefore() { return this.costPerRelease * this.releasesPerYear },
    totalAfter() { return this.costAfter * this.releasesPerYear },
    totalSaved() { return this.totalBefore - this.totalAfter },
  },
}
</script>
