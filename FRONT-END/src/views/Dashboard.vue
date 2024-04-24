<template>
  <div>
    <h1>Welcome to Library Management System</h1>

    <v-tabs v-model="tab">
      <!-- <v-tab v-for="item in tabs" :key="item.id">{{ item.title }}</v-tab> -->

      <v-tab-item v-for="item in tabs" :key="item.id">
        <component :is="item.component"></component>
      </v-tab-item>
    </v-tabs>
  </div>
</template>

<script>
import Books from './Books.vue';
import Borrowings from './Borrowings.vue';
import Publishers from './Publishers.vue';
import Readers from './Readers.vue';

export default {
  name: 'Dashboard',
  components: {
    Books,
    Borrowings,
    Publishers,
    Readers,
  },
  data() {
    return {
      tab: null,
      tabs: [
        { id: 1, title: 'Manage Books', component: 'Books' },
        { id: 2, title: 'Borrowed Books', component: 'Borrowings' },
        { id: 3, title: 'Manage Publishers', component: 'Publishers' },
        { id: 4, title: 'Manage Readers', component: 'Readers' },
      ],
    };
  },
  created() {
    this.tab = this.tabs[0].id; // Set the first tab as active by default
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap');
h1 {
  font-family: 'Quicksand', sans-serif;
  font-size: 32px;
  margin-top: 20px;
  text-align: center;
}
</style>
