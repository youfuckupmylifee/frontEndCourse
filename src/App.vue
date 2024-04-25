<template>
  <div class="container">
    <h1>Узнайте интересные факты о числах!</h1>
    <input type="number" v-model="number" placeholder="Введите число" @keypress.enter="fetchFact" autofocus />
    <select v-model="type">
      <option value="trivia">Случайный факт</option>
      <option value="math">Математика</option>
      <option value="year">Год</option>
    </select>
    <button @click="fetchFact">Поиск</button>
    <div v-if="loading" id="loadingIndicator"></div>
    <div id="factOutput">{{ message }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      number: '',
      type: 'trivia',
      message: '',
      loading: false,
    };
  },
  methods: {
    fetchFact() {
      const url = `http://numbersapi.com/${this.number}/${this.type}?json`;
      this.loading = true;
      this.message = '';

      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.loading = false;
          this.message = data.found ? data.text : `${this.number} - скучное число/скучный год`;
        })
        .catch(error => {
          this.loading = false;
          this.message = 'Произошла ошибка при загрузке данных';
          console.error('Error fetching fact:', error);
        });
    },
  },
};
</script>

<style>
body, html {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  background-color: #f4f4f9;
  margin: 0;
  padding: 20px;
  color: #333;
}

.container {
  background-color: white;
  max-width: 360px;
  margin: 50px auto;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  transition: transform 0.3s ease-in-out;
}

input, select, button {
  width: calc(100% - 20px);
  padding: 10px;
  margin-top: 10px;
  border: 2px solid #ddd;
  border-radius: 4px;
  transition: border-color 0.3s ease;
}

input:focus, select:focus, button:focus {
  border-color: #0056b3;
}

button {
  background-color: #0056b3;
  color: white;
  cursor: pointer;
  font-weight: bold;
  border: none;
}

button:hover {
  background-color: #003580;
}

#loadingIndicator {
  margin-top: 20px;
  border-radius: 50%;
  border-top: 2px solid #3498db;
  width: 30px;
  height: 30px;
  -webkit-animation: spin 2s linear infinite; /* Safari */
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

#factOutput {
  margin-top: 20px;
  font-size: 1.2em;
  color: #333;
}
</style>
