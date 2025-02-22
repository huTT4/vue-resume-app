<template>
  <div class="card card-w70">
    <template v-if="blocks.length">
      <component
        v-for="block in blocks"
        :key="block.id"
        :is="'resume-' + block.type"
        :value="block.value"
      ></component>
    </template>
    <!-- <template v-if="blocks.length" v-for="block in blocks" :key="block.id">
      <resume-title v-if="block.type === 'title'" :value="block.value"></resume-title>
      <resume-subtitle v-else-if="block.type === 'subtitle'" :value="block.value"></resume-subtitle>
      <resume-text v-else-if="block.type === 'text'" :value="block.value"></resume-text>
      <resume-avatar v-else-if="block.type === 'avatar'" :value="block.value"></resume-avatar>
    </template> -->
    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
import ResumeTitle from './parts/ResumeTitle'
import ResumeSubtitle from './parts/ResumeSubtitle'
import ResumeText from './parts/ResumeText'
import ResumeAvatar from './parts/ResumeAvatar'

export default {
  props: {
    blocks: {
      type: Array,
      required: true
    }
  },
  components: { ResumeTitle, ResumeSubtitle, ResumeText, ResumeAvatar }
}
</script>

<style>

</style>
