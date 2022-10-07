<template>
  <v-app>
    <v-main>
      <TodoItem/>
    </v-main>
  </v-app>
</template>

<script>
import TodoItem from './components/TodoItem.vue';

export default {
  name: 'App',

  components: {
    TodoItem,
  },

  data: () => ({
    //
  }),
};
</script>
