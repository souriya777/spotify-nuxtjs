<template>
  <section class="section">
    <h1>Accueil</h1>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.title }}
      </li>
    </ul>
  </section>
</template>
<script>
export default {
  async fetch() {
    this.todos = await fetch(
      `https://jsonplaceholder.typicode.com/todos`
    ).then((res) => res.json())
  },
  data() {
    return {
      todos: []
    }
  }
}
</script>
