<template>
  <div class="">
    <!-- =================================================================== -->
    <!-- Filter Tasks -->
    <!-- =================================================================== -->
    <p class="text-h6 text-secondary">Finish What You Have Listed</p>
    <!-- <q-input
      outlined
      type="text"
      v-model="searchValue"
      label="Search Task"
      id="search-input"
    /> -->
    <!-- <ul>
      <li
        class="card"
        v-for="(task, index) in filteredTasks"
        :key="index"
        style="list-style:none"
      >
        <span>
          {{ task ? task.name : "" }} {{ task ? task.content : "" }}
          {{ task ? task.status : "" }} -->
    <!-- <h2>Index: {{ index }} ID: {{ typeof task }}</h2> -->
    <!-- </span>

        <q-btn color="teal" @click="editTask(task, index)" label="Edit" />
        <q-btn color="deep-orange" @click="deleteTask(index)" label="Delete" />
      </li>
    </ul> -->
    <!-- ============================================================================ -->
    <!-- table  -->
    <!-- ============================================================================ -->
    <div>
      <div class="q-pa-sm">
        <q-table
          title="My Tasks"
          :data="data"
          :columns="columns"
          row-key="name"
          hide-bottom
          :filter="filter"
        >
          <template v-slot:top-right>
            <q-input
              borderless
              dense
              debounce="300"
              placeholder="Search Task"
              v-model="filter"
            >
              <template v-slot:append>
                <q-icon name="search" />
              </template>
            </q-input>
          </template>
          <template v-slot:body-cell-actions="props">
            <q-td :props="props">
              <q-btn
                dense
                round
                flat
                color="grey"
                @click="editTask(props.row, props.row.id)"
                icon="edit"
              ></q-btn>
              <q-btn
                dense
                round
                flat
                color="grey"
                @click="deleteTask(props.row.id)"
                icon="delete"
              ></q-btn>
            </q-td>
          </template>
        </q-table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    taskList: {
      type: Array
    }
  },
  data() {
    return {
      searchValue: "",
      filter: "",

      columns: [
        {
          name: "desc",
          required: true,
          label: "Title ",
          align: "left",
          field: row => row.name,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: "content",
          label: "Descriptoin",
          field: "content",
          align: "left"
        },

        { name: "status", label: "Priority", field: "status", align: "left" },
        { name: "actions", label: "Actions", field: "", align: "left" }
      ],
      data: this.taskList
    };
  },
  computed: {
    filteredTasks() {
      // Process search input
      const send =
        (this.searchValue &&
          this.taskList.filter(
            item =>
              item?.name
                ?.toUpperCase()
                .includes(this.searchValue?.toUpperCase()) || false
          )) ||
        this.taskList;
      console.log("Send", send);

      return send;
    }
  },
  methods: {
    editTask(task, index) {
      this.$emit("editTask", task, index);
      // this.searchValue = "";
    },

    deleteTask(index) {
      this.$emit("deleteTask", index);
    },
    myFilter(rows, terms, cols, cellValue) {
      const lowerTerms = terms ? terms.toLowerCase() : "";
      return rows.filter(row =>
        cols.some(
          col =>
            (cellValue(col, row) + "").toLowerCase().indexOf(lowerTerms) !== -1
        )
      );
    }
  }
};
</script>

<style scoped></style>
