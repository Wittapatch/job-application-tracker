<script setup>
defineProps({
  application: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['select'])

function statusClass(status) {
  if (status === 'Interview') return 'bg-amber-50 text-amber-800'
  if (status === 'Offer') return 'bg-emerald-50 text-emerald-800'
  if (status === 'Rejected') return 'bg-slate-100 text-slate-600'
  return 'bg-sky-50 text-sky-800'
}
</script>

<template>
  <article class="flex h-full min-w-0 flex-col rounded-2xl border border-slate-200 bg-white p-5 shadow-sm">
    <div class="flex items-start justify-between gap-3">
      <h3 class="break-words text-lg font-bold text-slate-900">{{ application.company }}</h3>
      <span class="rounded-full px-3 py-1 text-xs font-bold" :class="statusClass(application.status)">
        {{ application.status }}
      </span>
    </div>

    <p class="mt-1 break-words text-slate-600">{{ application.role }}</p>

    <p class="mt-4 text-sm text-slate-500">Applied: {{ application.applied_date }}</p>
    <p class="mt-1 text-sm text-slate-500">
      {{ application.work_mode }}<span v-if="application.location"> · {{ application.location }}</span>
    </p>
    <p class="mt-1 text-sm text-slate-500">Found on {{ application.source }}</p>

    <div class="mt-auto pt-5">
      <button type="button" class="details-button" @click="emit('select')">
        View details <span aria-hidden="true">→</span>
      </button>
    </div>
  </article>
</template>

<style scoped>
.details-button {
  border: 1px solid #6d28d9;
  border-radius: 10px;
  padding: 8px 16px;
  color: #6d28d9;
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
}

.details-button:hover {
  background: #f5f3ff;
}
</style>
