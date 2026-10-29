<script setup>
import ApplicationCard from './ApplicationCard.vue'

defineProps({
  applications: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['select'])
</script>

<template>
  <section aria-label="My applications">
    <div
      v-if="applications.length === 0"
      class="rounded-2xl border border-dashed border-slate-300 bg-slate-50 px-6 py-12 text-center"
    >
      <h3 class="text-lg font-bold text-slate-900">No applications yet</h3>
      <p class="mt-2 text-slate-600">Log your first application with the form.</p>
    </div>

    <div v-else class="grid gap-4 sm:grid-cols-2">
      <ApplicationCard
        v-for="application in applications"
        :key="application.id"
        :application="application"
        @select="emit('select', application.id)"
      />
    </div>
  </section>
</template>
