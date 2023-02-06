<template>
  <div class="col-md-12 p-2 panel">
    <div class="">
      <input
        type="search"
        class="form-control"
        placeholder="Введите текст для поиска"
        v-model="inputSearch"
      />
    </div>
    <div class="mt-4">
      <label><em>Выберите значения для добавления:</em></label>
      <ul v-if="isListUnselected">
        <li v-for="item in listUnselectedSearch" :key="item.id">
          <label>
            <input type="checkbox" v-model="item.selected" />
            {{ item.name }}
          </label>
        </li>
      </ul>
      <div v-else class="text-center text-gray non-selected h-4">
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
    listUnselected: {
      type: Array,
      default: () => [],
    },
  },
  data: () => ({
    inputSearch: '',
  }),
  computed: {
    isListUnselected() {
      return this.listUnselectedSearch?.length > 0;
    },
    listUnselectedSearch() {
      if (this.inputSearch.length === 0) return this.listUnselected;

      return this.listUnselected.filter((item) => {
        if (item.name.includes(this.inputSearch)) {
          return item;
        }
      });
    },
  },
  methods: {
    addFromListItem() {
      let arr = this.listUnselected.filter((item) => {
        if (item?.selected) {
          item.marked = true;
          return item;
        }
      });

      this.$emit('add-tags', arr);
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

.text-gray {
  color: #ccc;
}

.non-selected {
  user-select: none;
}

.h-4 {
  min-height: 40px;
  max-height: 120px;
  overflow-y: auto;
}
</style>
