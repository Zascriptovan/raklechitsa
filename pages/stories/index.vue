<template>
  <container class="container">
    <stories :stories="renderStories" :totalItems="totalItems" class="stories">
      <form class="stories__form" v-on:submit.prevent="findStories">
        <stories-button class="stories__clean-button" @btnClick="cleanSearch">
          Очистить
        </stories-button>
        <stories-button
          class="stories__clean-button_mobile"
          @btnClick="cleanSearch"
        >
          <img src="button/cleansearchmobile.svg" alt="" />
        </stories-button>
        <stories-input
          class="stories__input "
          v-model="searchText"
          :borderTheme="'stories'"
        >
        </stories-input>
        <stories-button
          type="submit"
          class="stories__button"
          :disabled="!searchText.length"
          >Поиск</stories-button
        >
        <stories-button
          type="submit"
          class="stories__button-mobile"
          :disabled="!searchText.length"
        ></stories-button>
      </form>
    </stories>
    <pagination
      :totalItems="totalItems"
      :itemsPerPage="itemsPerPage"
      @onPageChanged="changeStartIndex"
    />
  </container>
</template>

<script>
import SectionTitle from '@/components/ui/SectionTitle';
import Stories from '@/components/blocks/Stories';
import Input from '@/components/ui/Input';
import Button from '@/components/ui/Button';
import Pagination from '@/components/ui/Pagination';
import Container from '@/components/ui/Container';

export default {
  data() {
    return {
      searchText: '',
      itemsPerPage: 16,
      startIndex: 0,
      texts: '',
      totalItems: this.$store.state.stories.stories.length,
      title: 'РАКЛЕЧИТСЯ.РФ — истории неизлечимых превычек ',
    };
  },
  head() {
    return {
      title: this.title,
    };
  },

  components: {
    stories: Stories,
    'stories-input': Input,
    'stories-button': Button,
    pagination: Pagination,
    container: Container,
    'stories-title': SectionTitle,
  },
  computed: {
    renderStories() {
      const { stories } = this.$store.state;

      if (this.texts !== '') {
        this.texts = this.texts.toLowerCase();
        const newTotalItems = stories.stories.filter(
          item =>
            item.author.toLowerCase().includes(this.texts) ||
            item.title.toLowerCase().includes(this.texts)
        );

        return newTotalItems.filter(
          (item, idx) =>
            idx >= this.startIndex &&
            idx <= this.startIndex + this.itemsPerPage - 1
        );
      } else {
        return stories.stories.filter(
          (item, idx) =>
            idx >= this.startIndex &&
            idx <= this.startIndex + this.itemsPerPage - 1
        );
      }
    },
  },
  methods: {
    changeStartIndex(index) {
      this.startIndex = (index - 1) * this.itemsPerPage;
    },
    findStories(event) {
      this.texts = this.searchText.toLowerCase();
      const { stories } = this.$store.state;
      const newTotalItems = stories.stories.filter(
        item =>
          item.author.toLowerCase().includes(this.texts) ||
          item.title.toLowerCase().includes(this.texts)
      );
      this.totalItems = newTotalItems.length;
      this.startIndex = 0;
    },
    cleanSearch(event) {
      this.searchText = '';
      this.texts = '';
      const { stories } = this.$store.state;
      const newTotalItems = stories.stories;
      this.totalItems = newTotalItems.length;
    },
  },
  mounted() {
    if (process.browser) {
      if (window.innerWidth <= 768 && window.innerWidth > 475) {
        this.itemsPerPage = 12;
      } else if (window.innerWidth <= 475) {
        this.itemsPerPage = 9;
      } else {
        this.itemsPerPage = 16;
      }
    }
  },
};
</script>

<style scoped>
.stories {
  padding-top: 100px;
}
.stories__form {
  display: flex;
  position: relative;
  justify-content: space-between;
  margin-bottom: 70px;
}
.stories__input {
  height: 52px;
  flex: 1;
  margin-right: 20px;
}
.stories__button {
  width: 226px;
  height: 52px;
  padding: 0;
  background-color: #613a93;
}

.stories__button-mobile {
  background-color: #613a93;
  height: 46px;
  width: 46px;
  background-image: url(/button/magnifier.svg);
  padding: 0;
  background-position: center;
  background-repeat: no-repeat;
  display: none;
}
.stories__clean-button {
  font-style: normal;
  font-weight: normal;
  text-decoration: none;
  border: none;
  background-color: transparent;
  font-size: 16px;
  line-height: 19px;
  cursor: pointer;
  padding: 0;
  right: 266px;
  position: absolute;
  color: #666666;
  top: 17px;
  outline: 0;
}
.stories__clean-button_mobile {
  display: none;
  font-style: normal;
  font-weight: normal;
  text-decoration: none;
  border: none;
  background-color: transparent;
  font-size: 16px;
  line-height: 19px;
  cursor: pointer;
  padding: 0;
  right: 266px;
  position: absolute;
  color: #666666;
  top: 17px;
  outline: 0;
}

@media screen and (max-width: 1280px) {
  .stories {
    padding-top: 90px;
  }
  .stories__form {
    margin-bottom: 60px;
  }
  .stories__input {
    height: 48px;
  }
  .stories__button {
    height: 48px;
  }
  .stories__clean-button {
    top: 15px;
  }
}

@media screen and (max-width: 1024px) {
  .stories {
    padding-top: 80px;
  }
  .stories__form {
    margin-bottom: 46px;
  }
  .stories__input {
    height: 46px;
  }
  .stories__button {
    height: 46px;
    width: 208px;
  }
  .stories__clean-button {
    font-size: 15px;
    line-height: 18px;
    right: 248px;
    top: 14px;
  }
}

@media screen and (max-width: 768px) {
  .stories__form {
    margin-bottom: 60px;
  }
  .stories__clean-button {
    display: none;
  }
  .stories__clean-button_mobile {
    display: block;
    top: 9px;
    right: 243.22px;
  }
}

@media screen and (max-width: 475px) {
  .stories__button {
    display: none;
  }
  .stories__input {
    margin-right: 6px;
    padding: 0;
  }
  .stories__button-mobile {
    display: block;
  }
  .stories__clean-button_mobile {
    top: 9px;
    right: 68px;
  }
}

@media screen and (max-width: 320px) {
  .stories {
    padding-top: 50px;
  }
  .stories__form {
    margin-bottom: 30px;
  }
}
</style>
