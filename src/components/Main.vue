<template>
  <CustomNavbar></CustomNavbar>
  <div class="d-flex justify-content-center p-5 container">
    <div class="text-left">
      <h1>Zanurz się w świecie książek już dziś!</h1>
     

      <main class="mt-5">
        <div class="container">
          <section id="carouselworkers"></section>
          <!--Grid column-->
          <div class="row">
            <div class="col-12">
              <div  class="carousel slide" data-ride="carousel">
                <div class="carousel-inner">
                  <div class="carousel-item active">
                    <div class="row">
                      <div class="col-md-4 mb-3">
                        <div class="card">
                          <img class="img-fluid" src="a.jpg">
                          <div class="card-body">
                            <h4 class="card-title">Dodaj książkę!</h4>
                            <p class="card-text">Tworzenie nowych pozycji
                            </p>
                          </div>
                        </div>
                      </div>
                      <div class="col-md-4 mb-3">
                        <div class="card">
                          <img class="img-fluid" src="b.jpg">
                          <div class="card-body">
                            <h4 class="card-title">Dodaj autora!</h4>
                            <p class="card-text">Dodawanie nowych autorów
                            </p>
                          </div>
                        </div>
                      </div>
                      <div class="col-md-4 mb-3">
                        <div class="card">
                          <img class="img-fluid" src="c.jpg">
                          <div class="card-body">
                            <h4 class="card-title">Zarządzaj!</h4>
                            <p class="card-text">Wypożycz & Zwróć
                            </p>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>

  <div class="d-flex justify-content-center p-5 container">
    <section id="qoute" style="width: 995px;">
      <!--Grid column-->
      <div class="row justify-content-md-center" data-aos="fade-left">
        <div class="card" style="border-radius: 7px;">
          <div class="card-body p-2">
            <div class="text-center mb-4 pb-2">
              <img src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-quotes/bulb.webp" alt="Bulb"
                  width="100">
            </div>
            <figure class="text-center mb-0">
              <blockquote class="blockquote">
                <p class="pb-2">
                  <i class="fas fa-quote-left fa-xs text-primary"></i>
                  <span class="lead font-italic">Czytanie dobrych książek jest niczym rozmowa z najwspanialszymi ludźmi minionych czasów</span>
                  <i class="fas fa-quote-right fa-xs text-primary"></i>
                </p>
              </blockquote>
              <figcaption class="blockquote-footer mb-0">
                Kartezjusz
              </figcaption>
            </figure>
          </div>
        </div>
      </div>
    </section>
  </div>

  <div class="d-flex justify-content-center p-5 container">
      <button v-if="!showMore" @click="toggleShowMore" class="btn btn-primary">Dowiedz się więcej</button>
      <button v-if="showMore" @click="toggleShowMore" class="btn btn-primary">Ukryj statystyki</button>
      <br><br>
  </div>

  <div class="d-flex justify-content-center p-5 container">
      <div v-if="showMore" class="statistics-container">
          <h2 class="statistics-title">Nasza biblioteka w liczbach</h2>
          <div class="statistic-item">Mamy {{ booksCount }} porywających książek!</div>
          <div class="statistic-item">Zgromadziliśmy dzieła {{ authorsCount }} autorów!</div>
          <div class="statistic-item">Powitaliśmy {{ readersCount }} czytelników!</div>
          <div class="statistic-item">Wypożyczyliśmy {{ rentalsCount }} książek!</div>
        </div>
  </div>
  <br><br>
</template>

<script>
  import BookTable from "@/components/book/BookTable.vue";
  import AuthorTable from "@/components/author/AuthorTable.vue";
  import CustomNavbar from "@/components/utils/CustomNavbar.vue";

  export default {
    name: "Main",
    components: {CustomNavbar, AuthorTable, BookTable},
    data() {
      return {
        rentalsCount: 0,
        booksCount: 0,
        authorsCount: 0,
        readersCount: 0,
        showMore: false,
      }
    },
    mounted() {
      this.getCount();
    },
    methods: {
      toggleShowMore() {
        this.getCount();
        this.showMore = !this.showMore;
      },

      async getCount() {
        try {
          const responseRentals = await fetch('http://localhost:8080/rentals/count')
          const dataRentals = await responseRentals.json()
          this.rentalsCount = dataRentals

          const responseBooks = await fetch('http://localhost:8080/books/count')
          const dataBooks = await responseBooks.json()
          this.booksCount = dataBooks

          const responseAuthors = await fetch('http://localhost:8080/authors/count')
          const dataAuthors = await responseAuthors.json()
          this.authorsCount = dataAuthors

          const responseReaders = await fetch('http://localhost:8080/readers/count')
          const dataReaders = await responseReaders.json()
          this.readersCount = dataReaders

          return this.count
        } catch (error) {
          console.error(error)
        }
      },
    }
}
</script>

<style scoped>
  body {
    background-color: lightgrey;
    color: black;
  }
  
  h1 {
    color: black;
    size: 20px;
    margin-left:10px;
    
  }

  .btn {
  background-color:#2aadcd;
  border-color:#16b9e1; 
  height: 50px; 
  width: 200px; 
  font-size: 20px !important;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn:hover {
  background-color:#47494a !important;
  border-color:#47494a !important; 
}

.statistics-container {
  background-color: #f8f9fa;
  border: 1px solid #ced4da;
  width: 50%;
  text-align: center;
  padding: 40px;
  border-radius: 5px;
  margin-bottom: 20px;
}

.statistics-title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
}

.statistic-item {
  font-size: 20px;
  margin-bottom: 5px;
}
</style>