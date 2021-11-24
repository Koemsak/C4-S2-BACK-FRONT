<template>
  <section>
    <header>
      <h1>My books {{getStatus()}} </h1>
    </header>
    <book-form @add-book="addNewBook"></book-form>
    <ul>
      <book-card v-for="book of books" :key="book.id" :book="book" @book-removed="deleteBook" ></book-card>
    </ul>
  </section>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      books: [],
      url: process.env.VUE_APP_API_URL
    };
  },
  methods: {
    getStatus() {
      return process.env.VUE_APP_MODE;
    },
    addNewBook(title, description) {
      let data = {
        title: title,
        description: description
      }
      axios.post(this.url + "/api/books", data).then(response => {
        console.log(response.data.data);
        this.getBooks();
      })
    },
    getBooks() {
      axios.get(this.url + "/api/books").then(response => {
        this.books = response.data
      })
    },
    deleteBook(id) {
      axios.delete(this.url + "/api/books/" + id).then(response => {
        console.log(response.data.data);
        this.books = this.books.filter(book => book.id !== id);
      })
    }
  },
  mounted() {
    this.getBooks();
  },
};
</script>

<style>
:root {
  --main-color: #3a2dfc;
}

* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: var(--main-color);
  color: white;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

button {
  cursor: pointer;
  border: 1px solid var(--main-color);
  background-color: var(--main-color);
  color: white;
  padding: 0.5rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}

button:hover,
button:active {
  box-shadow: 1px 2px 6px rgba(0, 0, 0, 0.26);
}
</style>