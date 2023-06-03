<template>
    <div>
      <table class="table">
        <thead>
          <tr>
            <th></th>
            <th>Search Order</th>
            <th>Search Input</th>
            <th>Search Result</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="place in paginatedPlaces" :key="place.id">
            <td>
              <input type="checkbox" :value="place.id" v-model="selectedPlaces">
                
            </td>
            <td>{{ place.id }}</td>
            <td>{{ place.LocationName }}</td>
            <td>{{ place.center }}</td>
          </tr>
        </tbody>
      </table>
      <div class="pagination">
        <button :disabled="currentPage === 1" @click="currentPage--">Prev</button>
        <span>{{ currentPage }} / {{ totalPages }}</span>
        <button :disabled="currentPage === totalPages" @click="currentPage++">Next</button>
        <button @click="deleteSelectedPlaces">Delete Selected Places</button>
      </div>
    </div>
  </template>
  
  <script>
export default {
    data() {
      return {
        selectedPlaces: [], // array of selected place IDs
        itemsPerPage: 10,
        currentPage: 1
      }
    },
    props: {
        places: [], // array of all searched places
    },
    computed: {
      totalPages() {
        return Math.ceil(this.places.length / this.itemsPerPage);
      },
      paginatedPlaces() {
        const startIndex = (this.currentPage - 1) * this.itemsPerPage;
        return this.places.slice(startIndex, startIndex + this.itemsPerPage);
      }
    },
    methods: {
    deleteSelectedPlaces() {
      // Iterate over selectedPlaces array and remove corresponding place objects from places array
      this.selectedPlaces.forEach(id => {
        const index = this.places.findIndex(place => place.id === id);
        if (index > -1) {
          this.$emit("remove-marker",this.places[index].center)
          this.places.splice(index, 1);
        }
      });
      this.places.forEach((place, index) => {
        place.id = index + 1;
      });
      // Reset selectedPlaces array
      this.selectedPlaces = [];
    }
  }
}
  </script>
  
  <style scoped>
  .table {
    border-collapse: collapse;
    width: 100%;
  }
  
  .table th,
  .table td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }
  
  .table th {
    background-color: #f2f2f2;
  }
  
  .pagination {
    margin-top: 10px;
  }
  
  .pagination button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 8px 16px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin-right: 5px;
    cursor: pointer;
  }
  
  .pagination button:disabled {
    background-color: #ccc;
    color: #666;
    cursor: not-allowed;
  }
  </style>