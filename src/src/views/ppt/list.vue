<template>
  <div class="app-container">
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column label="名称" min-width="200" align="center">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>
      <el-table-column label="作者" min-width="300" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.zkHosts }}</span>
        </template>
      </el-table-column>
      <el-table-column label="时间" min-width="300" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.zkHosts }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="操作" min-width="150">
        <template slot-scope="scope">
          <el-dropdown trigger="click">
            <el-button type="primary" size="mini">
              操作<i class="el-icon-arrow-down el-icon--right" />
            </el-button>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item @click.native="check(scope.row)">查看</el-dropdown-item>
              <el-dropdown-item @click.native="del(scope.row)">删除</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>

export default {
  data() {
    return {
      list: null,
      listLoading: true,
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
    },
    check(row) {
      this.$router.push('/canalServer/nodeServers?clusterId=' + row.id)
    },
    del() {

    }
  }
}
</script>
