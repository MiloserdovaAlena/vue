<template>
  <form class="form" @submit.prevent="createComment">
    <h2 class="form__title">Создание постов</h2>
    <label class="form__label form__label--text">
      <span class="form__label-text">Введите название</span>
      <my-input
        v-model.trim="formData.author"
        class="form__input form__input--focus"
        inputType="text"
        inputName="author"
        required
        placeholder="Название"
      />
    </label>
    <label class="form__label form__label--text">
      <span class="form__label-text">Введите комментарий</span>
      <my-input
        class="form__input"
        placeholder="Описание"
        v-model="formData.text"
        textareaName="text"
        required
      />
    </label>
    <fieldset class="form__fieldset">
      <legend class="form__legend">Реакция:</legend>
      <label class="form__label">
        <input
          v-model="formData.reaction"
          type="radio"
          name="reaction"
          value="1"
          class="form__radio visually-hidden"
          required
        />
        <span class="form__radio-icon">
          <svg
            width="800px"
            height="800px"
            viewBox="0 0 1024 1024"
            class="icon"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M192 938.7H87.9c-48.4 0-87.9-39.5-87.9-88V386.6c0-48.5 39.5-87.9 87.9-87.9h125.4c11.6 0 22.7 4.7 30.8 13.1 8 8.4 12.3 19.6 11.9 31.2l-21.3 554.7c-0.9 22.8-19.8 41-42.7 41zM87.9 384c-1.4 0-2.6 1.2-2.6 2.6v464.1c0 1.4 1.2 2.6 2.6 2.6h63L169 384H87.9z"
              fill="#5F6379"
            />
            <path
              d="M810.4 938.7H275.7l24.6-640H418l72-201.8C510.7 38.9 566 0 627.5 0c42.4 0 82.6 18.4 110.3 50.4S778 124.8 772 166.7l-18.9 132h142.6c70.7 0 128.2 57.5 128.2 128.2l-1 9.3-84.4 379.4c-2.6 68.3-59.1 123.1-128.1 123.1z m-446.1-85.4h446.1c23.6 0 42.9-19.2 42.9-42.9l1-9.3L938.5 423c-2-21.8-20.4-39-42.7-39h-241l32.8-229.4c2.5-17.7-2.5-34.8-14.2-48.3s-28-20.9-45.9-20.9c-25.6 0-48.5 16.2-57.1 40.3L478.1 384h-95.7l-18.1 469.3z"
              fill="#3688FF"
            />
          </svg>
        </span>
        <span class="visually-hidden">Нравится</span>
      </label>
      <label class="form__label">
        <input
          v-model="formData.reaction"
          type="radio"
          required
          name="reaction"
          value="0"
          class="form__radio visually-hidden"
          checked
        />
        <span class="form__radio-icon">
          <svg
            width="800px"
            height="800px"
            viewBox="0 0 48 48"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <rect width="48" height="48" fill="white" fill-opacity="0.01" />
            <path
              d="M24 44C35.0457 44 44 35.0457 44 24C44 12.9543 35.0457 4 24 4C12.9543 4 4 12.9543 4 24C4 35.0457 12.9543 44 24 44Z"
              fill="#2F88FF"
              stroke="#000000"
              stroke-width="4"
              stroke-linejoin="round"
            />
            <path
              d="M31 18V19"
              stroke="white"
              stroke-width="4"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M17 18V19"
              stroke="white"
              stroke-width="4"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M17 31H31"
              stroke="white"
              stroke-width="4"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </span>
        <span class="visually-hidden">Нейтрально</span>
      </label>
      <label class="form__label">
        <input
          v-model="formData.reaction"
          type="radio"
          name="reaction"
          value="-1"
          class="form__radio visually-hidden"
          required
        />
        <span class="form__radio-icon">
          <svg
            width="800px"
            height="800px"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M8 14H4V4H8M8 14V4M8 14L13.1956 20.0615C13.6886 20.6367 14.4642 20.8839 15.1992 20.7002L15.2467 20.6883C16.5885 20.3529 17.1929 18.7894 16.4258 17.6386L14 14H18.5604C19.8225 14 20.7691 12.8453 20.5216 11.6078L19.3216 5.60777C19.1346 4.67292 18.3138 4 17.3604 4H8"
              stroke="#001A72"
              stroke-width="1.5"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </span>
        <span class="visually-hidden">Не нравится</span>
      </label>
    </fieldset>
    <my-button class="form__button">Создать</my-button>
  </form>
</template>

<script>
export default {
  props: {
    parentCommentId: {
      type: Number,
      defualt: null,
    },
  }, //id родительского комментария
  data() {
    return {
      formData: {
        author: "",
        text: "",
        reaction: 0,
        parentId: null,
      },
    };
  },
  methods: {
    createComment() {
      this.formData = {
        author: this.formData.author,
        text: this.formData.text,
        reaction: +this.formData.reaction,
        parentId: this.parentCommentId,
      };
      this.$emit("create", this.formData); //подписка на событие которая перекидывает информацию в AppVue
      this.formData = {
        author: "",
        text: "",
        reaction: 0,
        parentId: null,
      }; //идет обнуление
    },
  },
};
</script>

<style scoped>
.form__label {
  display: flex;
  flex-direction: column-reverse;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.form__label-text {
  margin-left: 10px;
}

.form__input,
.form__textarea {
  padding: 12px;
  border: 2px solid #007bff;
  border-radius: 6px;
}

.form__textarea {
  resize: vertical;
}

.form__fieldset {
  padding: 15px;
  border: 2px solid #007bff;
  border-radius: 6px;
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.form__legend {
  font-weight: bold;
  margin-bottom: 10px;
}

.form__button {
  align-self: center;
  margin-top: 30px;
  background-color: #007bff;
  color: white;
  padding: 15px 30px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.form__button:hover {
  background-color: #0056b3;
}

.form__button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  border: 0;
  clip: rect(0 0 0 0);
}

svg {
  width: 40px;
  height: 40px;
  margin: 0;
}

.form__label--text {
  flex-direction: row;
}

.form__radio:checked + .form__radio-icon {
  border: 2px solid #007bff; /* Change border color to highlight */
  border-radius: 6px; /* Add border radius for rounded highlight */
}

.form__radio:focus ~ .form__radio-icon {
  outline: 2px solid black;
}
</style>
