<template>
  <div class="app">
    <h1>Учет расходов</h1>
    
    <!-- Вывод балансов -->
    <div v-if="history.length">
      <p>Доходы: {{ incomeBalance }} руб.</p>
      <p>Расходы: {{ outcomeBalance }} руб.</p>
      <p>Баланс: {{ totalBalance }} руб.</p>
    </div>
    <p v-else>Вы не совершали финансовых операций.</p>
    
    <ul>
      <!-- Отображение истории транзакций -->
      <li v-for="item in history" :key="item.id">
        {{ item.text }} - {{ item.amount > 0 ? '+' : '' }}{{ item.amount }} руб.
      </li>
    </ul>

    <!-- Форма для добавления новых транзакций -->
    <form @submit.prevent="addTransaction">
      <div>
        <label for="title">Название статьи:</label>
        <input id="title" type="text" v-model.trim="title" placeholder="Название статьи">
      </div>
      <div>
        <label for="amount">Сумма (доход + / расход -):</label>
        <input id="amount" type="number" v-model.number="amount" placeholder="Сумма">
      </div>
      <button type="submit">Добавить</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      history: [],
      title: '',
      amount: 0 
    }
  },
  computed: {
    incomeBalance() {
      // Сумма всех доходных операций
      return this.history.filter(item => item.amount > 0)
                         .reduce((sum, item) => sum + item.amount, 0);
    },
    outcomeBalance() {
      // Сумма всех расходных операций
      return this.history.filter(item => item.amount < 0)
                         .reduce((sum, item) => sum + item.amount, 0);
    },
    totalBalance() {
      // Общий баланс
      return this.history.reduce((sum, item) => sum + item.amount, 0);
    }
  },
  methods: {
    addTransaction() {
      if (this.title.trim() !== '' && this.amount !== 0) {
        const newTransaction = {
          id: Date.now(),
          text: this.title,
          amount: parseFloat(this.amount)
        };
        this.history.push(newTransaction);
        this.title = '';
        this.amount = 0;
      }
    }
  }
}
</script>


<style scoped>
/* Общие стили для приложения */
.app {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  background: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

h1 {
  color: #333;
  text-align: center;
}

/* Стилизация сообщений и балансов */
p {
  font-size: 16px;
  color: #666;
  margin: 10px 0;
}

/* Стилизация списка транзакций */
ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #fff;
  border: 1px solid #eee;
  padding: 10px 15px;
  margin-bottom: 10px;
  border-radius: 5px;
  font-size: 16px;
  color: #333;
}

li:nth-child(odd) {
  background-color: #f1f1f1;
}

/* Стилизация формы */
form {
  margin-top: 20px;
}

input[type="text"], input[type="number"] {
  width: calc(100% - 22px);
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #6d4fa2;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
  border-radius: 4px;
  margin-top: 10px;
}

button:hover {
  background-color: #543982;
}

label {
  font-weight: bold;
  color: #333;
}
</style>