<template>
  <li class="card" v-show="isLoaded">
    <img
      class="card__img"
      :src="card.imgSrc"
      alt="Card image"
      @error="showAltImg"
      @load="showCard"
    />
    <h2 class="text card__title">{{ card.title }}</h2>
    <p class="text card__desc">
      {{ card.desc }}
    </p>
    <p class="text card__price">{{ card.price }} руб.</p>
    <button @click="deleteCard" class="btn card__delete-btn"></button>
  </li>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      isLoaded: false,
    };
  },
  methods: {
    showCard() {
      this.isLoaded = true;
    },

    showAltImg(event) {
      event.target.src = '/assets/images/default-card-img.jpg';
    },

    deleteCard() {
      this.$emit('cardDeletion', this.card);
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../sass/mixins.scss';

.card {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 332px;
  width: 100%;
  padding-bottom: 24px;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;

  @include respond-to(765px, max) {
    max-width: unset;

    padding-top: 24px;
  }

  &__delete-btn {
    position: absolute;
    top: -8px;
    right: -8px;
    width: 40px;
    height: 40px;
    background-image: url('../assets/icons/btn-delete.svg');
    opacity: 0;
    transition: opacity 0.3s cubic-bezier(0.39, 0.58, 0.57, 1);
  }

  &__img {
    padding-bottom: 16px;
  }

  &__title {
    padding: 0 0 16px 16px;
    font-weight: 600;
    font-size: 20px;
    line-height: 25px;
    overflow-wrap: anywhere;
  }

  &__desc {
    max-width: 300px;
    width: 100%;
    padding: 0 0 32px 16px;
    overflow-wrap: anywhere;

    @include respond-to(765px, max) {
      max-width: 332px;
    }
  }

  &__price {
    padding: 0 0 0 16px;
    font-weight: 600;
    font-size: 24px;
    line-height: 30px;
    overflow-wrap: anywhere;
  }

  &:hover {
    .card__delete-btn {
      opacity: 1;
    }
  }
}
</style>
