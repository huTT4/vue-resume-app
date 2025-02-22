<template>
  <form class="card card-w30" @submit.prevent="submit" @keyup.enter="submit">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="type">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model="value"></textarea>
    </div>

    <button class="btn primary" :disabled="!isValid">Добавить</button>
  </form>
</template>

<script>
export default {
  emits: {
    'block-added': (block) => {
      return (
        typeof block === 'object' &&
        typeof block.type === 'string' &&
        typeof block.value === 'string' &&
        typeof block.id === 'string'
      )
    }
  },
  data () {
    return {
      type: 'title',
      value: ''
    }
  },
  methods: {
    async submit () {
      if (this.isValid) {
        const block = {
          type: this.type,
          value: this.value
        }

        const resp = await fetch('https://vue-with-http-4-default-rtdb.europe-west1.firebasedatabase.app/blocks.json', {
          method: 'POST',
          headers: {
            'Content-Type': 'application-json'
          },
          body: JSON.stringify(block)
        })
        const firebaseData = await resp.json()

        block.id = firebaseData.name

        this.$emit('block-added', block)
        this.type = 'title'
        this.value = ''
      }
    }
  },
  computed: {
    isValid () {
      return this.value.trim().length > 3
    }
  }
}
</script>

<style>
</style>
