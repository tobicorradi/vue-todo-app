<template>
  <div id="app">
    <div class="container">
      <h1>Vue Todo App <span>by Tobias Corradi</span></h1>
      <div class="search-box">
        <form @submit="createTask" action="">
          <input
            class="search-box__input"
            type="text"
            v-model="taskText"
            placeholder="Write your task here..."
            autofocus
          />
        </form>
      </div>
      <div class="list">
        <div class="empty-list" v-if="!todoList.length">
          <img src="./assets/calendar.png" alt="" />
          <p>There are no tasks</p>
        </div>
        <div v-else>
          <div class="list__total-tasks">
            <p>
              Total Tasks: <strong>{{ todoList.length }}</strong>
            </p>
          </div>
          <div
            class="list__singleTask"
            v-for="(task, index) in todoList"
            v-bind:key="task.index"
            :class="{ completed: task.isCompleted }"
          >
            <div class="list__singleTask__text">
              <input
                class="list__checkbox"
                type="checkbox"
                :checked="task.isCompleted"
                @click="task.isCompleted = !task.isCompleted"
              />
              <p
                v-if="!task.editing"
                @click="task.isCompleted = !task.isCompleted"
              >
                {{ task.text }}
              </p>
              <input
                v-else
                class="list__edit-input"
                type="text"
                autofocus
                v-model="task.text"
                @blur="doneEditing(task)"
                @keyup.enter="doneEditing(task)"
              />
            </div>
            <div v-if="!task.editing" class="list__buttons">
              <button @click="editTask(task)" class="btn edit-button">
                Edit
              </button>
              <button @click="deleteTask(index)" class="btn delete-button">
                Delete
              </button>
            </div>
            <div v-else class="list__accept-button">
              <button @click="doneEditing(task)" class="btn">Confirm</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      taskText: "",
      totalTasks: 0,
      todoList: [
        {
          text: "¡I am a task!",
          isCompleted: false,
          editing: false,
        },
        {
          text: "I am a completed task",
          isCompleted: true,
          editing: false,
        },
      ],
    };
  },
  methods: {
    createTask(e) {
      e.preventDefault();
      if (this.taskText != "" && this.taskText != null) {
        this.todoList.unshift({ text: this.taskText, isCompleted: false });
        this.taskText = "";
      }
    },
    deleteTask(index) {
      this.todoList.splice(index, 1);
    },
    editTask(task) {
      task.editing = !task.editing;
      task.isCompleted = false;
    },
    doneEditing(task) {
      task.editing = false;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
*:focus {
  outline: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
input:not(:type[text]) {
  cursor: pointer;
}
h1 {
  margin-bottom: 20px;
  font-size: 18px;
}
h1 span {
  font-size: 10px;
  color: #b9b9b9;
  font-weight: 200;
}
.container {
  max-width: 400px;
  margin: 0 auto;
}
.search-box {
  margin-bottom: 30px;
}
.search-box__input {
  width: 100%;
  padding: 19px 30px;
  border: 0;
  box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
  font-size: 15px;
  border: 1px solid #f6f6f6;
}
.search-box__input:focus {
  outline: 0;
}
.list__singleTask {
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  padding: 18px 30px;
  text-align: left;
  margin-bottom: 10px;
  position: relative;
}
.list__singleTask p,
.list__edit-input {
  max-width: 185px;
  line-height: 21px;
  font-weight: bold;
  word-wrap: break-word;
}
.list__singleTask__text {
  display: flex;
  align-items: center;
}
.list__checkbox {
  margin-right: 13px;
  width: 17px;
  height: 17px;
}
.completed p {
  text-decoration: line-through;
}
.completed::before {
  content: "";
  display: block;
  width: 3px;
  height: 100%;
  background: #05c41c;
  position: absolute;
  left: 0;
}
.btn {
  border: 0;
  padding: 10px;
  border-radius: 4px;
  color: black;
  cursor: pointer;
}
.delete-button {
  background-color: #eeeeee;
}
.edit-button {
  background-color: white;
  margin-right: 15px;
}
.list__edit-input {
  padding: 8px;
  font-size: 16px;
  border: 1px solid #e2e2e2;
}
.empty-list {
  margin-top: 53px;
}
.empty-list p {
  margin-top: 23px;
  color: #717171;
}
.list__total-tasks {
  text-align: left;
  margin-bottom: 15px;
}
</style>
