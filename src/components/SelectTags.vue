<template>
  <div class="container">
    <div class="tags">
      <span v-for="item in selectedItem" :key="item.id" class="tag">
        {{ item.name }}
        <i
          class="glyphicon glyphicon-ban-circle"
          @click="(item.marked = false), (item.selected = false)"
        ></i>
      </span>
    </div>
    <button
      type="button"
      class="btn btn-default"
      title="Добавить поле"
      @click="setActivePanel"
    >
      <i v-if="isActivePanel" class="glyphicon glyphicon-minus"></i>
      <i v-else class="glyphicon glyphicon-plus"></i>
    </button>
    <div v-if="isActivePanel" class="row">
      <div class="col-md-5" style="z-index: 1000; position: absolute">
        <SearchPanel
          :list-unselected="unSelectedItem"
          add-tags="changeMarkedItem"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SearchPanel from './SearchPanel.vue';
export default {
  name: 'SelectTags',
  components: {
    SearchPanel,
  },
  props: {
    list: {
      type: Array,
      required: true,
    },
  },
  data: () => ({
    isActivePanel: false,
    localList: [],
  }),
  computed: {
    unSelectedItem() {
      return this.localList.filter((item) => {
        if (!item.marked) return item;
      });
    },
    selectedItem() {
      return this.localList.filter((item) => {
        if (item.marked) return item;
      });
    },
  },
  methods: {
    setActivePanel() {
      this.isActivePanel = !this.isActivePanel;
    },
    changeMarkedItem(newArr) {
      this.localList = [...newArr];
    },
  },
  created() {
    this.localList = this.list.map((item) => {
      return { ...item, selected: false, marked: false };
    });
  },
};
</script>

<style scoped>
.tags {
  margin-bottom: 15px;
}

.tag {
  border: 1px solid #ccc;
  background-color: #ccc;
  color: #000;
  border-radius: 14px;
  padding: 4px 14px;
}

.tag i {
  cursor: pointer;
}

.tag i:hover {
  color: red;
}
</style>
