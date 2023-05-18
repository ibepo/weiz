<template>
  <Teleport to="body">
    <Transition name="fade">
      <div v-show="modalActive"
        class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8 ">
        <Transition name="tada">
          <div v-if="modalActive" class="p-4 bg-white self-start mt-32 max-w-screen-md rounded-xl ">
            <slot />
            <button class="text-white mt-8 bg-weather-primary py-2 px-6" @click="$emit('close-modal')">Close</button>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>

const props = defineProps(
  {
    modalActive: {
      type: Boolean,
      default: false,
    },
  }
)

const emits = defineEmits(["close-modal"])

</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.tada-enter-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}

.tada-leave-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.tada-enter-from {
  opacity: 0;
  transform: scale(0.8);
}

.tada-leave-to {
  transform: scale(0.8);
}
</style>