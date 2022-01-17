<template>
  <ul class="list cards__container">
    <transition-group name="card-list"
      ><Card
        @cardDeletion="deleteCard"
        @imgLoad="emitEvent"
        v-for="card in cards"
        :key="card.id"
        :card="card"
    /></transition-group>
  </ul>
</template>

<script>
import Card from './Card';

export default {
  components: { Card },
  props: {
    cards: {
      type: Array,
      required: true,
    },
  },
  methods: {
    deleteCard(card) {
      this.$emit('cardDeletion', card);
    },

    emitEvent() {
      this.$emit('imgLoad');
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../sass/mixins.scss';

.cards__container {
  max-width: 1028px;
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  gap: 16px;

  @include respond-to(765px, max) {
    flex-direction: column;
    align-items: center;
  }
}

.card-list-enter-active,
.card-list-leave-active {
  transition: all 0.25s ease;
}

.card-list-enter-from,
.card-list-leave-to {
  opacity: 0;
  transform: translateX(-130px);
}

.card-list-move {
  transition: transform 0.8s ease;
}
</style>
