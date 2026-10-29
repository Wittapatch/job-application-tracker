<script setup>
import { computed, ref } from 'vue'
import { supabase } from './lib/supabase.js'
import ApplicationForm from './components/ApplicationForm.vue'
import ApplicationList from './components/ApplicationList.vue'
import ApplicationDetails from './components/ApplicationDetails.vue'

const userId = ref('')
const applications = ref([])
const message = ref('')
const currentView = ref('list')
const selectedApplicationId = ref(null)
const events = ref([])

const selectedApplication = computed(() =>
  applications.value.find((application) => application.id === selectedApplicationId.value),
)

// RLS needs a signed-in user, so each browser signs in anonymously.
async function signIn() {
  const { data } = await supabase.auth.getSession()
  if (data.session) {
    userId.value = data.session.user.id
    return
  }

  const { data: signInData, error } = await supabase.auth.signInAnonymously()
  if (error) {
    message.value = `Could not sign in: ${error.message}`
    return
  }
  userId.value = signInData.user.id
}

async function loadApplications() {
  const { data, error } = await supabase
    .from('applications')
    .select()
    .order('applied_date', { ascending: false })

  if (error) {
    message.value = `Could not load applications: ${error.message}`
    return
  }
  applications.value = data
}

async function saveApplication(application) {
  if (!supabase || !userId.value) {
    message.value = 'Supabase sign-in is not ready.'
    return
  }

  const { data, error } = await supabase
    .from('applications')
    .insert({
      owner_id: userId.value,
      company: application.company,
      role: application.role,
      job_url: application.jobUrl,
      applied_date: application.appliedDate,
      source: application.source,
      work_mode: application.workMode,
      location: application.location,
      salary: application.salary,
      notes: application.notes,
      status: 'Applied',
    })
    .select()
    .single()

  if (error) {
    message.value = `Could not save application: ${error.message}`
    return
  }

  applications.value.unshift(data)
  message.value = `Saved ${data.company} to Supabase.`
}

async function openDetails(id) {
  selectedApplicationId.value = id
  events.value = []
  currentView.value = 'details'
  await loadEvents(id)
}

// Read the follow-up rows that Make.com added for this application.
async function loadEvents(applicationId) {
  const { data, error } = await supabase
    .from('application_events')
    .select()
    .eq('application_id', applicationId)
    .order('created_at', { ascending: true })

  if (error) {
    message.value = `Could not load follow-ups: ${error.message}`
    return
  }
  events.value = data
}

async function updateStatus(newStatus) {
  const application = selectedApplication.value
  if (!application || !userId.value) return

  const { data, error } = await supabase
    .from('applications')
    .update({ status: newStatus })
    .eq('id', application.id)
    .eq('owner_id', userId.value)
    .select('id')
    .maybeSingle()

  if (error || !data) {
    message.value = error
      ? `Could not update status: ${error.message}`
      : 'No application was updated. Check ownership and RLS policies.'
    return
  }

  application.status = newStatus
  message.value = `${application.company} is now ${newStatus}.`
}

async function startApp() {
  if (!supabase) {
    message.value = 'Supabase is not configured. Add your keys to .env.local.'
    return
  }
  await signIn()
  if (userId.value) {
    await loadApplications()
  }
}

startApp()
</script>

<template>
  <div class="min-h-screen bg-slate-50">
    <header class="border-b border-slate-200 bg-white">
      <div class="mx-auto flex max-w-6xl flex-wrap items-center justify-between gap-3 px-5 py-4">
        <div class="flex items-center gap-3">
          <span class="logo" aria-hidden="true">J</span>
          <div>
            <h1 class="text-lg font-bold text-slate-900">Job Application Tracker</h1>
            <p class="text-sm text-slate-500">Every application, status and follow-up in one place</p>
          </div>
        </div>
        <span
          class="rounded-full px-3 py-1 text-xs font-bold"
          :class="userId ? 'bg-emerald-50 text-emerald-800' : 'bg-slate-100 text-slate-600'"
        >
          {{ userId ? 'Connected to Supabase' : 'Not connected' }}
        </span>
      </div>
    </header>

    <main class="mx-auto grid max-w-6xl items-start gap-6 px-5 py-8 lg:grid-cols-5">
      <p
        v-if="message"
        class="rounded-lg border border-slate-200 bg-white px-4 py-3 text-sm text-slate-700 lg:col-span-5"
        role="status"
      >
        {{ message }}
      </p>

      <section class="card lg:col-span-2">
        <h2 class="text-lg font-bold text-slate-900">Log an application</h2>
        <p class="mb-5 mt-1 text-sm text-slate-500">Fields marked * are required.</p>
        <ApplicationForm @submitted="saveApplication" />
      </section>

      <section v-if="currentView === 'details' && selectedApplication" class="card lg:col-span-3">
        <ApplicationDetails
          :key="selectedApplication.id"
          :application="selectedApplication"
          :events="events"
          @back="currentView = 'list'"
          @update-status="updateStatus"
        />
      </section>

      <section v-else class="card lg:col-span-3">
        <h2 class="mb-5 text-lg font-bold text-slate-900">
          My applications ({{ applications.length }})
        </h2>
        <ApplicationList :applications="applications" @select="openDetails" />
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
