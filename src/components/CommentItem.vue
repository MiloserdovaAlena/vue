<template>
  <li
    class="item"
    :class="colorReaction"
    :style="{
      'margin-left': nestMargin + 'px', ////считает отступ и подставляет в элементы списка
    }"
  >
    <h3 class="item__author">{{ comment.comment.author }}</h3>
    <p class="item__text">{{ comment.comment.text }}</p>

    <time class="item__createdAt" datetime="{{ formattedDate }}">{{
      formattedDate
    }}</time>
    <my-button class="item__button" @click="showDialog">Ответить</my-button>

    <p v-if="comment.children" class="item__children"> <!--если есть дочерний элемент то он показывает этот класс-->
      <svg
        fill="#000000"
        width="800px"
        height="800px"
        viewBox="0 0 32 32"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
      >
        <title>send-envelope</title>
        <path
          d="M0 25.984v-12q0-2.464 1.76-4.224t4.256-1.76h0.352l0.096 0.288q0.544 1.344 1.344 2.176t2.208 1.184v0.352h-4q-0.672 0-1.28 0.48l11.264 7.52 11.264-7.52q-0.576-0.48-1.248-0.48h-4v-0.352q1.376-0.384 2.176-1.184t1.344-2.176l0.096-0.288h0.384q2.464 0 4.224 1.76t1.76 4.224v12q0 2.496-1.76 4.256t-4.224 1.76h-20q-2.496 0-4.256-1.76t-1.76-4.256zM4 23.968l6.016-1.984-5.408 5.408q0.544 0.608 1.408 0.608h20q0.8 0 1.408-0.608l-5.408-5.408 5.984 2.016v-8.032l-12 8-12-8v8zM10.048 5.632q0.096-0.608 0.544-1.056l4-4q0.576-0.576 1.408-0.576t1.408 0.576l4 4q0.448 0.448 0.544 1.056t-0.096 1.152q-0.224 0.544-0.736 0.896t-1.12 0.32h-1.984v6.016q0 0.832-0.608 1.408t-1.408 0.576-1.408-0.576-0.576-1.408v-6.016h-2.016q-0.608 0-1.12-0.32t-0.736-0.896-0.096-1.152z"
        ></path></svg>
      {{ comment.children }} <!--вывод дочерних элементов(5)-->
    </p>
  </li>
</template>

<script>
export default {
  props: {
    comment: {
      type: Object,
      required: true,
    },
  },
  computed: {
    nestMargin() {
      return 30 * this.comment.nest; //считает отступ(вложенность)
    },
    colorReaction() { //функция для цвета
      if (this.comment.reactionSum > 0) {
        return "item_green";
      } else if (this.comment.reactionSum < 0) {
        return "item_red";
      }
      return "";
    },
    formattedDate() {
      const createdAt = new Date(this.comment.comment.createdAt); //с сервера приходят дата и время
      return createdAt.toLocaleString(); //Метод преобразует дату и время, сохраненные в createdAt
    },
  },
  methods: {
    showDialog() {
      this.$emit("showDialog", this.comment.comment.id);
    },
  },
};
</script>

<style scoped>
li {
  list-style: none;
  position: relative;
}

.item {
  padding: 20px;
  border: 2px solid #007bff;
  margin-top: 15px;
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 123, 255, 0.5);
}

.item_green {
  background-color: #d7f5db;
  border-color: #9fdf9f;
}

.item_red {
  background-color: #f8d7d7;
  border-color: #f49c9c;
}

.item__author {
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 10px;
}

.item__text {
  font-size: 22px;
  line-height: 1.6;
}

.item__createdAt {
  margin-top: 10px;
  font-size: 20px;
  color: #777;
  max-width: 100px;
}

.item__children {
  position: absolute;
  right: 10px;
  top: 10px;
  font-size: 12px;
}

.item__button {
  position: absolute;
  right: 10px;
  bottom: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 18px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 15px;
  margin: 0;
}

.item__button:hover {
  background-color: #0056b3;
}

.item__button:active {
  background-color: #003d80;
}

svg {
  width: 25px;
  height: 25px;
  margin: 5px 5px 0;
}
</style>
