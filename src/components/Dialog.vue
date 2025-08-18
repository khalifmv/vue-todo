<script setup>
const props = defineProps({
  open: {
    default: false,
    type: Boolean
  },
  title: {
    default: "Title",
    required: false
  }
})
const emit = defineEmits(['close'])
</script>

<template>
  <Transition
    enter-active-class="transition-opacity duration-200 ease-out"
    enter-from-class="opacity-0"
    enter-to-class="opacity-100"
    leave-active-class="transition-opacity duration-100 ease-in"
    leave-from-class="opacity-100"
    leave-to-class="opacity-0"
  >
    <div
      v-if="props.open" 
      class="flex flex-col items-center justify-center w-full h-full absolute bg-black/50 z-50 top-0 left-0 backdrop-blur-md"
    >
      <Transition
        enter-active-class="transition duration-200 ease-out transform"
        enter-from-class="opacity-0 scale-90"
        enter-to-class="opacity-100 scale-100"
        leave-active-class="transition duration-100 ease-in transform"
        leave-from-class="opacity-100 scale-100"
        leave-to-class="opacity-0 scale-90"
      >
        <div
          v-if="props.open"
          class="flex flex-col rounded-lg bg-white p-4 min-w-[200px] w-[300px]"
        >
          <div class="flex justify-between items-center">
            <p>{{ props.title }}</p>
            <img
              @click="emit('close', true)"
              src="/x-symbol.svg"
              alt="Close"
              class="cursor-pointer w-3 h-3"
            />
          </div>
          <slot></slot>
        </div>
      </Transition>
    </div>
  </Transition>
</template>
