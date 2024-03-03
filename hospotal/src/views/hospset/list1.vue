<template>
  <div class="app-container">
    <el-table
      :data="list" stripe style="width: 100%" @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55"/>
      <el-table-column type="index" width="50" label="序号"/>
      <el-table-column prop="hosname" label="医院名称"/>
      <el-table-column prop="hoscode" label="医院编号"/>
      <el-table-column prop="apiUrl" label="api基础路径" width="200"/>
      <el-table-column prop="contactsName" label="联系人姓名"/>
      <el-table-column prop="contactsPhone" label="联系人手机"/>
      <el-table-column label="状态" width="80">
        <template slot-scope="scope">
          {{ scope.row.status === 1 ? '可用' : '不可用' }}
        </template>
      </el-table-column>

      <el-table-column label="操作" width="280" align="center">
        <template slot-scope="scope">
          <el-button type="danger" size="mini"
                     icon="el-icon-delete" @click="removeDataById(scope.row.id)">删除</el-button>
          <el-button v-if="scope.row.status==1" type="primary" size="mini"
                     icon="el-icon-delete" @click="lockHostSet(scope.row.id,0)">锁定</el-button>
          <el-button v-if="scope.row.status==0" type="danger" size="mini"
                     icon="el-icon-delete" @click="lockHostSet(scope.row.id,1)">取消锁定</el-button>
          <router-link :to="'/hospSet/edit/'+scope.row.id">
            <el-button type="primary" size="mini" icon="el-icon-edit"></el-button>
          </router-link>

        </template>
      </el-table-column>

    </el-table>

    <!-- 分页 -->
    <el-pagination
      :current-page="current"
      :page-size="limit"
      :total="total"
      style="padding: 30px 0; text-align: center;"
      layout="total, prev, pager, next, jumper"
      @current-change="getList"/>

  </div>
</template>

<script>
import hospset from "@/api/hospset";
export default {
  data() {
    return {
      current:1,
      limit:3,
      searchObj:{},
      list:[],  //每页的数据集合
      total
    }
  },
  //在页面渲染之前执行，一般调用methods定义的方法，得到数据
  created() {
    this.getList()
  },
  methods:{
    getList(page=1){
      this.current = page
      hospset.getHospSetList(this.current, this.limit, this.searchObj)
        .then(response=>{
          this.list = response.data.records
          this.total = response.data.total
      })//请求成功
        .catch()//请求失败
    }
  }

}
</script>

<style scoped>

</style>
