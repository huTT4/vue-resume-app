<template>
  <div class="container column">
    <ResumeForm @block-added="addBlock" />
    <AppLoader v-if="loadingContent" />
    <ResumeView v-else :blocks="blocks" />
  </div>
  <div class="container">
    <AppLoader v-if="loadingComments" />
    <ResumeComments
      v-else
      :comments="comments"
      @load-comments="loadComments"
    />
  </div>
</template>

<script>
import ResumeForm from './components/ResumeForm'
import ResumeView from './components/ResumeView'
import ResumeComments from './components/ResumeComments'
import AppLoader from './components/AppLoader'

export default {
  data () {
    return {
      blocks: [],
      comments: [],
      loadingContent: false,
      loadingComments: false
    }
  },
  mounted () {
    this.load()
  },
  methods: {
    addBlock (block) {
      this.blocks.push(block)
    },
    async loadComments () {
      try {
        this.loadingComments = true
        const resp = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42')
        if (!resp.ok) {
          throw new Error(`Статус: ${resp.status}`)
        }
        this.comments = await resp.json()
        this.loadingComments = false
      } catch (error) {
        console.error('Ошибка!', error.message)
      }
    },
    async load () {
      try {
        this.loadingContent = true
        const resp = await fetch('https://vue-with-http-4-default-rtdb.europe-west1.firebasedatabase.app/blocks.json')
        const firebaseData = await resp.json()
        this.blocks = Object.keys(firebaseData).map(key => {
          return {
            type: firebaseData[key].type,
            value: firebaseData[key].value,
            id: key
          }
        })
        this.loadingContent = false
      } catch (error) {
        this.loadingContent = false
      }
    }
  },
  components: { ResumeForm, ResumeView, ResumeComments, AppLoader }
}
</script>
