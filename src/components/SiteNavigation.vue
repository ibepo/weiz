<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6 ">
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3 ">
          <i class="fa-solid fa-sun text-2xl"></i>
          <i class="fa-solid fa-traffic-cone text-2xl"></i>
          <p class="text-2xl font-Roboto">The Local Weather</p>
        </div>
      </RouterLink>
      <div class="flex gap-3 flex-1 justify-end">
        <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
          @click="toggleModal"></i>
        <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"></i>
      </div>
      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl">About</h1>
          <h2 class="text-xl font-light mt-2 ">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Consectetur
            veritatis
            blanditiis
            atque eius reiciendis est natus, debitis itaque assumenda sapiente modi, porro vero voluptate id dignissimos,
            exercitationem numquam optio facilis?</h2>
          <h1 class="text-2xl mt-2 ">section2</h1>
          <h2 class="text-xl font-light mt-1 ">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Consectetur
            veritatis
            blanditiis
            atque eius reiciendis est natus, debitis itaque assumenda sapiente modi, porro vero voluptate id dignissimos,
            exercitationem numquam optio facilis?</h2>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue"
import { RouterLink } from 'vue-router';
import BaseModal from './BaseModal.vue';

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value
}

</script>
