<script setup>
  import { ref, getCurrentInstance, onMounted } from 'vue';

  let props = defineProps(['piece', 'direction']);

  let highlighted = ref(false);

  function highlight(){
    highlighted.value = true;
  }
  
  function unhighlight(){
    highlighted.value = false;
  }

  defineExpose({
    highlight,
    unhighlight
  })
</script>
<template>
  <Transition
    :style="'--move-x: ' + (props.direction.x * 133) + 'px; --move-y: ' + (props.direction.y * 133) + 'px'"
    name="fade"
    mode="out-in"
  >
    <component
      :is="piece"
      class="aspect-square max-w-full max-h-full rounded-lg object-cover"
      :class='{
        "outline-6 outline-[#00FF00] -outline-offset-4": highlighted
      }'
    />
  </Transition>
</template>
<style>
  .fade-leave-from {
    display: none;
  }

  .fade-enter-active {
    transition: all 0.17s;
  }

  .fade-enter-from{
    transform: translate(var(--move-x), var(--move-y));
  }
</style>