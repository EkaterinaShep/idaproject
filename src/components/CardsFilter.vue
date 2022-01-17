<template>
  <div class="filter">
    <select v-model="modelValue" @change="handleOptionChange">
      <option value="default" selected>По умолчанию</option>
      <option value="min">Дешевле</option>
      <option value="max">Дороже</option>
      <option value="title">По названию</option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    modelValue: {
      type: String,
    },
  },

  methods: {
    handleOptionChange(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../sass/mixins.scss';
@import '../sass/variables.scss';

.filter {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: fit-content;

  @include card-effect;

  &::after {
    content: url('../assets/icons/select-arrow.svg');
    position: absolute;
    right: 16px;
    top: 6px;
    pointer-events: none;

    @include respond-to(550px, max) {
      top: 1px;
    }
  }

  & > select {
    -webkit-appearance: none;
    appearance: none;
    padding: 10px 28px 11px 16px;
    font-family: 'Source Sans Pro', sans-serif;
    font-size: 12px;
    line-height: 15px;
    color: #b4b4b4;
    background-color: transparent;
    border: none;
    outline: none;
    cursor: pointer;

    & > option {
      color: #3f3f3f;
      cursor: pointer;
    }

    @include respond-to(550px, max) {
      padding: 5px 28px 5px 16px;
    }

    @include respond-to(400px, max) {
      font-size: 10px;
    }
  }
}
</style>
