<template>
  <div class="title-filter__container">
    <h1 class="text title">Добавление товара</h1>
    <CardsFilter v-model="selectedSort" class="select" />
  </div>

  <div class="form-cards__container">
    <CardForm @cardCreation="handleCardCreation" />
    <CardsList
      @imgLoad="showCards"
      v-show="!isCardsLoading"
      @cardDeletion="handleCardDeletion"
      :cards="getSortedCards"
    />
    <div v-show="isCardsLoading" class="loader__container">
      <Loader />
    </div>
  </div>
</template>

<script>
import CardForm from './components/CardForm';
import CardsList from './components/CardsList';
import CardsFilter from './components/CardsFilter';
import Loader from './components/UI/Loader';

export default {
  components: { CardForm, CardsList, CardsFilter, Loader },

  data() {
    return {
      cards: [],

      selectedSort: 'default',

      isCardsLoading: false,
    };
  },

  methods: {
    showCards() {
      this.isCardsLoading = false;
    },

    handleCardCreation(card) {
      this.cards.push(card);

      this.addCardsToLocalStorage();
    },

    handleCardDeletion(cardForDeletion) {
      this.cards = this.cards.filter((card) => card.id !== cardForDeletion.id);

      this.addCardsToLocalStorage();
    },

    addCardsToLocalStorage() {
      const parsed = JSON.stringify(this.cards);

      localStorage.setItem('cards', parsed);
    },
  },

  mounted() {
    try {
      this.isCardsLoading = true;

      const cards = localStorage.getItem('cards');

      if (cards) {
        this.cards = JSON.parse(cards);
        return;
      }

      this.isCardsLoading = false;
    } catch (err) {
      console.error(err);
    }
  },

  computed: {
    getSortedCards() {
      switch (this.selectedSort) {
        case 'title':
          return [...this.cards].sort((card1, card2) =>
            card1.title?.localeCompare(card2.title)
          );
        case 'min':
          return [...this.cards].sort((card1, card2) => {
            const price1 = card1.price
              .split('')
              .filter((item) => !/\s/.test(item))
              .join('');
            const price2 = card2.price
              .split('')
              .filter((item) => !/\s/.test(item))
              .join('');

            return parseInt(price1) - parseInt(price2);
          });
        case 'max':
          return [...this.cards]
            .sort((card1, card2) => {
              const price1 = card1.price
                .split('')
                .filter((item) => !/\s/.test(item))
                .join('');
              const price2 = card2.price
                .split('')
                .filter((item) => !/\s/.test(item))
                .join('');

              return parseInt(price1) - parseInt(price2);
            })
            .reverse();
        default:
          return [...this.cards];
      }
    },
  },
};
</script>

<style lang="scss">
@import './sass/normalize.scss';
@import './sass/base.scss';
@import './sass/typography.scss';
@import './sass/variables.scss';
@import './sass/mixins.scss';

.app {
  max-width: #{$max-width};
  width: 100%;

  .title-filter__container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: #{$gap};

    .title {
      font-weight: 600;
      font-size: 28px;
      line-height: 35px;

      @include respond-to(600px, max) {
        font-size: 24px;
      }

      @include respond-to(480px, max) {
        font-size: 17px;
      }

      @include respond-to(350px, max) {
        font-size: 14px;
      }
    }
  }

  .form-cards__container {
    display: flex;
    gap: #{$gap};

    .loader__container {
      flex-grow: 1;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>
