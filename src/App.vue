<template>
  <div id="app">
    <div class="col-8 offset-2">
      <table class="table caption-top table-hover">
        <caption class="text-center">
          <h1>学生管理系统</h1>
          <el-button type="primary" @click="getStudents">获取学生信息</el-button>
          <br>
          <el-button type="danger" @click="clear">清空学生信息</el-button>
          <br>
          <el-button type="success" @click="dialogVisible = true">
            添加学生信息
          </el-button>
          <el-dialog v-model="dialogVisible" title="添加学生信息" width="30%"><!--before-close="handleClose"-->
            <span>学号</span>
            <input v-model="newStudents.number">
            <br>
            <span>姓名</span>
            <input v-model="newStudents.name">
            <br>
            <span>年龄</span>
            <input v-model="newStudents.age">
            <br>
            <span>语文</span>
            <input v-model="newStudents.chi">
            <br>
            <span>数学</span>
            <input v-model="newStudents.math">
            <br>
            <span>英语</span>
            <input v-model="newStudents.eng">
            <br>
            <span class="dialog-footer">
              <el-button type="primary" @click="add">
                提交
              </el-button>
              <el-button @click="dialogVisible = false">
                取消
              </el-button>
            </span>
          </el-dialog>
        </caption>
        <thead>
        <tr>
          <th scope="col">学号</th>
          <th scope="col">姓名</th>
          <th scope="col">年龄</th>
          <th scope="col">语文</th>
          <th scope="col">数学</th>
          <th scope="col">英语</th>
          <th scope="col">操作</th>
        </tr>
        </thead>
        <tbody>
        <Student v-for="stu in students_for_page" :key="stu.id" :student="stu"></Student>
        </tbody>
      </table>
      <div class="align-content-center">
        <el-button type="primary" @click="pageUp">
          上一页
        </el-button>
        <el-button type="success" @click="pageDown">
          下一页
        </el-button>
      </div>
    </div>
  </div>
</template>

<script>
import Student from './components/Student'
import axios from "axios";
import {ElMessageBox} from 'element-plus'

export default {
  name: 'App',
  components: {
    Student
  }, data() {
    return {
      students: [],
      axios: axios.get("http://localhost:8080/students"),
      dialogVisible: false,
      currentPage: 1,
      newStudents: {
        "number": null,
        "name": null,
        "age": null,
        "chi": null,
        "math": null,
        "eng": null
      }
    }
  },
  methods: {
    getStudents() {
      this.axios.then(res => {
        console.log(res.data)
        this.students = res.data
      })
      console.log("获取数据成功")
    }, clear() {
      this.students = ''
    },
    add() {
      axios({
        url: "http://121.43.231.76:8080/add",
        method: "POST",
        data: this.newStudents
      })
      this.dialogVisible = false
    },
    pageUp() {
      --this.currentPage;
    },
    pageDown() {
      ++this.currentPage;
    }
  },
  computed: {
    students_for_page() {
      return this.students.slice(this.currentPage * 5 - 5, this.currentPage * 5);
    }
  }
}

</script>

<style lang="scss">
.dialog-footer button:first-child {
  margin-right: 10px;
}
</style>
