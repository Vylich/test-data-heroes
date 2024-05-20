<script setup>
import Card from "./components/Card.vue";
import { onMounted, ref } from "vue";
import axios from "axios";

const characters = ref([]);
const infoPages = ref({});
const name = ref("");
const status = ref("");

onMounted(() => {
  getCards();
});

const getPagination = (type) => {
  const typeUrl = type === "next" ? infoPages.value.next : infoPages.value.prev;
  axios
    .get(typeUrl)
    .then((res) => {
      characters.value = res.data.results;
      infoPages.value = res.data.info;
    })
    .catch((err) => {
      console.log(err);
    });
};

const onChangeInput = (e) => {
  name.value = e.target.value;
};

const onChangeSelect = (e) => {
  status.value = e.target.value;
};

const submitSort = () => {
  axios
    .get(
      `https://rickandmortyapi.com/api/character/?name=${name.value}&status=${status.value}`,
    )
    .then((res) => {
      characters.value = res.data.results;
      infoPages.value = res.data.info;
    })
    .catch((err) => {
      characters.value = [];
      infoPages.value = {};
      console.log(err);
    });
};

const getCards = () => {
  status.value = "";
  name.value = "";
  axios
    .get("https://rickandmortyapi.com/api/character")
    .then((res) => {
      characters.value = res.data.results;
      infoPages.value = res.data.info;
    })
    .catch((err) => {
      console.log(err);
    });
};
</script>

<template>
  <div class="sort">
    <h2>Фильтрация</h2>
    <div class="wrapper">
      <label for="name">Имя</label>
      <input :value="name" @change="onChangeInput" type="text" name="name" />
    </div>
    <div class="wrapper">
      <label for="status">Статус</label>
      <select
        @change="onChangeSelect"
        :value="status"
        name="status"
        id="select-status"
      >
        <option value="">Все</option>
        <option value="alive">Живые</option>
        <option value="dead">Мертвые</option>
        <option value="unknown">Неизвестно</option>
      </select>
    </div>
    <div class="btnWrap">
      <button @click="getCards">Отменить</button>
      <button @click="submitSort">Применить</button>
    </div>
  </div>
  <div class="container">
    <Card
      v-for="character in characters"
      :key="character.id"
      :character="character"
    />

    <span v-if="characters.length === 0">Ничего не найдено</span>
  </div>
  <div class="pagination">
    <button v-if="infoPages.prev" @click="getPagination('prev')">Назад</button>
    <button v-if="infoPages.next" @click="getPagination('next')">Вперед</button>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  width: 100%;
  min-height: 100vh;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
.wrapper {
  display: flex;
  gap: 10px;
}
.sort {
  display: flex;
  flex-direction: column;
  align-items: end;
  gap: 20px;
  margin: 20px;

  h2 {
    margin: 0;
  }
}
.pagination {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin: 20px;
}

.btnWrap {
  display: flex;
  gap: 10px;
}
</style>
