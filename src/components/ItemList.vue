<template>
  <div class="item-list">
    <div class="header-menu">
      <input
        v-model="currentName"
        class="header-menu__input input"
        type="text"
        :placeholder="placeholder"
        @keydown.esc="clear"
        @keyup.enter="addItem"
        @focus="focused = true" />

      <button
        v-show="focused"
        class="header-menu__button-add button-add"
        :disabled="isMatchElement"
        :class="{ disabled: disabledAddButton }"
        @click="addItem">
        <svg
          width="24"
          height="24"
          stroke-width="1.5"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg">
          <path
            d="M8 12H12M16 12H12M12 12V8M12 12V16"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round" />
          <path
            d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round" />
        </svg>
      </button>

      <button
        v-show="focused"
        class="header-menu__button-clear button-clear"
        @click="clear">
        <svg
          width="24"
          height="24"
          stroke-width="1.5"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg">
          <path
            d="M6.75827 17.2426L12.0009 12M17.2435 6.75736L12.0009 12M12.0009 12L6.75827 6.75736M12.0009 12L17.2435 17.2426"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round" />
        </svg>
      </button>
    </div>

    <section class="item-list__main">
      <div class="item-list__item">
        <div
          v-for="itemElement of searchItems"
          :key="itemElement"
          class="item-list__item-wrapper">
          <Item
            :item-element="itemElement"
            :match="
              itemElement.name.toLowerCase() === currentName.toLowerCase()
                ? true
                : false
            "
            @delete-user="deleteUser" />
        </div>
      </div>

      <div class="item-list__buttons">
        <button
          class="item-list__button"
          :class="{ active: sortName }"
          @click="sortByName">
          Sort by Value
        </button>
        <button
          class="item-list__button"
          :class="{ active: sortDate }"
          @click="sortByDate">
          Sort by Date
        </button>
      </div>
    </section>
  </div>
</template>

<script>
import Item from './Item.vue';

export default {
  name: 'ItemList',
  components: {
    Item,
  },
  data: () => ({
    currentName: '',
    currentTime: new Date().toISOString(),
    receivedUsers: [],
    focused: false,
    deletedItems: 0,
    sortDate: false,
    sortName: false,
    placeholder: 'Search or Add...',
  }),
  computed: {
    disabledAddButton() {
      return this.currentName === '' || this.isMatch() ? true : false;
    },
    searchItems() {
      return this.receivedUsers.filter((el) => {
        return el.name.toLowerCase().includes(this.currentName.toLowerCase());
      });
    },
    numberList() {
      return this.receivedUsers.length;
    },
  },
  mounted() {
    if (localStorage.getItem('users')) {
      this.receivedUsers = JSON.parse(localStorage.getItem('users'));
    }
  },
  methods: {
    clear() {
      return (this.currentName = '');
    },
    sortByName() {
      this.sortName = true;
      this.sortDate = false;
      return this.receivedUsers.sort((a, b) => a.name.localeCompare(b.name));
    },
    sortByDate() {
      this.sortName = false;
      this.sortDate = true;
      return this.receivedUsers.sort(
        (a, b) => Date.parse(b.time) - Date.parse(a.time)
      );
    },
    addItem() {
      let newItem = {
        name: this.currentName,
        number: this.numberList + this.deletedItems + 1,
        time: new Date().toISOString(),
      };
      this.receivedUsers.push(newItem);
      const parsed = JSON.stringify(this.receivedUsers);
      localStorage.setItem('users', parsed);
    },
    deleteUser(number) {
      let position = this.receivedUsers.findIndex(
        (element) => element.number === number
      );
      this.receivedUsers.splice(position, 1);
      this.deletedItems = this.deletedItems + 1;
    },
    isMatch() {
      let matchElement = false;
      this.receivedUsers.forEach((el) => {
        if (el.name.toLowerCase() === this.currentName.toLowerCase()) {
          return (matchElement = true);
        }
      });
      return matchElement;
    },
  },
};
</script>

<style lang="scss">
@import '../sass/ItemList.scss';
@import '../sass/HeaderMenu.scss';
</style>
