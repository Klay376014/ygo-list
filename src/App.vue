<script setup>
import { ref, onMounted } from "vue";

const getStoredList = () => {
  const storedListJSON = localStorage.getItem('cardList');
  const storedList = JSON.parse(storedListJSON);
  return Array.isArray(storedList) ? storedList : [];
};

const list = ref([]);
const newCard = ref({ cardNumber: "", cardQuantity: 0, user: "" });

const addCard = () => {
  if (
    newCard.value.cardNumber !== "" &&
    parseInt(newCard.value.cardQuantity) > 0 &&
    newCard.value.user !== ""
  ) {
    list.value.push({ ...newCard.value });
    newCard.value = { cardNumber: "", cardQuantity: 0, user: "" };
    updateLocalStorage();
  }
};

const src = "http://cardimage.cardbox.sc/SC/";

const deleteCard = (index) => {
  const confirmation = confirm("確定刪除此卡片？");
  if (confirmation) {
    list.value.splice(index, 1);
    updateLocalStorage();
  }
};

const updateLocalStorage = () => {
  localStorage.setItem("cardList", JSON.stringify(list));
};

onMounted(() => {
  list.value = getStoredList();
});

</script>

<template>
  <div class="container">
    <h1>輸入卡片資訊</h1>
    <form @submit.prevent="addCard">
      <div>
        <label for="cardNumber">卡片號碼：</label>
        <input type="text" v-model="newCard.cardNumber" id="cardNumber" required />
      </div>
      <div>
        <label for="cardQuantity">卡片張數：</label>
        <input type="number" v-model="newCard.cardQuantity" id="cardQuantity" max="3" required />
      </div>
      <div>
        <label for="user">　填表人：</label>
        <input type="text" v-model="newCard.user" id="user" required />
      </div>
      <button type="submit">新增</button>
    </form>

    <div v-if="list.length > 0" class="list">
      <h2>卡片清單</h2>
      <ul>
        <li v-for="(card, index) in list" :key="index">
          <div>
            <img :src="src + card.cardNumber + '.jpg'" alt="" />
          </div>
          <div class="content">
            <strong>卡片張數：{{ card.cardQuantity }}</strong>

            <strong>填表人：{{ card.user }}</strong>
            <button @click="deleteCard(index)">刪除</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped></style>
