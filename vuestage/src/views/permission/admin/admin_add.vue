<template>
  <div class="app-container admin_add">
    <el-row type="flex" justify="center">
      <el-col :xs="24" :md="12">
        <el-form ref="ruleForm" :rules="rules" :model="ruleForm" label-position="right" label-width="150px">
          <el-form-item label="账号名" prop="username">
            <el-input v-model="ruleForm.username" placeholder="账号名"/>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input v-model="ruleForm.password" placeholder="密码"/>
          </el-form-item>
          <el-form-item label="角色" prop="role_ids">
            <el-checkbox-group v-model="ruleForm.role_ids">
              <el-checkbox v-for="(item,index) in adminRoles" :label="item.id" :key="index">
                {{ item.role_name }}
              </el-checkbox>
            </el-checkbox-group>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">确定</el-button>
            <el-button @click="resetForm('ruleForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import { adminAdd, adminRoles } from '@/api/permission/admin'
export default {
  name: 'AdminAdd',
  data() {
    return {
      ruleForm: {
        username: '',
        password: '',
        role_ids: []
      },
      adminRoles: [],
      rules: {
        username: [
          { required: true, message: '请输入账号名', trigger: 'blur' },
          { min: 2, max: 20, message: '长度在 2 到 20 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ],
        role_ids: [
          { required: true, message: '请设置角色' }
        ]
      }
    }
  },
  mounted() {
    this.getAdminRoles()
  },
  methods: {
    getAdminRoles() {
      adminRoles().then(res => {
        this.adminRoles = res.data
      }).catch(() => {
      })
    },
    submitForm(formName) {
      const _this = this
      this.$refs[formName].validate((valid) => {
        if (valid) {
          adminAdd(this.ruleForm).then(res => {
            this.$message({
              message: '添加成功',
              type: 'success',
              onClose: function() {
                _this.$router.push('/permission/admin/admin_lst')
              }
            })
          }).catch(() => {
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>
