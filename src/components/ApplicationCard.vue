<script setup>
defineProps({
  application: {
    type: Object,
    required: true,
  },
})
</script>

<template>
  <article class="flex h-full min-w-0 flex-col rounded-2xl border border-slate-200 bg-white p-5 shadow-sm">
    <div class="flex items-start justify-between gap-3">
      <h3 class="break-words text-lg font-bold text-slate-900">{{ application.company }}</h3>
      <span
        class="rounded-full px-3 py-1 text-xs font-bold"
        :class="application.status === 'Applied' ? 'bg-sky-50 text-sky-800' : 'bg-slate-100 text-slate-700'"
      >
        {{ application.status }}
      </span>
    </div>

    <p class="mt-1 break-words text-slate-600">{{ application.role }}</p>

    <p class="mt-4 text-sm text-slate-500">Applied: {{ application.applied_date }}</p>
    <p class="mt-1 text-sm text-slate-500">
      {{ application.work_mode }}<span v-if="application.location"> · {{ application.location }}</span>
    </p>
    <p class="mt-1 text-sm text-slate-500">Found on {{ application.source }}</p>
  </article>
</template>
