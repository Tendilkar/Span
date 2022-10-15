<template>
  <div class="">
    <el-dialog
      :title="formType == 'add' ? 'Add Employee' : 'Edit Employee'"
      :visible.sync="addEditDailog"
      width="45%"
    >
      <span>
        <el-form
          label-position="top"
          ref="form"
          label-width="120px"
        >
          <el-form-item label="Name">
            <el-input v-model="employee.name"></el-input>
          </el-form-item>
          <el-form-item label="Email">
            <el-input v-model="employee.email"></el-input>
          </el-form-item>
          <el-form-item label="Address">
            <el-input
              type="textarea"
              :rows="2"
              v-model="employee.address"
            ></el-input>
          </el-form-item>
          <el-form-item label="Phone">
            <el-input v-model="employee.phone"></el-input>
          </el-form-item>
        </el-form>
      </span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addEditDailog = false">Cancel</el-button>
        <el-button type="success" @click="saveRecord">{{
          formType == 'add' ? 'Add' : 'Edit'
        }}</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      addEditDailog: false,
      formType: '',
      employee: {
        id: null,
        name: null,
        email: null,
        address: null,
        phone: null,
      },
    }
  },
  methods: {
    ShowAddEditDialog(type, val) {
      this.addEditDailog = true
      this.employee.id = null
      this.employee.name = null
      this.employee.email = null
      this.employee.address = null
      this.employee.phone = null
      this.formType = type
      if (val !== null && val !== '' && val !== undefined) {
        Object.assign(this.employee, val)
      }
    },
    saveRecord(){
      this. formType == 'add' ? this.addEmployees() : this.updateEmployee()
    },
     addEmployees() {
      const axios = require('axios');
        axios.post('http://localhost:3000/users', {
            id: this.employee.length + 1,
            name: this.employee.name,
            email: this.employee.email,
            address: this.employee.address,
            phone: this.employee.phone
        }).then(resp => {
            this.addEditDailog = false
            this.$emit("emitGetEmployees")
        }).catch(error => {
            console.log(error);
        }); 
    },
    updateEmployee() {
      const axios = require('axios');
        axios.patch(`http://localhost:3000/users/${this.employee.id}`, {
            name: this.employee.name,
            email: this.employee.email,
            address: this.employee.address,
            phone: this.employee.phone
        }).then(resp => {
            this.addEditDailog = false
            this.$emit("emitGetEmployees")
        }).catch(error => {
            console.log(error);
        }); 
    },
  },
}
</script>

<style>
.el-form-item {
    margin-bottom: 0px;
}
</style>