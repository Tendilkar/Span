<template>
  <div class="container">
    <div class="row">
      <h3>Manage Employees</h3>
      <el-button class="btn-primary" type="primary" size="mini" round @click="openModal('add')"
        >Add new Employee</el-button>
      <div class="ml-3">
        <el-table :data="empDetails" style="border: 1px solid black; width: 100%">
          <el-table-column min-width="100px" prop="name" label="Name">
          </el-table-column>
          <el-table-column min-width="120px" prop="address" label="Address"> </el-table-column>
          <el-table-column prop="phone" label="Phone"> </el-table-column>
          <el-table-column min-width="60px" label="Actions">
            <template v-slot="{row}">
            <span class="" style="margin-right: 19px;">
              <i
                class="el-icon-edit"
                style="cursor: pointer; color: orange;"
                @click="openModal('edit',row)"
              ></i>
            </span>
            <span class="">
              <i class="el-icon-delete" style="cursor: pointer; color: red;" @click="alertDelete(row.id)"></i>
            </span>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>

    <!--AddEdit modal -->
    <AddEditDialog ref="EmpAddEditDialog" @emitGetEmployees="getEmployees"/>

    <!-- Delete modal -->
    <DeleteDialog ref="EmpDeleteDialog" @emitDeleteEmp="deleteEmployee"/>
  </div>
</template>

<script>
import DeleteDialog from '@/components/Employee/EmployeeDeleteDialog.vue'
import AddEditDialog from '@/components/Employee/EmployeeAddEditDialog.vue'
export default {
  components:{
    DeleteDialog,
    AddEditDialog
  },
  data() {
    return {
      formType:'',
      empDetails: [],
    }
  },
  mounted() {
    this.getEmployees()
  },
  methods: {
    openModal(type, row) {
      debugger
      this.formType = type
      this.$refs.EmpAddEditDialog.ShowAddEditDialog(this.formType, row)
    },
    getEmployees() {
      const axios = require('axios')
      axios
        .get('http://localhost:3000/users')
        .then((resp) => {
          this.empDetails = resp.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    alertDelete(id) {
      this.$refs.EmpDeleteDialog.ShowDeleteDialog(id)
    },
    deleteEmployee(id) {
      const axios = require('axios');
        axios.delete(`http://localhost:3000/users/${id}`)
        .then(resp => {
            console.log(resp.data);
            this.getEmployees()
        }).catch(error => {
            console.log(error);
        }); 
    },
  },
}
</script>

<style>
.container{
  margin: 2rem;
}
.btn-primary{
  float: right;
  margin-bottom: 8px;
}
</style>
