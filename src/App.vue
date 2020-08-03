<template>
  <div id="app" class="small-container">
    <h1>Employee</h1>

    <employee-form @add:employee="addEmployee" />
    <employee-table
        :employees="employees"
        @delete:employee="deleteEmployee"
        @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from "@/components/EmployeeTable";
import EmployeeForm from "@/components/EmployeeForm";

export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm,
  },
  data(){
    return {
      baseUrl: 'https://jsonplaceholder.typicode.com/users/',
      employees: []
    }
  },
  mounted() {
    this.getEmployees()
  },
  methods: {
    async getEmployees(){
      try {
        const response = await fetch(this.baseUrl)
        const data = await response.json()
        this.employees = data
      } catch (error){
        console.log(error)
      }
    },
    async addEmployee(employee){
      try{
        const response = await fetch(this.baseUrl, {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: {
            'Content-type': 'application/json; charset=UTF-8'
          },
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      } catch (error){
        console.log(error)
      }
    },
    async editEmployee(id, updatedEmployee){
      try{
        const response = await fetch(`${this.baseUrl}${id}`,{
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        const data = await response.json()
        this.employees = this.employees.map(employee => (employee.id === id ? data: employee))
      } catch (error){
        console.log(error)
      }
    },
    async deleteEmployee(id){
      try {
        await fetch(this.baseUrl + '${id}',{
          method: 'DELETE'
        })
        this.employees = this.employees.filter(employee => employee.id !== id);
      }catch (error){
        console.log(error)
      }
    }
  }
}
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }
  .small-container {
    max-width: 680px;
  }
</style>
