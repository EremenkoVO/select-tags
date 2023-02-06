<template>
  <div class="container">
    <ul v-if="isMarkedItems" class="tags">
      <li v-for="item in markedItems" :key="item.id" class="tag">
        <span>
          {{ item.name }}
          <i
            class="glyphicon glyphicon-ban-circle"
            @click="
              (item.selected = false), (item.marked = false), changeMarkedItem()
            "
          ></i>
        </span>
      </li>
    </ul>
    <button
      type="button"
      class="btn btn-default btn-sm"
      title="Добавить поле"
      @click="setActivePanel"
    >
      <i v-if="isActivePanel" class="glyphicon glyphicon-minus"></i>
      <i v-else class="glyphicon glyphicon-plus"></i>
    </button>
    <div v-if="isActivePanel" class="row">
      <div class="col-md-5" style="z-index: 1000; position: absolute">
        <SearchPanel
          :list-unmarked="unMarkedItem"
          @add-tags="changeMarkedItem"
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
    unMarkedItem() {
      return this.localList.filter((item) => {
        if (!item.marked) return item;
      });
    },
    markedItems() {
      return this.localList.filter((item) => {
        if (item.marked) return item;
      });
    },
    isMarkedItems() {
      return this.markedItems.length > 0;
    },
  },
  methods: {
    setActivePanel() {
      this.isActivePanel = !this.isActivePanel;
    },
    changeMarkedItem() {
      let newArr = [];

      this.markedItems.forEach((item) => {
        if (item.marked) {
          newArr.push(item);
        }
      });

      this.$emit('update-marked-list', newArr);
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
  list-style: none;
  margin: 0;
  overflow: hidden;
  padding: 0;
}

.tag {
  border: 1px solid #ccc;
  background-color: #ccc;
  color: #000;
  border-radius: 14px;
  padding: 4px 14px;
  margin: 4px;
}

.tags li {
  float: left;
}

.tag i {
  cursor: pointer;
}

.tag i:hover {
  color: crimson;
}
</style>
