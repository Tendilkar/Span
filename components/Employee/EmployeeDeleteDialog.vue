<template>
  <div class="">
     <el-dialog
      title="Delete Employee"
      :visible.sync="deleteDialog"
      width="30%"
     >
      <span>Are you sure want to delete ?</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="Close">Cancel</el-button>
        <el-button type="danger" @click="DeleteEmp()">Confirm</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data(){
    return{
      deleteDialog:false,
      selectedEmpId : '',
    }
  },
  methods:{
    ShowDeleteDialog(id){
      this.deleteDialog = true
      this.selectedEmpId = id
    },
    Close(){
      this.deleteDialog = false
    },
    DeleteEmp() {
      this.deleteDialog = false
      this.$emit("emitDeleteEmp", this.selectedEmpId)
    }
  }

}
</script>

<style>

</style>