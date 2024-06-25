<template>
  <h1>Добавление записи</h1>
  <div class="form-container">
    <form @submit.prevent="submitCard">
      <div class="form-group">
        <label for="title">Название:</label>
        <input
          type="text"
          id="title"
          v-model="title"
          :maxlength="count1"
          @input="updateCount1"
        />
        <span>Осталось: {{ count1 - title.length }}</span>
      </div>
      <div class="form-group">
        <label for="description">Описание:</label>
        <textarea
          id="description"
          v-model="description"
          :maxlength="count2"
          @input="updateCount2"
        ></textarea>
        <span>Осталось: {{ count2 - description.length }}</span>
      </div>
      <div class="button-group">
        <button type="submit" class="send">Отправить</button>
        <button type="button" class="cancel" @click="goBack">Отмена</button>
      </div>
    </form>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'FormPage',
  data() {
    return {
      title: '',
      description: '',
      count1: 25,
      count2: 75 
    };
  },
  methods: {
    submitCard() {
      // Проверка на заполнение формы
      if (this.title === '' || this.description === '') {
        alert('Заполните все поля');
      } else {
        const newCard = {
          id: uuidv4(),
          title: this.title,
          description: this.description
        };

        let existingCards = JSON.parse(localStorage.getItem('cards')) || [];
        existingCards.push(newCard);
        localStorage.setItem('cards', JSON.stringify(existingCards));
        this.$router.push('/');
      }
    },
    goBack() {
      this.$router.push('/');
    },
  }
};
</script>

<style scoped>
.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60vh;
  padding: 20px;
}

form {
  width: 100%;
  max-width: 500px;
  padding: 30px;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
  display: block;
}

input,
textarea {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  margin-bottom: 10px;
  border-radius: 5px;
  font-size: 16px;
  border: 1px solid #ccc;
  box-sizing: border-box;
  transition: border 0.3s;
}

input:focus,
textarea:focus {
  border: 1px solid #6cd670;
  outline: none;
}

textarea {
  height: 120px;
  resize: none;
}

span {
  font-size: 14px;
      color: #666;
}

.button-group {
  display: flex;
  justify-content: space-between;
}

.send,
.cancel {
  width: 48%;
  padding: 10px;
  border: none;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.3s;
}

.send {
  background-color: #6cd670;
}

.send:hover {
  background-color: #5bca5f;
  transform: scale(1.05);
}

.cancel {
  background-color: #ca2c2c;
}

.cancel:hover {
  background-color: #a00d0d;
  transform: scale(1.05);
}
</style>
