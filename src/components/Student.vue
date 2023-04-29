<template>
  <tr>
    <th scope="row">{{ student.number }}</th>
    <td v-show="!is_edit">{{ student.name }}</td>
    <td v-show="!is_edit">{{ student.age }}</td>
    <td v-show="!is_edit">{{ student.chi }}</td>
    <td v-show="!is_edit">{{ student.math }}</td>
    <td v-show="!is_edit">{{ student.eng }}</td>
    <td v-show="!is_edit">
      <el-button  @click="edit" type="primary">修改</el-button>
      <el-button type="danger" @click="remove">删除</el-button>
    </td>
    <!--    单击修改出现-->
    <th v-show="is_edit" scope="row">
      <el-input type="text" v-model="student.number">
      </el-input>
    </th>
    <td v-show="is_edit">
      <el-input type="text" v-model="student.name">
      </el-input>
    </td>

    <td v-show="is_edit">
      <el-input type="text" v-model.number="student.age">
      </el-input>
    </td>
    <td v-show="is_edit">
      <el-input type="text" v-model.number="student.chi">
      </el-input>
    </td>
    <td v-show="is_edit">
      <el-input type="text" v-model.number="student.math">
      </el-input>
    </td>
    <td v-show="is_edit">
      <el-input type="text" v-model.number="student.eng">
      </el-input>
    </td>
    <td v-show="is_edit">
    <el-button  type="primary" @click="update">更新</el-button>
    <el-button type="danger" @click="remove">删除</el-button>
    </td>
  </tr>

</template>

<script>

import axios from "axios";

export default {
  data() {
    return {
      is_edit: false,
    }
  },
  methods: {
    edit() {
      this.is_edit = !this.is_edit;
      console.log(this.is_edit);
    },
    update(){
      this.$parent.getStudents();
      axios({
        url:"http://localhost:8080/update",
        method:"POST",
        data:this.student,
      });
      this.is_edit=!this.is_edit;
      this.$parent.getStudents();
      console.log(this.student)
    },
    remove(){
      axios({
        url:"http://localhost:8080/delete",
        method:"POST",
        data: this.student
      })
      this.$parent.getStudents()
    }
  },
  props: ["student"]
}
</script>

<style scoped>

</style>
