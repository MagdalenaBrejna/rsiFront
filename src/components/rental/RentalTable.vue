<template>
  <CustomNavbar></CustomNavbar>
  <br><br>
  <div id="rental-table" class="container">
    <h4>Historia Wypożyczeń</h4>
    <table>
      <thead>
      <tr>
        <th>Książka</th>
        <th>Czytelnik</th>
        <th>Data Od</th>
        <th>Data Do</th>
        <th>Data Zwrotu</th>
        <th>Usuń</th>
        <th>Aktualizuj</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(rental, index) in paginatedRentals()" :key="rental.id">
        <td>{{ rental.book.title }}</td>
        <td>{{ rental.reader.lastName }}</td>
        <td>{{ rental.dateRented }}</td>
        <td>{{ rental.dateShouldBeReturned }}</td>
        <td>{{ rental.dateReturned }}</td>
      
        <td>
          <button @click="deleteRental(rental.id)" class="btn btn-danger" style="background-color:#d37676; border-color:#d37676;">Usuń</button>
        </td>
        <td>
          <button class="btn btn-primary" @click="this.$router.push({ name: 'rentalUpdate', params: { id: rental.id } });" style="background-color:#a0a0a0; border-color:#a0a0a0;">Aktualizuj</button>
        </td>
      </tr>
      </tbody>
    </table>
    
    <br>

    <div class="pagination">
      <button class="btn btn-primary" :disabled="currentPage === 1" @click="currentPage--" style="background-color:#2aadcd; border-color:#16b9e1;">Poprzednia</button>
      <span class="ms-3 me-3">Strona {{ currentPage }} z {{ totalPages }}</span>
      <button class="btn btn-primary" :disabled="currentPage === totalPages" @click="currentPage++" style="background-color:#2aadcd; border-color:#16b9e1;">Następna</button>
    </div>

    <br><br>
    <!-- <div class="container">
      <div class="row align-items-center">
        <div class="col">
          <button @click="getCount" class="btn big-btn btn-primary">
              Odśwież dane
          </button>
        </div>
        <div class="col text-left">
          <h6 class="mb-0 ml-3">Liczba wypożyczeń: {{ count }}</h6>
        </div>
      </div>
    </div> -->

    <RentalForm :rentalsSource="rentals" class="mt-5"/>
  </div>

</template>

<script>
import axios from "axios";
import RentalForm from "@/components/rental/RentalForm.vue";
import CustomNavbar from "@/components/utils/CustomNavbar.vue";

export default {
  name: 'RentalTable',
  components: {CustomNavbar, RentalForm},

  data() {
    return {
      rentals: [],
      currentPage: 1,
      itemsPerPage: 2,
      showCount: false,
      count: 0,
    }
  },

  mounted() {
    this.getRentals()
  },
  methods: {
    async getRentals() {
      try {
        const response = await fetch('http://localhost:8081/rentals')
        const data = await response.json()
        this.rentals = data
      } catch (error) {
        console.error(error)
      }
    },
    paginatedRentals() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.rentals.slice(startIndex, endIndex);
    },
    async deleteRental(itemId) {
      axios.delete(`http://localhost:8081/rentals/${itemId}`)
          .then(response => {
            // Obsługa odpowiedzi serwera
            console.log(response.data);
          })
          .catch(error => {
            // Obsługa błędów
            this.$router.push({ name: 'ErrorPageRU', params: { id: error } });
            console.error(error);
          });
      
      this.rentals = this.rentals.filter(obj => {
        return obj.id !== itemId
      });
    },

    async getCount() {
      try {
        const response = await fetch('http://localhost:8081/rentals/count')
        const data = await response.json()
        console.log(data)
        this.count = data
        return this.count
      } catch (error) {
        console.error(error)
      }
    },
    toggleCount() {
      this.showCount = !this.showCount;
      
      if (this.showCount) {
        this.getCount();
      }
    },

  },
  computed: {
    totalPages() {
      return Math.ceil(this.rentals.length / this.itemsPerPage);
    }
  },
}
</script>

<style scoped>

body {
  background-color: lightgrey;
  color: black;
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

.big-btn {
  background-color:#2aadcd;
  border-color:#16b9e1; 
  height: 30px; 
  width: 230px; 
  font-size: 16px !important;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn:hover, .big-btn:hover {
  background-color:#47494a !important;
  border-color:#47494a !important; 
}

.text-right {
  text-align: right;
}
</style>