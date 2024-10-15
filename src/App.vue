<template>
  <div class="container mb-5">
    <h3 class="text-center my-4">دانلود جدول بصورت Pdf</h3>
    <div id="buttons" class="d-flex">
      <div id="showTodos">
        <button class="btn btn-success me-3" @click="ShowTodosBtnHandler">
          نمایش Todos
        </button>
      </div>
      <div id="downloadPdf">
        <button class="btn btn-primary" :disabled="ShowTemplate? false : true" @click="downloadBtnHandler">
          دانلود فایل Pdf
        </button>
      </div>
    </div>

    <div v-if="ShowTemplate" class="container mt-3">
      <table id="todosList" class="table table-striped" dir="ltr">
        <thead>
          <tr>
            <th>#</th>
            <th>Title</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(todo, index) in Todos" :key="index">
            <td>{{ todo.id }}</td>
            <td>{{ todo.title }}</td>
            <td
              :class="todo.completed == false ? 'text-danger' : 'text-success '"
            >
              {{ todo.completed == true ? "Done" : "Undone" }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import jsPDF from "jspdf";
import autoTable from "jspdf-autotable";
import { ref } from "vue";

const ShowTemplate = ref(false);
const Todos = ref([]);

function ShowTodosBtnHandler() {
  axios
    .get("https://jsonplaceholder.typicode.com/todos")
    .then(function (response) {
      Todos.value = response.data;
    })
    .catch(function (error) {
      // handle error
      console.log(error);
    })
    .finally(function () {
      // always executed
    });

  ShowTemplate.value = !ShowTemplate.value;
}

function downloadBtnHandler() {
  const doc = new jsPDF();
  autoTable(doc, { html: "#todosList" });
  doc.save("test.pdf");
}
</script>

<style>
</style>
