<template>
  <div>
    <div style="cursor: pointer" @click="$emit('click')">Assignees</div>
    <ul v-if="active">
      <li>A</li>
      <li>B</li>
      <li>C</li>
      <li>D</li>
      <li>E</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      active: false
    };
  }
};
</script>
