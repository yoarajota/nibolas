<template>
  <h1 class="text-zinc-100 text-6xl text-center mt-12">
    Quanto Nibolas deve?
  </h1>
  <h2 class="text-zinc-100 text-2xl text-center mt-6">
    Lista publica das dividas de Nibolas
  </h2>
  <h3 class="text-zinc-100 text-3xl text-center mt-4">
    R$ 0,00
  </h3>

  <div class="mx-auto max-w-3xl">
    <form class="flex gap-4">
      <div>
        <label for="price" class="block text-sm font-medium leading-6 text-zinc-100">Nome</label>

        <div class="relative mt-2 rounded-md shadow-sm">
          <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
            <span class="text-gray-500 sm:text-sm">$</span>
          </div>
          <input type="text" name="price" id="price" class="block w-full rounded-md border-0 py-1.5 pl-7 pr-20 text-zinc-100 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset sm:text-sm sm:leading-6" placeholder="0.00">
        </div>
      </div>
      <div>
        <label for="price" class="block text-sm font-medium leading-6 text-zinc-100">Valor</label>

        <div class="relative mt-2 rounded-md shadow-sm">
          <div class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3">
            <span class="text-gray-500 sm:text-sm">$</span>
          </div>
          <input type="text" name="price" id="price" class="block w-full rounded-md border-0 py-1.5 pl-7 pr-20 text-zinc-100 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset sm:text-sm sm:leading-6" placeholder="0.00">
        </div>
      </div>
      <button class="inline-flex justify-center rounded-lg text-sm font-semibold py-3 px-4 bg-zinc-100 text-bg-slate-900 hover:bg-slate-900">
        Enviar
      </button>
    </form>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

html,body, #__nuxt, #__layout{
  height: 100vh !important;
  width: 100vw !important;
  background-color: #0f172a;
}

h1 {
  font-family: 'Roboto', sans-serif;
}
</style>