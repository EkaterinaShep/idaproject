<template>
  <div class="form" :class="{ modal: isModal }">
    <form @submit="createCard">
      <label class="form__label form__label--required" for="card-title"
        >Наименование товара</label
      >
      <input
        v-model="card.title"
        class="input form__input"
        id="card-title"
        type="text"
        placeholder="Введите наименование товара"
        required
      />
      <label class="form__label" for="card-desc">Описание товара</label>
      <textarea
        v-model="card.desc"
        class="input form__input form__input-card-desc"
        id="card-desc"
        type="text"
        placeholder="Введите описание товара"
      ></textarea>
      <label class="form__label form__label--required" for="card-img-src"
        >Ссылка на изображение товара</label
      >
      <input
        v-model="card.imgSrc"
        class="input form__input"
        id="card-img-src"
        type="text"
        placeholder="Введите ссылку"
        required
      />
      <label class="form__label form__label--required" for="card-price"
        >Цена товара</label
      >
      <input
        v-model="price"
        class="input form__input"
        id="card-price"
        type="text"
        @keypress="isNumber"
        @input="handleNum"
        placeholder="Введите цену"
        required
      />
      <button class="btn form__btn" type="submit" :disabled="disabledBtn">
        Добавить товар
      </button>
    </form>
    <button class="btn hamburger__btn" @click="toggleModal">
      {{ habmbBtnContent }}
    </button>
  </div>
</template>

<script>
import { uuid } from 'vue-uuid';

export default {
  data() {
    return {
      card: {
        id: '',
        title: '',
        desc: '',
        imgSrc: '',
        price: '',
      },

      isModal: false,
      price: '',
      habmbBtnContent: 'Добавить товар',
      disabledBtn: true,
    };
  },

  methods: {
    isNumber(event) {
      if (event.charCode >= 48 && event.charCode <= 57) {
        return true;
      }

      event.preventDefault();
    },

    handleNum() {
      const priceWithoutSpaces = +this.price.replace(/\s/g, '');

      if (isNaN(priceWithoutSpaces)) {
        this.price = '';
        return;
      }

    

      this.price = new Intl.NumberFormat('ru-RU').format(priceWithoutSpaces);
      this.card.price = this.price;
    },

    createCard(event) {
      event.preventDefault();

      this.card.id = uuid.v4();
      this.$emit('cardCreation', this.card);
      this.card = {
        id: '',
        title: '',
        desc: '',
        imgSrc: '',
        price: '',
      };
      this.price = '';

      this.toggleModal();
    },

    toggleModal() {
      if (window.innerWidth > 765) {
        return;
      }

      if (this.isModal) {
        this.isModal = false;
        this.habmbBtnContent = 'Добавить товар';
        return;
      }

      this.isModal = true;
      this.habmbBtnContent = 'X';
    },
  },

  watch: {
    card: {
      handler() {
        if (this.card.title && this.card.imgSrc && this.card.price) {
          this.disabledBtn = false;
          return;
        }

        this.disabledBtn = true;
      },
      deep: true,
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../sass/variables.scss';
@import '../sass/mixins.scss';

.form {
  position: sticky;
  top: #{$gap};
  align-self: flex-start;
  max-width: 332px;
  width: 100%;
  padding: 24px;

  @include card-effect(
    (0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02))
  );

  @include respond-to(765px, max) {
    flex-basis: 15%;
    display: flex;
    max-width: unset;
    padding: 0;
  }

  & > form {
    @include respond-to(765px, max) {
      width: 0;
      pointer-events: none;
      opacity: 0;
    }
  }

  &__label {
    position: relative;
    display: inline-block;
    padding-bottom: 4px;
    font-size: 10px;
    line-height: 13px;
    letter-spacing: -0.02em;
    color: #49485e;

    &--required {
      &::after {
        position: absolute;
        top: 0;
        content: '';
        width: 4px;
        height: 4px;
        background: #{$red};
        border-radius: 4px;
      }
    }
  }

  &__input {
    width: 100%;
    padding: 10px 16px 11px;
    margin-bottom: #{$gap};
    font-size: 12px;
    line-height: 15px;

    @include card-effect;

    &-card-desc {
      min-height: 108px;
      overflow: hidden;
      resize: none;
    }

    &::placeholder {
      color: #b4b4b4;
    }
  }

  &__btn {
    width: 100%;
    padding: 10px 0 11px;
    margin-top: 6px;
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    font-size: 12px;
    line-height: 15px;
    text-align: center;
    letter-spacing: -0.02em;
    color: #ffffff;
    background: #7bae73;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
    border-radius: 10px;

    &:disabled {
      color: #b4b4b4;
      background: #eeeeee;
      box-shadow: none;
      cursor: unset;
    }
  }
}

.hamburger__btn {
  width: 1px;
  font-family: 'Inter', sans-serif;
  font-weight: 600;
  font-size: 12px;
  line-height: 15px;
  opacity: 0;

  @include respond-to(765px, max) {
    width: unset;
    opacity: 1;
  }
}

.modal {
  position: absolute;
  z-index: 1000;
  top: 0;
  left: 0;
  display: block;
  padding: 24px;

  & > form {
    width: 100%;
    opacity: 1;
    pointer-events: unset;
  }

  & > .hamburger__btn {
    position: absolute;
    top: 15px;
    right: 15px;
    font-size: 30px;
    line-height: unset;
  }
}
</style>
