<template>
  <div class=" text-center">
    <p class="text-h3 q-mt-lg">{{ msg }}</p>
    <div class="row">
      <div class="FORM col-md-6 q-pa-xl">
        <label class="text-h6"> {{ isEditing ? "Edit" : "Add" }} Task</label>
        <!-- =================================================================== -->
        <!-- Form For Adding and Editing Task-->
        <!-- =================================================================== -->
        <div class="column ">
          <q-input
            outlined
            type="text"
            class="q-my-lg"
            v-model.lazy="allTask.name"
            label=" Task"
          />
          <q-input
            outlined
            class="q-mb-lg"
            type="textarea"
            v-model.lazy="allTask.content"
            name=""
            id=""
            cols="30"
            rows="6"
            label="Enter Description"
          />

          <div class="q-mb-lg text-left" id="status">
            <span class="text-weight-medium"> Priority: </span>
            <input
              type="radio"
              class="q-mx-sm"
              id="one"
              value="High"
              v-model="allTask.status"
            />
            <label for="one">High</label>

            <input
              type="radio"
              id="two"
              class="q-mx-sm"
              value="Medium"
              v-model="allTask.status"
            />
            <label for="two">Medium</label>

            <input
              type="radio"
              class="q-mx-sm"
              id="three"
              value="Low"
              v-model="allTask.status"
            />
            <label for="three">Low</label>
          </div>

          <q-btn
            color="primary"
            type="submit"
            :label="(!isEditing && 'Add') || 'Edit'"
            @click="isEditing ? editData() : storeTask()"
          />
        </div>
      </div>
      <ListOperations
        class="col-md-6 q-pa-xl"
        :taskList="taskList"
        @editTask="editTask($event, $event)"
        @deleteTask="deleteTask($event)"
      />
    </div>
  </div>
</template>

<script>
import ListOperations from "./ListOperations";

const allTaskOBJ = {
  id: 0,
  name: "",
  content: "",
  status: ""
};
export default {
  name: "Todo",
  // Validation
  props: {
    msg: {
      type: String,
      required: true
    }
  },
  components: {
    ListOperations
  },
  data() {
    return {
      selectedIndex: null,
      isEditing: false,
      task: "",
      allTask: { ...allTaskOBJ },
      taskList: []
    };
  },

  methods: {
    storeTask() {
      this.taskList.push(this.allTask);
      this.id++;

      this.resetForm();
      // return this.taskList;
    },
    resetForm() {
      this.allTask = { ...allTaskOBJ };
      this.isEditing = false;
      this.selectedIndex = null;
    },
    editTask(task, index) {
      this.isEditing = true;
      this.allTask = task;
      this.selectedIndex = index;
    },

    editData() {
      console.log(this.selectedIndex, this.allTask);
      console.table(this.taskList[this.selectedIndex]);
      this.resetForm();
    },

    deleteTask(index) {
      this.taskList.splice(index, 1);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
