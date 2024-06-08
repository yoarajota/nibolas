<script setup>
import { createClient } from '@supabase/supabase-js'
const supabaseUrl = 'https://algbvvaontqbyovbsrfz.supabase.co'
const supabaseKey = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImFsZ2J2dmFvbnRxYnlvdmJzcmZ6Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2OTcyMzM5NDcsImV4cCI6MjAxMjgwOTk0N30.cd32GxDZEivaTZdlbtOV7bhmfhPZCUB_za40JhjXMg0"
const supabase = createClient(supabaseUrl, supabaseKey)

const items = ref([])
const reactiveForm = reactive({
  name: '',
  value: 0
})

useHead({
  title: 'Quanto deve Nibolas?',
  meta: [
  ],
  bodyAttrs: {
  },
  script: []
})

const fetched = ref(false)
const howMuch = ref(0)

function animateSumOfValue(value) {
  let interval = 120;
  let animate;
  let summed = 0;

  const startInterval = () => {
    animate = setInterval(() => {
      let remaining = value - summed;

      if (remaining === 0) {
        clearInterval(animate)
      }

      if (remaining >= 1000) {
        howMuch.value += 1000
        summed += 1000
      } else if (remaining >= 100) {
        howMuch.value += 100
        summed += 100
      } else if (remaining >= 10) {
        howMuch.value += 10
        summed += 10
        clearInterval(animate);
        interval = 120; // Aumenta o intervalo quando se aproxima de 10
        startInterval();
      } else if (remaining >= 1) {
        howMuch.value += 1
        summed += 1
        clearInterval(animate);
        interval = 180; // Aumenta ainda mais o intervalo quando se aproxima de 1
        startInterval();
      } else {
        clearInterval(animate)
      }
    }, interval)
  }

  startInterval();
}

function handleInserts(payload) {
  if (!fetched.value || items.value.find(item => item.id == payload.new.id)) return

  items.value.push(payload.new)
  animateSumOfValue(payload.new.value)
}

onBeforeMount(() => {
  supabase
    .from('nibolas')
    .select('*')
    .order('id')
    .then(({ data }) => {
      items.value = data
      fetched.value = true
      calcHowMuch()
    })

  supabase
    .channel('nibolas')
    .on('postgres_changes', { event: 'INSERT', schema: 'public', table: 'nibolas' }, handleInserts)
    .subscribe()
})

function calcHowMuch() {
  let total = 0
  items.value.forEach(item => {
    total += item.value
  })

  let interval = 50;
  let animate;

  const startInterval = () => {
    animate = setInterval(() => {
      if ((howMuch.value + 1000) < total) {
        howMuch.value += 1000
      } else if ((howMuch.value + 100) < total) {
        howMuch.value += 100
      } else if ((howMuch.value + 10) < total) {
        howMuch.value += 10
        clearInterval(animate);
        interval = 120; // Aumenta o intervalo quando se aproxima de 10
        startInterval();
      } else if (howMuch.value < total) {
        howMuch.value += 1
        clearInterval(animate);
        interval = 180; // Aumenta ainda mais o intervalo quando se aproxima de 1
        startInterval();
      } else {
        clearInterval(animate)
      }
    }, interval)
  }

  startInterval();
}

function submit(e) {
  e.preventDefault()

  supabase
    .from('nibolas')
    .insert(
      reactiveForm
    )
    .select('id')
    .single()
    .then(({ data }) => {
      items.value.push({ ...reactiveForm, id: data.id })

      animateSumOfValue(reactiveForm.value)

      reactiveForm.name = ''
      reactiveForm.value = 0
    })

}

</script>

<template>
  <h1 class="text-zinc-100 text-4xl sm:text-6xl text-center mt-12">
    Quanto deve Nibolas?
  </h1>
  <h2 class="text-zinc-100 text-lg sm:text-2xl text-center mt-6">
    Lista publica das dividas de Nibolas
  </h2>
  <h3 class="text-zinc-100 text-xl sm:text-3xl text-center mt-4">
    R$ {{ howMuch }}
  </h3>

  <div class="mx-auto px-6 sm:px-0 max-w-3xl">
    <form @submit="submit" class="flex flex-col sm:flex-row gap-4 items-center sm:items-end justify-center mt-8">
      <div class="min-w-full sm:min-w-fit">
        <label for="name" class="block text-sm font-medium leading-6 text-zinc-100">Nome</label>

        <div class="relative mt-2 rounded-md shadow-sm">
          <input v-model="reactiveForm.name" type="text" name="name" id="name"
            class="block sm:w-[420px] w-full rounded-md outline-none border-0 py-1.5 px-3 text-zinc-100 bg-slate-900 ring-1 border-none ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset sm:text-sm sm:leading-6">
        </div>
      </div>
      <div class="min-w-full sm:min-w-fit">
        <label for="value" class="block text-sm font-medium leading-6 text-zinc-100">Valor devido</label>

        <div class="relative mt-2 rounded-md shadow-sm">
          <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
            <span class="text-zinc-100 sm:text-sm">R$</span>
          </div>
          <input v-model="reactiveForm.value" type="number" name="value" id="value"
            class="block w-36 rounded-md outline-none border-0 py-1.5 pl-9 text-zinc-100 bg-slate-900 ring-1 border-none ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset sm:text-sm sm:leading-6"
            placeholder="0">
        </div>
      </div>
      <button
        class="h-fit inline-flex justify-center rounded-lg text-sm font-semibold py-2 px-6 bg-zinc-100 text-slate-900 hover:bg-slate-300">
        Enviar
      </button>
    </form>

    <div v-if="items.length > 0"
      class="animate__animated animate__fadeInUp mx-auto mt-16 max-w-xl text-zinc-100 flex flex-col-reverse gap-4 justify-center items-center">
      <div v-for="item in items" :key="item.id" class="flex justify-between items-center gap-8">
        <div class="w-[300px]">
          <h4 class="font-semibold text-end">{{ item.name }} </h4>
        </div>
        <div class="w-[300px]">
          <p class="font-semibold">R$ {{ item.value }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

html,
body,
#__nuxt,
#__layout {
  width: 100vw !important;
  background-color: #0f172a;
  overflow-x: hidden;
  overflow-y: scroll;
}

body {
  margin-bottom: 10vh;
}

h1 {
  font-family: 'Roboto', sans-serif;
}
</style>