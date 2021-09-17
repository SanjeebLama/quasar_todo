<template>
  <q-page class="">
    <Todo msg="Todo App" />
  </q-page>
</template>

<script>
import Todo from "../components/Todo";
export default {
  name: "PageIndex",
  components: {
    Todo
  }
};
</script>
