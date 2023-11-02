<template>
  <button class="my-button">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "my-button",
};
</script>

<style>
/* Исходные стили кнопки */
.my-button {
  display: inline-block;
  padding: 12px 24px;
  font-size: 16px;
  font-weight: bold;
  text-align: center;
  text-transform: uppercase;
  background-color: #ff5722;
  color: #fff;
  border: 2px solid #ff5722;
  border-radius: 50px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s, transform 0.3s;
}

/* Изменение цвета фона и текста при наведении курсора */
.my-button:hover {
  background-color: #fff;
  color: #ff5722;
}

/* Изменение размера кнопки при нажатии */
.my-button:active {
  transform: scale(0.95);
}
</style>
