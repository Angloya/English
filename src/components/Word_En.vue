<template>
<div>
<ul>
    <li v-for="word in words" :key='word'>
  {{word.word}}
    </li>
</ul>
</div>
</template>
<script>
export default {
  name: 'Word_En',
  props: [
    'word', 'front', 'back', 'done', 'active', 'index'
  ]
}
</script>
<style scoped>

</style>
