<template>
  <CustomNavbar></CustomNavbar>
  <br>
  <div id="person-form" class="container">
    <h4>Zmień dane autora książki</h4>
    <form @submit.prevent="handleSubmit">

      <label>Imię</label>
      <input
        v-model="author.firstName"
        type="text"
        :class="{ 'has-error': submitting && invalidFirstName }"
        @focus="clearStatus"
        @keypress="clearStatus"
        :placeholder="author.firstName"
      />

      <label>Nazwisko</label>
      <input
        v-model="author.lastName"
        type="text"
        :class="{ 'has-error': submitting && invalidLastName }"
        @focus="clearStatus"
        :placeholder="author.lastName"
      />

      <p v-if="error && submitting" class="error-message">
        Proszę wypełnić wskazane pola formularza
      </p>

      <p v-if="success" class="success-message">
        Dane poprawnie zapisano
      </p>

      <br>

      <button class="btn btn-primary" style="background-color:#2aadcd; border-color:#16b9e1; margin-left: 90px">Zmień</button>
    </form>
  </div>
</template>
<script>

import axios from "axios";
import CustomNavbar from "@/components/utils/CustomNavbar.vue";

export default {
  name: 'AuthorUpdate',
  components: {CustomNavbar},
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      author: {
        id: 0,
        firstName: '',
        lastName: '',
      },
    }
  },
  mounted() {
    this.getAuthor()
  },
  methods: {
    async postData() {
      try {
        const response = await axios.put(`http://localhost:8081/authors/${this.authorId()}`, {
              id: this.author.id,
              firstName: this.author.firstName,
              lastName: this.author.lastName,
            }
        )
        console.log(response.data)
        this.$router.push({ name: 'authors'});
      } catch (error) {
        console.error(error)
      }
    },

    async getAuthor() {
      axios.get(`http://localhost:8081/authors/${this.authorId()}`)
          .then(response => {
            // Obsługa odpowiedzi serwera
            console.log(response.data);
            this.author.id = response.data.id,
            this.author.firstName = response.data.firstName;
            this.author.lastName = response.data.lastName;
          })
          .catch(error => {
            // Obsługa błędów
            console.error(error);
          })
    },

    authorId() {
      return this.$route.params.id;
    },

    handleSubmit() {
      this.submitting = true
      this.clearStatus()

      if (this.invalidFirstName || this.invalidLastName) {
        this.error = true
        return
      }

      this.postData()
      this.author = {
        firstName: '',
        lastName: '',
      }

      this.error = false
      this.success = true
      this.submitting = false
    },
    
    clearStatus() {
      this.success = false
      this.error = false
    },

  },
  computed: {
    invalidFirstName() {
      return this.author.firstName === ''
    },

    invalidLastName() {
      return this.author.lastName === ''
    },
  },
}
</script>
<style scoped>
  form {
    margin-bottom: 2rem;
  }

  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }

  input {
    width: 310px;
    height: 30px;
  }

  h4 {
    color: black;
    size: 20px;    
  }

  .btn {
    background-color:#2aadcd;
    border-color:#16b9e1; 
    height: 30px; 
    width: 100px; 
    font-size: 16px !important;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .btn:hover {
    background-color:#47494a !important;
    border-color:#47494a !important; 
  }
</style>