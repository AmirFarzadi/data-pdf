<template>
  <div class="container mt-3">
    <button @click="createPdf">test</button>
    <table id="todosList" class="list-group">
      <thead>
        <tr>
          <th>
            <span class="fw-bold">Status</span>
          </th>
          <th>
            <span class="fw-bold">titile</span>
          </th>
          <th>
            <span id="todosNumber" class="fw-bold ms-5">#</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in Todos"
        :key="index">
            <td :class="todo.completed == false ? 'text-danger' : 'text-success '">{{ todo.completed == true ? "Done" : "Undone" }}</td>
            <td>{{ todo.title }}</td>
            <td>{{ todo.id }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import jsPDF from "jspdf";
import autoTable from "jspdf-autotable";
import { inject } from "vue";
const Todos = inject("Todos");

function createPdf() {
  const doc = new jsPDF();
  autoTable(doc, { html: "#todosList" });
  doc.save("test.pdf");
}


</script>

<style>
</style>