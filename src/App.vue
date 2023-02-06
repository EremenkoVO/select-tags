<template>
  <div id="app">
    <SelectedItems :list="list" @update-marked-list="updateList" />
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
    SelectedItems: () => import('./components/SelectTags.vue'),
  },
  data: () => ({
    list: [
      {
        id: 1,
        name: 'Первый элемент',
      },
      {
        id: 2,
        name: 'Второй элемент',
      },
      {
        id: 3,
        name: 'Третий элемент',
      },
    ],
    markedList: [],
  }),
  methods: {
    updateList(newArr) {
      this.markedList = newArr;
    },
  },
};
</script>

<style></style>
