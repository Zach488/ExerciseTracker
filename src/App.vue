<script setup>
import { ref, onMounted, computed, watch } from "vue";

const items = [
  {
    id: "day1",
    value: "Day 1",
    model: "input_category",
    bubble: "day_1",
  },
  {
    id: "day2",
    value: "Day 2",
    model: "input_category",
    bubble: "day_2",
  },
  {
    id: "day3",
    value: "Day 3",
    model: "input_category",
    bubble: "day_3",
  },
  {
    id: "day4",
    value: "Day 4",
    model: "input_category",
    bubble: "day_4",
  },
  {
    id: "day5",
    value: "Day 5",
    model: "input_category",
    bubble: "day_5",
  },
  {
    id: "day6",
    value: "Day 6",
    model: "input_category",
    bubble: "day_6",
  },
];

const todos = ref([]);
const name = ref("");
const input_category = ref("Day 1");

const input_exercise = ref("");
const input_weight = ref("");
const input_reps = ref("");

const selectedCategoryItems = computed(() => {
  return todos.value.filter((n) => n.category === input_category.value);
});

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  {
    deep: true,
  }
);

const addTodo = () => {
  if (
    input_exercise.value.trim() === "" ||
    input_weight === "" ||
    input_reps === "" ||
    input_category.value === null
  ) {
    return;
  }

  todos.value.push({
    exercise: input_exercise.value,
    weight: input_weight.value,
    reps: input_reps.value,
    category: input_category.value,
    done: false,
    editable: false
  });

  input_exercise.value = ''
  input_weight.value = ''
  input_reps.value = ''

};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up,
        <input type="text" id="name" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A Exercise</h3>

      <form id="new-todo-form" @submit.prevent="addTodo">
        <h4>Exercise</h4>
        <input
          type="text"
          name="content"
          id="content"
          placeholder="e.g. pushups"
          v-model="input_exercise"
        />
        <div class="reps-weight">
          <h4>Weight (kg)</h4>
          <input
            type="number"
            name="content"
            id="content"
            placeholder="e.g. 10"
            v-model="input_weight"
          />
          <h4>Total Reps</h4>
          <input
            type="number"
            name="content"
            id="content"
            placeholder="e.g. 12"
            v-model="input_reps"
          />
        </div>

        <h4>Pick a day</h4>
        <div class="options">
          <label v-for="item in items">
            <input
              type="radio"
              name="category"
              :id="item.id"
              :value="item.value"
              v-model="input_category"
            />
            <span :class="`bubble ${item.bubble}`"></span>
            <div>{{ item.value }}</div>
          </label>
        </div>
        <input type="submit" value="Add exercise" />
      </form>
    </section>

    <section class="todo-list" v-if="selectedCategoryItems.length > 0">
      <h3>Exercises LIST</h3>

      <div class="list">
        <div class="todo-item">
          <div class="todo-content">Exercise</div>
          <div class="todo-content">Reps</div>
          <div class="todo-content">Weight(kg)</div>
          <div class="todo-content"></div>
        </div>
      </div>

      <div class="list" id="todo-list">
        <div v-for="todo in selectedCategoryItems" class="todo-item">
          <div class="todo-content">
            <input type="text" v-model="todo.exercise" />
          </div>
          <div class="todo-content">
            <input type="text" v-model="todo.weight" />
          </div>
          <div class="todo-content">
            <input type="text" v-model="todo.reps" id="reps" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
