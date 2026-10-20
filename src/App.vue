<script setup>
import { ref } from 'vue'
import ApplicationForm from './components/ApplicationForm.vue'

const lastApplication = ref(null)

// For now the app only keeps the last submitted application.
// Saving to Supabase comes next.
function saveApplication(application) {
  lastApplication.value = application
}
</script>

<template>
  <div class="min-h-screen bg-slate-50">
    <header class="border-b border-slate-200 bg-white">
      <div class="mx-auto flex max-w-6xl items-center gap-3 px-5 py-4">
        <span class="logo" aria-hidden="true">J</span>
        <div>
          <h1 class="text-lg font-bold text-slate-900">Job Application Tracker</h1>
          <p class="text-sm text-slate-500">Every application, status and follow-up in one place</p>
        </div>
      </div>
    </header>

    <main class="mx-auto grid max-w-6xl items-start gap-6 px-5 py-8 lg:grid-cols-3">
      <section class="card lg:col-span-2">
        <h2 class="text-lg font-bold text-slate-900">Log an application</h2>
        <p class="mb-5 mt-1 text-sm text-slate-500">Fields marked * are required.</p>
        <ApplicationForm @submitted="saveApplication" />
      </section>

      <section v-if="lastApplication" class="card" role="status">
        <p class="text-sm font-semibold text-emerald-700">Application received</p>
        <h2 class="mt-2 text-lg font-bold text-slate-900">{{ lastApplication.company }}</h2>
        <p class="text-slate-600">{{ lastApplication.role }}</p>
        <p class="mt-3 text-sm text-slate-500">
          Applied {{ lastApplication.appliedDate }} · {{ lastApplication.workMode }} · {{ lastApplication.source }}
        </p>
        <p class="mt-1 text-sm text-slate-500">Status: {{ lastApplication.status }}</p>
      </section>
    </main>
  </div>
</template>

<style scoped>
.logo {
  display: grid;
  width: 40px;
  height: 40px;
  place-items: center;
  border-radius: 12px;
  background: #6d28d9;
  color: #ffffff;
  font-size: 18px;
  font-weight: 700;
}

.card {
  border: 1px solid #e2e8f0;
  border-radius: 20px;
  background: #ffffff;
  padding: 24px;
  box-shadow: 0 1px 2px rgba(15, 23, 42, 0.06);
}
</style>
