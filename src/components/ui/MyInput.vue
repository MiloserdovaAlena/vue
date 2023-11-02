<template>
  <input
    :value="modelValue"
    @input="updateInput"
    class="add-comment__input"
    type="text"
  />
</template>

<script>
export default {
  name: "my-input",
  props: {
    modelValue: [String, Number],
  },
  methods: {
    updateInput(event) {
      this.$emit("update:modelValue", event.target.value);
    },
  },
};
</script>

<style>
.add-comment__input {
  margin-top: 0.5rem;
  padding: 1rem;
  border-radius: 1rem;
  border: 1px solid rgb(255, 208, 216);
}
</style>
