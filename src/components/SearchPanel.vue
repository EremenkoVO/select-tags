<template>
  <div class="col-md-12 p-2 panel">
    <div>
      <input
        type="search"
        class="form-control"
        placeholder="Введите текст для поиска"
        v-model="inputSearch"
      />
    </div>
    <div class="mt-4">
      <label><em>Выберите значения для добавления:</em></label>
      <div v-if="isListUnmarked" class="h-search-panel">
        <ul>
          <li v-for="item in listUnmarkedSearch" :key="item.id">
            <label>
              <input type="checkbox" v-model="item.selected" />
              {{ item.name }}
            </label>
          </li>
        </ul>
      </div>
      <div v-else class="text-center text-gray none-selected h-search-panel">
        <em>Нет элементов для выбора</em>
      </div>
    </div>
    <div class="footer text-right">
      <button class="btn btn-success" @click="addFromListItem">Добавить</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SearchPanel',
  props: {
    listUnmarked: {
      type: Array,
      default: () => [],
    },
  },
  data: () => ({
    inputSearch: '',
  }),
  computed: {
    isListUnmarked() {
      return this.listUnmarkedSearch?.length > 0;
    },
    listUnmarkedSearch() {
      if (this.inputSearch.length === 0) return this.listUnmarked;

      return this.listUnmarked.filter((item) => {
        if (item.name.includes(this.inputSearch)) {
          return item;
        }
      });
    },
  },
  methods: {
    addFromListItem() {
      this.listUnmarked.filter((item) => {
        if (item?.selected) {
          item.marked = true;
          return item;
        }
      });

      this.$emit('add-tags');
    },
  },
};
</script>
<style scoped>
ul {
  list-style-type: none;
  padding-left: 14px;
}

.panel {
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 14px;
}

.h-search-panel {
  min-height: 40px;
  max-height: 120px;
  overflow-y: auto;
}
</style>
