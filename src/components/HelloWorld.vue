<template>
    <div>
    <form @submit="addBook(title,author,image)">
    <input v-model="title" placeholder="Titre">
    <input v-model="author" placeholder="Auteur">
    <input v-model="image" placeholder="Image URL">
    <button type="submit">Add New Book</button>
    </form>
      <article v-for="(book, idx) in books" :key="idx">
        <button @click="deleteBook(book.id)">Supprimer</button>
        <h1>{{ book.title }} {{ book.author }}</h1>
        <img :src="book.image">
      </article>
    </div>
</template>

<script>
import { db } from '../main'
export default {
  name: 'HelloWorld',
  data () {
    return {
      books: [],
      author: '',
      title: '',
      image: ''
    }
  },
  firestore () {
    return {
      books: db.collection('books').orderBy('createdAt', 'desc')
    }
  },
  methods: {
    addBook (title, author, image) {
      const createdAt = new Date()
      db.collection('books').add({ author, title, image, createdAt })
    },
    deleteBook (id) {
      db.collection('books').doc(id).delete()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
