<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee" />
    <employee-table 
    :employees="employees" 
    @delete:employee="deleteEmployee" 
    @edit:employee='editEmployee' />
   


  </div>
</template>

<script>
  import EmployeeTable from './components/EmployeeTable.vue'
  import EmployeeForm from './components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm,
    },
    data(){
      return{
        employees: [ ]
      }
    }, //end of data

      mounted() {
          this.getEmployees();
      },

    methods: {

    //GET JSON DATA FOR EMPLOYEES
      async getEmployees() {
           try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employees = data
             } catch (error) {
                console.error('error');
                }
          },
    // CREATE NEW EMPLOYEE
     async addEmployee(employee) {
          try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
              })
        const data = await response.json()
        this.employees = [...this.employees, data]
            } catch (error) {
                console.error(error)
          }
    },

  //UPDATE EMPLOYEE
    async editEmployee(id, updatedEmployee) {
          try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
              })
          const data = await response.json()
          this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
              } catch (error) {
                    console.error(error)
                    }
    },

    // DELETE EMPLOYEE
    async deleteEmployee(id) {
           try {
            await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: "DELETE"
            });
            this.employees = this.employees.filter(employee => employee.id !== id);
             } catch (error) {
                console.error(error);
        }
}
      } //end of methods
  } //end of export
   

</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
  form {
    margin-bottom: 2rem;
  }
</style>
