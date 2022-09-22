<template>
    <div class="container">
            <h1>Books</h1>
            <table class="table">
                <thead class="thead-dark">
                    <tr>
                      <th scope="col">Book Name</th>
                      <th scope="col">ISBN</th>
                      <th scope="col">Author</th>
                    </tr>
                </thead>
                  <tbody>
                    <tr v-for="book in books" :key="book.id">
                      <td>{{ book.name }}</td>
                      <td>{{ book.isbn }}</td>
                      <td>{{ book.author }}</td>
                      <td><button class="btn btn-primary btn-small" @click="updateBook(book)">Edit</button></td>
                    </tr>
                  </tbody>
            </table>
        
        <div class="add_book">
          <form v-on:submit.prevent="submitForm">
            <div class="form-group">
              <label for="name">Book Name</label>
              <input type="text" class="form-control" id="name" v-model="name" />
            </div>
            <div class="form-group">
              <label for="isbn">ISBN</label>
              <input type="text" class="form-control" id="isbn" v-model="isbn" />
            </div>
            <div class="form-group">
              <label for="author">Author</label>
              <input type="text" class="form-control" id="author" v-model="author" />
            </div>
            
            <div class="form-group">
              <button type="submit">Add Book</button>
            </div>
          </form>
        </div>
    </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
import axios from "axios";

    export default {
        data() {
            return {
                books: [],
                name: "",
                isbn: "",
                author:"",
            }
        },
        methods: {
            async getData() {
                try {
                    const response = await axios.get('https://wajesmart.herokuapp.com/api/books');
                    this.books = response.data; 
                    console.log(this.books);
                } catch (error) {
                    console.log(error);
                }
            },

            async submitForm() {
              try {
                const response = await axios.post('https://wajesmart.herokuapp.com/api/books', {
                    name: this.name,
                    isbn: this.isbn,
                    author: this.author,
                  }
                );
                this.books.push(response.data);
                this.name = "";
                this.isbn = "";
                this.author = "";
              } catch (error) {
                console.log(error);
              }
            },

            async updateBook(book){
                try{
                const response = await axios.put('http://wajesmart.herokuapp.com/api/books/${book.id}', {
                        name: book.name,
                        isbn: this.isbn,
                        author: this.author,
                    });
                    let bookIndex = this.books.findIndex(t => t.id === book.id);
                    this.books = this.books.map((book) => {
                        if(this.books.findIndex(t => t.id === book.id) === bookIndex){
                            return response.data;
                        }
                        return book;
                    });

                }catch(error){
                    console.log(error);
                }
            }
        },
        created() {
            this.getData();
        }
    }
</script>
<style scoped>
  
</style>