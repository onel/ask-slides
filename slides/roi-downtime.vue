<template>
  <div class="py-16">
    <p class="text-sm font-semibold uppercase tracking-widest text-olive-400 mb-4">ROI</p>
    <h2 class="font-display text-5xl md:text-6xl text-olive-950 mb-6">When your app goes down, docs pay for themselves.</h2>
    <p class="text-xl text-olive-600 max-w-3xl leading-relaxed mb-10">
      During an incident, engineers scramble through code, Slack threads, and tribal knowledge. With current docs, they find the answer in minutes instead of hours.
    </p>

    <!-- Sliders -->
    <div class="grid md:grid-cols-3 gap-6 mb-10">
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Downtime cost</label>
        <input type="range" v-model.number="costPerHour" min="1000" max="50000" step="1000"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">&euro;{{ costPerHour.toLocaleString() }}<span class="text-sm font-normal text-olive-400">/hour</span></div>
      </div>
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Current recovery time</label>
        <input type="range" v-model.number="hoursBefore" min="1" max="12" step="1"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">{{ hoursBefore }} hour{{ hoursBefore > 1 ? 's' : '' }}</div>
      </div>
      <div class="bg-olive-50 rounded-xl p-6 border border-olive-200">
        <label class="text-sm font-semibold text-olive-700 block mb-2">Incidents per year</label>
        <input type="range" v-model.number="incidents" min="1" max="24" step="1"
          class="w-full accent-olive-600" @mousedown.stop @keydown.stop />
        <div class="text-2xl font-semibold text-olive-950 mt-2">{{ incidents }} incident{{ incidents > 1 ? 's' : '' }}</div>
      </div>
    </div>

    <!-- Comparison -->
    <div class="grid md:grid-cols-2 gap-8 mb-10">
      <div class="bg-olive-950 rounded-2xl p-8 text-olive-100">
        <h3 class="font-display text-2xl mb-4">Without askmanu</h3>
        <div class="space-y-3 text-olive-300">
          <div class="flex justify-between border-b border-olive-800 pb-2">
            <span>Recovery time</span>
            <span class="font-semibold text-olive-100">{{ hoursBefore }}h per incident</span>
          </div>
          <div class="flex justify-between border-b border-olive-800 pb-2">
            <span>Cost per incident</span>
            <span class="font-semibold text-olive-100">&euro;{{ downtimeBefore.toLocaleString() }}</span>
          </div>
          <div class="flex justify-between">
            <span>Annual downtime cost</span>
            <span class="font-semibold text-red-400">&euro;{{ totalBefore.toLocaleString() }}</span>
          </div>
        </div>
      </div>
      <div class="bg-olive-50 rounded-2xl p-8 border border-olive-200">
        <h3 class="font-display text-2xl text-olive-950 mb-4">With askmanu</h3>
        <div class="space-y-3 text-olive-700">
          <div class="flex justify-between border-b border-olive-200 pb-2">
            <span>Recovery time</span>
            <span class="font-semibold text-olive-950">{{ hoursAfter }}h per incident</span>
          </div>
          <div class="flex justify-between border-b border-olive-200 pb-2">
            <span>Cost per incident</span>
            <span class="font-semibold text-olive-950">&euro;{{ downtimeAfter.toLocaleString() }}</span>
          </div>
          <div class="flex justify-between">
            <span>Annual downtime cost</span>
            <span class="font-semibold text-green-700">&euro;{{ totalAfter.toLocaleString() }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Savings -->
    <div class="bg-olive-200 rounded-2xl p-8 text-center">
      <p class="text-olive-600 text-lg mb-2">Annual savings from faster incident recovery</p>
      <p class="font-display text-6xl text-olive-950">&euro;{{ totalSaved.toLocaleString() }}</p>
      <p class="text-olive-500 mt-3 text-sm">{{ hoursBefore - hoursAfter }}h faster recovery &times; {{ incidents }} incident{{ incidents > 1 ? 's' : '' }}/year &times; &euro;{{ costPerHour.toLocaleString() }}/h</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return { costPerHour: 5000, hoursBefore: 4, incidents: 6 }
  },
  computed: {
    hoursAfter() { return Math.max(1, Math.round(this.hoursBefore / 3)) },
    downtimeBefore() { return this.costPerHour * this.hoursBefore },
    downtimeAfter() { return this.costPerHour * this.hoursAfter },
    totalBefore() { return this.downtimeBefore * this.incidents },
    totalAfter() { return this.downtimeAfter * this.incidents },
    totalSaved() { return this.totalBefore - this.totalAfter },
  },
}
</script>
