<template>
  <div class="input">
    <div class="text-field">
      <input
        class="text-field__input"
        type="text"
        placeholder="Search"
        :value="modelValue"
        @input="onInput"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "Input",
  props: {
    modelValue: {
      type: String,
      default: ''
    }
  },
  methods: {
    onInput(event) {
      this.$emit('input', event.target.value)
    }
  }
}
</script>

<style lang="scss" scoped>
.input {
  .text-field {
    margin-bottom: 1rem;

    &__input {
      margin: 0 auto;
      display: block;
      width: 60%;
      height: calc(2.25rem + 2px);
      padding: 0 15px;
      font-family: inherit;
      font-size: 1rem;
      font-weight: 400;
      line-height: 1.5;
      color: #212529;
      background-color: #fff;
      background-clip: padding-box;
      border: 1px solid #bdbdbd;
      border-radius: 0.25rem;
      transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    }

    &__input::placeholder {
      color: #212529;
      opacity: 0.4;
    }

    &__input:focus {
      color: #212529;
      background-color: #fff;
      border-color: #bdbdbd;
      outline: 0;
      box-shadow: 0 0 0 0.2rem rgba(158, 158, 158, 0.25);
    }

    &__input:disabled,
    &__input[readonly] {
      background-color: #f5f5f5;
      opacity: 1;
    }
  }
}
</style>
