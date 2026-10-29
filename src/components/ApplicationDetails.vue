<script setup>
import { ref } from 'vue'

const props = defineProps({
  application: {
    type: Object,
    required: true,
  },
  events: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['back', 'update-status'])

const newStatus = ref(props.application.status)
</script>

<template>
  <section aria-labelledby="details-title">
    <button type="button" class="back-button" @click="emit('back')">
      <span aria-hidden="true">←</span> Back to applications
    </button>

    <p class="mt-5 text-sm font-semibold text-violet-700">{{ application.company }}</p>
    <h2 id="details-title" class="break-words text-2xl font-bold text-slate-900">{{ application.role }}</h2>

    <dl class="mt-6 grid gap-4 border-t border-slate-200 pt-5 sm:grid-cols-2">
      <div>
        <dt class="label">Status</dt>
        <dd class="value">{{ application.status }}</dd>
      </div>
      <div>
        <dt class="label">Date applied</dt>
        <dd class="value">{{ application.applied_date }}</dd>
      </div>
      <div>
        <dt class="label">Work mode</dt>
        <dd class="value">{{ application.work_mode }}<span v-if="application.location"> · {{ application.location }}</span></dd>
      </div>
      <div>
        <dt class="label">Found on</dt>
        <dd class="value">{{ application.source }}</dd>
      </div>
      <div v-if="application.salary">
        <dt class="label">Salary or allowance</dt>
        <dd class="value">{{ application.salary }}</dd>
      </div>
      <div v-if="application.job_url" class="sm:col-span-2">
        <dt class="label">Job posting</dt>
        <dd class="value break-all">
          <a :href="application.job_url" target="_blank" class="text-violet-700 underline">{{ application.job_url }}</a>
        </dd>
      </div>
      <div v-if="application.notes" class="sm:col-span-2">
        <dt class="label">Notes</dt>
        <dd class="value whitespace-pre-line">{{ application.notes }}</dd>
      </div>
    </dl>

    <div class="mt-6 border-t border-slate-200 pt-5">
      <label class="label" for="status">Update status</label>
      <div class="mt-2 flex flex-col gap-3 sm:flex-row">
        <select id="status" v-model="newStatus" class="status-select">
          <option>Applied</option>
          <option>Interview</option>
          <option>Offer</option>
          <option>Rejected</option>
        </select>
        <button type="button" class="save-button" @click="emit('update-status', newStatus)">
          Save status
        </button>
      </div>
    </div>

    <div class="mt-6 border-t border-slate-200 pt-5">
      <h3 class="text-lg font-bold text-slate-900">Follow-ups</h3>
      <p class="mt-1 text-sm text-slate-500">Added automatically by Make.com to the application_events table.</p>

      <p v-if="events.length === 0" class="mt-3 text-sm text-slate-600">
        No follow-up yet. Make.com adds one a few seconds after you save an application.
      </p>

      <ul v-else class="mt-3 space-y-2">
        <li v-for="event in events" :key="event.id" class="rounded-lg bg-violet-50 px-4 py-3 text-sm text-violet-900">
          <span class="font-bold">{{ event.event_type }}</span>
          <span v-if="event.due_date"> · due {{ event.due_date }}</span>
        </li>
      </ul>
    </div>
  </section>
</template>

<style scoped>
.label {
  display: block;
  color: #64748b;
  font-size: 13px;
  font-weight: 700;
}

.value {
  margin-top: 4px;
  color: #0f172a;
}

.back-button {
  color: #6d28d9;
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
}

.status-select {
  min-height: 44px;
  flex: 1;
  border: 1px solid #cbd5e1;
  border-radius: 10px;
  background: #ffffff;
  padding: 8px 12px;
  color: #0f172a;
}

.save-button {
  min-height: 44px;
  border-radius: 10px;
  background: #6d28d9;
  padding: 8px 20px;
  color: #ffffff;
  font-weight: 700;
  cursor: pointer;
}

.save-button:hover {
  background: #5b21b6;
}
</style>
