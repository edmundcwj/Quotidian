<template>
  <div id="view-employee">
    <ul class="collection with-header">
      <li class="collection-header">
        <h4>{{ name }}</h4>
      </li>
      <li class="collection-item">Employee ID#: {{ employee_id }}</li>
      <li class="collection-item">Department: {{ dept }}</li>
      <li class="collection-item">Position: {{ position }}</li>
    </ul>
    <router-link to="/" class="btn grey">Back</router-link>
    <button @click="deleteEmployee" class="btn red">Delete</button>

  </div>
</template>

<script>
import db from "./firebaseInit";
export default {
  name: "view-employee",
  data() {
    return {
      employee_id: null,
      name: null,
      dept: null,
      position: null,
    };
  },
  beforeRouteEnter(to, from, next) {
    db.collection("users")
      .where("employee_id", "==", to.params.employee_id)
      .get()
      .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          next((vm) => {
            vm.employee_id = doc.data().employee_id;
            vm.name = doc.data().name;
            vm.dept = doc.data().dept;
            vm.position = doc.data().position;
          });
        });
      });
  },
  watch: {
    $route: "fetchData",
  },
  methods: {
    deleteEmployee() {
      if (confirm("Are you sure?")) {
        db.collection("users")
          .where("employee_id", "==", this.$route.params.employee_id)
          .get()
          .then((querySnapshot) => {
            querySnapshot.forEach((doc) => {
              doc.ref.delete();
              this.$router.push("/");
            });
          });
      }
    },
  },
};
</script>
