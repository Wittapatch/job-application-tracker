<script setup>
import { reactive, ref } from 'vue'

const emit = defineEmits(['submitted'])

const errorMessage = ref('')

const form = reactive({
  company: '',
  role: '',
  jobUrl: '',
  appliedDate: '',
  source: 'LinkedIn',
  workMode: 'On-site',
  location: '',
  salary: '',
  notes: '',
})

function clearForm() {
  form.company = ''
  form.role = ''
  form.jobUrl = ''
  form.appliedDate = ''
  form.source = 'LinkedIn'
  form.workMode = 'On-site'
  form.location = ''
  form.salary = ''
  form.notes = ''
}

function submitApplication() {
  const requiredFields = [form.company, form.role, form.appliedDate]

  if (requiredFields.some((value) => !value.trim())) {
    errorMessage.value = 'Complete company, role and date applied before saving.'
    return
  }

  errorMessage.value = ''

  const application = {
    ...form,
    status: 'Applied',
  }

  emit('submitted', application)
  clearForm()
}
</script>

<template>
  <form class="space-y-5" @submit.prevent="submitApplication" novalidate>
    <p
      v-if="errorMessage"
      class="rounded-lg border border-red-200 bg-red-50 px-4 py-3 text-sm text-red-800"
      role="alert"
    >
      {{ errorMessage }}
    </p>

    <div class="grid gap-5 sm:grid-cols-2">
      <label class="field-label">
        Company <span aria-hidden="true">*</span>
        <input v-model="form.company" class="field-control" type="text" placeholder="Agoda" required />
      </label>

      <label class="field-label">
        Role <span aria-hidden="true">*</span>
        <input v-model="form.role" class="field-control" type="text" placeholder="Frontend Developer Intern" required />
      </label>
    </div>

    <label class="field-label">
      Job posting link <span class="optional">(optional)</span>
      <input v-model="form.jobUrl" class="field-control" type="url" placeholder="https://careers.example.com/jobs/123" />
    </label>

    <div class="grid gap-5 sm:grid-cols-2">
      <label class="field-label">
        Date applied <span aria-hidden="true">*</span>
        <input v-model="form.appliedDate" class="field-control" type="date" required />
      </label>

      <label class="field-label">
        Found it on
        <select v-model="form.source" class="field-control">
          <option>LinkedIn</option>
          <option>JobsDB</option>
          <option>Company website</option>
          <option>University career fair</option>
          <option>Referral</option>
          <option>Other</option>
        </select>
      </label>
    </div>

    <div class="grid gap-5 sm:grid-cols-2">
      <label class="field-label">
        Work mode
        <select v-model="form.workMode" class="field-control">
          <option>On-site</option>
          <option>Hybrid</option>
          <option>Remote</option>
        </select>
      </label>

      <label class="field-label">
        Location <span class="optional">(optional)</span>
        <input v-model="form.location" class="field-control" type="text" placeholder="Bangkok" />
      </label>
    </div>

    <label class="field-label">
      Salary or allowance <span class="optional">(optional)</span>
      <input v-model="form.salary" class="field-control" type="text" placeholder="15,000 THB / month" />
    </label>

    <label class="field-label">
      Notes <span class="optional">(optional)</span>
      <textarea
        v-model="form.notes"
        class="field-control min-h-24 resize-y"
        rows="3"
        placeholder="Recruiter name, referral, what to prepare"
      ></textarea>
    </label>

    <button type="submit" class="form-button">Save application</button>
  </form>
</template>

<style scoped>
.field-label {
  display: block;
  color: #334155;
  font-size: 14px;
  font-weight: 700;
}

.optional {
  color: #64748b;
  font-weight: 400;
}

.field-control {
  display: block;
  width: 100%;
  min-height: 44px;
  margin-top: 8px;
  border: 1px solid #cbd5e1;
  border-radius: 10px;
  background: #ffffff;
  padding: 8px 12px;
  color: #0f172a;
  font-size: 16px;
  font-weight: 400;
}

.field-control:focus {
  outline: 3px solid #ddd6fe;
  border-color: #6d28d9;
}

.form-button {
  width: 100%;
  min-height: 48px;
  border-radius: 10px;
  background: #6d28d9;
  color: #ffffff;
  font-weight: 700;
  cursor: pointer;
}

.form-button:hover {
  background: #5b21b6;
}
</style>
