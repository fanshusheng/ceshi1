<!--
 * @Author: 方书生 fanss1368@163.com
 * @Date: 2023-02-20 11:19:50
 * @LastEditors: 方书生 fanss1368@163.com
 * @LastEditTime: 2023-02-20 11:58:08
 * @FilePath: \hrsaas\src\views\employees\detail\index.vue
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-tabs>
          <el-tab-pane label="登录账户设置">
            <!-- 放置表单 -->
            <el-form ref="userForm" :model="FormData" :rules="rules" label-width="120px" style="margin-left: 120px; margin-top:30px">
              <el-form-item label="姓名:" prop="username">
                <el-input v-model="FormData.username" style="width:300px" />
              </el-form-item>
              <el-form-item label="新密码:" prop="password2">
                <el-input v-model="FormData.password2" style="width:300px" type="password" />
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="saveUser">更新</el-button>
              </el-form-item>
            </el-form>
          </el-tab-pane>
          <el-tab-pane label="个人详情">
            <component :is="UserInfo" />
          </el-tab-pane>
          <el-tab-pane label="岗位信息">
            <component :is="JobInfo" />
          </el-tab-pane>
        </el-tabs>
      </el-card>
    </div>
  </div>
</template>

<script>
import { getUserDetailByIdApi } from '@/api/employees'
import { saveUserDetailByIdApi } from '@/api/employees'

// 引入user-info.vue
import UserInfo from '../component/user-info.vue'

// 引入JobInfo
import JobInfo from '../component/job-info.vue'
export default {
  components: { UserInfo: UserInfo, JobInfo: JobInfo },
  data() {
    return {
      UserInfo: 'UserInfo',
      JobInfo: 'JobInfo',
      userId: this.$route.params.id, // 这样可以后面直接通过 this.userId进行获取数据
      FormData: {
        // 专门存放基本信息
        username: '',
        password2: ''
      },
      rules: {
        username: [{ required: true, message: '姓名不能为空', trigger: 'blur' }],
        password2: [{ required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 6, max: 9, message: '密码长度6-9位', trigger: 'blur' }]
      }
    }
  },
  created() {
    this.getUserDetailById()
  },
  methods: {
    async getUserDetailById() {
      this.FormData = await getUserDetailByIdApi(this.userId)
    },
    async saveUser() {
      try {
        // 校验
        await this.$refs.userForm.validate()
        await saveUserDetailByIdApi({ ...this.FormData, password: this.FormData.password2 }) // 将新密码的值替换原密码的值
        this.$message.success('保存成功')
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<style>

</style>
