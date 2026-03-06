<template>
  <div class="py-16">
    <p class="text-sm font-semibold uppercase tracking-widest text-olive-400 mb-4">ROI</p>
    <h2 class="font-display text-5xl md:text-6xl text-olive-950 mb-6">Better docs, faster onboarding.</h2>
    <p class="text-xl text-olive-600 max-w-2xl leading-relaxed mb-10">
      Up-to-date documentation means new developers find answers instead of asking around. Adjust the numbers to match your team.
    </p>

    <!-- Sliders -->
    <div class="grid md:grid-cols-3 gap-6 mb-10">
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Monthly salary</label>
        <input type="range" v-model.number="salary" min="2000" max="15000" step="1000"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">&euro;{{ salary.toLocaleString() }}<span class="text-sm font-normal text-olive-400">/mo</span></div>
      </div>
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Current onboarding</label>
        <input type="range" v-model.number="weeksBefore" min="2" max="12" step="1"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">{{ weeksBefore }} weeks</div>
      </div>
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">New hires per year</label>
        <input type="range" v-model.number="devs" min="1" max="50" step="1"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">{{ devs }} developer{{ devs > 1 ? 's' : '' }}</div>
      </div>
    </div>

    <!-- Comparison -->
    <div class="grid md:grid-cols-2 gap-8 mb-10">
      <div class="bg-olive-950 rounded-2xl p-8 text-olive-100">
        <h3 class="font-display text-2xl mb-4">Without askmanu</h3>
        <div class="space-y-3 text-olive-300">
          <div class="flex justify-between border-b border-olive-800 pb-2">
            <span>Onboarding time</span>
            <span class="font-semibold text-olive-100">{{ weeksBefore }} weeks</span>
          </div>
          <div class="flex justify-between border-b border-olive-800 pb-2">
            <span>Cost per hire</span>
            <span class="font-semibold text-olive-100">&euro;{{ costBefore.toLocaleString() }}</span>
          </div>
          <div class="flex justify-between">
            <span>Total ({{ devs }} hire{{ devs > 1 ? 's' : '' }})</span>
            <span class="font-semibold text-red-400">&euro;{{ totalBefore.toLocaleString() }}</span>
          </div>
        </div>
      </div>
      <div class="bg-olive-50 rounded-2xl p-8 border border-olive-200">
        <h3 class="font-display text-2xl text-olive-950 mb-4">With askmanu</h3>
        <div class="space-y-3 text-olive-700">
          <div class="flex justify-between border-b border-olive-200 pb-2">
            <span>Onboarding time</span>
            <span class="font-semibold text-olive-950">{{ weeksAfter }} weeks</span>
          </div>
          <div class="flex justify-between border-b border-olive-200 pb-2">
            <span>Cost per hire</span>
            <span class="font-semibold text-olive-950">&euro;{{ costAfter.toLocaleString() }}</span>
          </div>
          <div class="flex justify-between">
            <span>Total ({{ devs }} hire{{ devs > 1 ? 's' : '' }})</span>
            <span class="font-semibold text-green-700">&euro;{{ totalAfter.toLocaleString() }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Savings -->
    <div class="bg-olive-200 rounded-2xl p-8 text-center">
      <p class="text-olive-600 text-lg mb-2">Annual savings from onboarding alone</p>
      <p class="font-display text-6xl text-olive-950">&euro;{{ totalSaved.toLocaleString() }}</p>
      <p class="text-olive-500 mt-3 text-sm">{{ weeksBefore - weeksAfter }} fewer weeks per developer &times; {{ devs }} hire{{ devs > 1 ? 's' : '' }}/year</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return { salary: 5000, weeksBefore: 6, devs: 5 }
  },
  computed: {
    weeksAfter() { return Math.max(1, Math.round(this.weeksBefore / 3)) },
    costBefore() { return Math.round(this.salary * this.weeksBefore / 4) },
    costAfter() { return Math.round(this.salary * this.weeksAfter / 4) },
    totalBefore() { return this.costBefore * this.devs },
    totalAfter() { return this.costAfter * this.devs },
    totalSaved() { return this.totalBefore - this.totalAfter },
  },
}
</script>
