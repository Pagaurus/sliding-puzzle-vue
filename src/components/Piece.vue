<script setup>
  const props = defineProps(['image']);

  console.log(props.image);
</script>
<template>

  <!--<div class="w-full h-full relative object-stretch" ref="piece">-->
    <img v-if="image" :src="image" class="w=full h-full rounded-lg select-none" draggable="false"></img>
  
    <div v-else class="w=full h-full rounded-lg select-none" draggable="false"></div>
  
</template>