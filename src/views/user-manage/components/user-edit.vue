<template>
  <div class="user-add">
    <el-form
      :model="editData"
      status-icon
      ref="editData"
      label-width="100px"
      class="user-add-ruleForm"
    >
      <el-form-item
        label="姓名"
        prop="user_cname"
        :rules="[
          { required: true, message: '姓名不能为空' }
        ]"
      >
        <el-input 
          type="text" 
          v-model="editData.user_cname" 
          autocomplete="off"
        ></el-input>
      </el-form-item>
      <el-form-item
        label="账号"
        prop="user_name"
        :rules="[
          { required: true, message: '账号不能为空' }
        ]"
      >
        <el-input 
          type="text" 
          v-model="editData.user_name" 
          autocomplete="off"
        ></el-input>
      </el-form-item>
      <el-form-item
        label="密码"
        prop="user_password"
        :rules="[
          { required: true, message: '密码不能为空' }
        ]"
      >
        <el-input 
          type="password" 
          v-model="editData.user_password" 
          autocomplete="off"
        ></el-input>
      </el-form-item>
      <el-form-item
        label="部门"
        prop="user_department"
        :rules="[
          { required: true, message: '部门必填项' }
        ]"
      >
        <el-input v-model.number="editData.user_department"></el-input>
      </el-form-item>
      <el-form-item
        label="职位"
        prop="user_role"
        :rules="[
          { required: true, message: '职位必填项' }
        ]"
      >
        <el-input v-model.number="editData.user_role"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('editData', editData)">提交</el-button>
        <el-button @click="resetForm('editData')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import API from "../api";

export default {
  name: "user-edit",
  props: {
    editData: {
      type: Object,
      default: () => {}
    },
    editType: {
      type: Number,
      default: null
    }
  },
  updated() {
    if (this.editData === 2) {
      this.resetForm("editData");
    }
  },
  data() {
    return {};
  },
  methods: {
    submitForm(formName, data) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          if (2 === this.editType) {
            this.addUser(data);
          } else if (1 === this.editType) {
            this.updateUser(data);
          }
        } else {
          return false;
        }
        this.getUsers();
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    getUsers() {
      this.$emit("getUserList");
    },
    async addUser(data) {
      try {
        let result = await API.addUser(data);
        let { code } = result;
        if (200 === code) {
          this.$notify.success({
            message: "添加用户成功",
            showClose: false,
            duration: 800
          });
        }
      } catch (err) {
        console.log(err);
      }
    },
    async updateUser(data) {
      try {
        let result = await API.updateUser(data);
        let { code } = result;
        if (200 === code) {
          this.$notify.success({
            message: "修改用户成功",
            showClose: false,
            duration: 800
          });
        }
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>

<style lang="less">
.user-add {
  .user-add-ruleForm {
    input {
      width: 90%;
      height: 32px;
    }
  }
}
</style>