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
