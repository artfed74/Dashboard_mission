<template>
  <div class="main-container">
    <div class="header">
      <router-link to="/form" class="add-link">
        <button class="add-button">Добавить</button>
      </router-link>
      <label class="switch">
        <input type="checkbox" v-model="isDragEnabled">
        <span class="slider"></span>
      </label>
      <div class="text">
         Click&drag
      </div>
    </div>
    <div
      class="card-container"
      @dragover.prevent
      @drop="handleDrop"
    >
      <div
        v-for="card in filteredCards"
        :key="card.id"
        class="card"
        :data-id="card.id"
        :draggable="isDragEnabled"
        @dragstart="isDragEnabled ? handleDragStart(card) : null"
        @dragover="isDragEnabled ? handleDragOver : null"
        @dragend="isDragEnabled ? handleDragEnd : null"
      >
        <div class="card-content">
          <h3>{{ card.title }}</h3>
          <p>{{ card.description }}</p>
        </div>
        <button class="delete-dashboard" @click="deleteCard(card.id)">Удалить</button>
      </div>
    </div>
  </div>
</template>

<script>
import jsonData from '../../data.json';

export default {
  name: "HomePage",
  data() {
    return {
      cards: jsonData,
      draggedCard: null,
      isDragEnabled: true,
    };
  },
  mounted() {
    const localCards = JSON.parse(localStorage.getItem("cards"));
    if (localCards) {
        this.cards = localCards;    
    } else {
        this.cards = jsonData;
        this.saveCardsToLocalStorage(); 
    }
},
  computed: {
    filteredCards() {
      return this.cards.filter(card => card !== null);
    }
  },
  methods: {
    handleDragStart(card) {
      this.draggedCard = card;
    },
    handleDragOver(e) {
      e.preventDefault();
    },
    handleDrop(e) {
      if (!this.draggedCard) {
        return;
      }
      const indexToInsert = Array.from(e.target.parentElement.children).indexOf(e.target);
      const draggedIndex = this.cards.indexOf(this.draggedCard);
      this.cards.splice(draggedIndex, 1);
      this.cards.splice(indexToInsert, 0, this.draggedCard);
      this.saveCardsToLocalStorage();
      this.draggedCard = null;
    },
    handleDragEnd() {
      this.saveCardsToLocalStorage();
    },
    saveCardsToLocalStorage() {
      localStorage.setItem("cards", JSON.stringify(this.cards));
    },
    deleteCard(cardId) {
      if (confirm("Вы точно хотите удалить выбранную карточку?")) {
        this.cards = this.cards.filter(card => card.id !== cardId);
        this.saveCardsToLocalStorage();
      }
    }
  }
};
</script>

<style scoped>
.main-container {
  max-width: 1800px;
  margin: 0 auto;
  padding: 20px;
}

.header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.add-link {
  text-decoration: none;
  margin-right: 15px;
}

.add-button {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.add-button:hover {
  background-color: #45a049;
}

/* контейнр карт */
.card-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  margin-top: 30px;
}

/* карта */
.card {
    position: relative;
    width: calc(100% - 20px); 
    height: 120px; 
    background-color: #ffffff;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    cursor: grab;
    transition: box-shadow 0.3s;
}
.card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.card-content {
  margin-bottom: 10px;
}

.card h3 {
  margin: 0 0 10px;
  font-size: 20px;
}

.card p {
  margin: 0;
  color: #666;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2; 
  -webkit-box-orient: vertical;
  word-break: break-word; 
  font-size: 20px;
}


.delete-dashboard {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: rgb(248, 30, 30);
  border: none;
  color: white;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
}

.delete-dashboard:hover {
  background-color: rgb(219, 23, 23);
}


.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
  margin-left: 30px;
  
}

.switch input {
  display: none;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  transition: background-color 0.4s;
  border-radius: 34px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  transition: transform 0.4s;
  border-radius: 50%;
}

input:checked + .slider {
  background-color: #4caf50;
}

input:checked + .slider:before {
  transform: translateX(26px);
}

.text {
  font-size: 18px;
  color: #333;
  margin-left: 10px;
}
</style>
