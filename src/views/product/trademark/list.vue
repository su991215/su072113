<template>
  <div>
    <el-button type="primary" @click="dialogVisible = true" icon="el-icon-plus"
      >添加</el-button
    >

    <el-table :data="trademarkList" border style="width: 100%; margin: 20px 0">
      <el-table-column prop="index" label="序号" width="80" align="center">
      </el-table-column>
      <el-table-column prop="tmName" label="品牌名称"> </el-table-column>
      <el-table-column label="品牌LOGO">
        <template slot-scope="scope">
          <img class="trademark-img" :src="scope.row.logoUrl" alt="logo" />
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template>
          <el-button type="warning" icon="el-icon-edit">修改</el-button>
          <el-button type="danger" icon="el-icon-delete">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- <el-pagination
      class="trademark-pagination"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :page-sizes="[3, 6, 9]"
      :page-size.sync="limit"
      :current-page="page"
      layout="prev, pager, next, jumper, sizes, total"
      :total="total"
    >
    </el-pagination> -->
    <el-pagination
      class="trademark-pagination"
      @size-change="getPageList(page, $event)"
      @current-change="getPageList($event, limit)"
      :page-sizes="[3, 6, 9]"
      :page-size.sync="limit"
      :current-page="page"
      layout="prev, pager, next, jumper, sizes, total"
      :total="total"
    >
    </el-pagination>

    <el-dialog
      title="添加品牌"
      :visible.sync="dialogVisible"
      width="50%"
      :before-close="handleClose"
    >
      <el-form :model="trademarkForm" ref="trademarkForm" label-width="100px">
        <el-from-item label="品牌名称" prop="tmName">
          <el-input v-model="trademarkForm.tmName"></el-input>
        </el-from-item>
        <el-from-item label="品牌LOGO" prop="logoUrl">
          <el-upload
            class="avatar-uploader"
            action="https://jsonplaceholder.typicode.com/posts/"
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload"
          >
            <img v-if="imageUrl" :src="imageUrl" class="avatar" />
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
          </el-upload>
        </el-from-item>
      </el-form>
      <img
        v-if="trademarkForm.logoUrl"
        :src="trademarkForm.logoUrl"
        class="avatar"
      />
      <i v-else class="el-icon-plus avatar-uploader-icon"></i>

      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false"
          >确 定</el-button
        >
      </span>
    </el-dialog>
  </div>
</template>

<script>
// import { trademark } from "@/api";

export default {
  name: "TrademarkList",
  data() {
    return {
      trademarkList: [],
      total: 0,
      page: 1,
      limit: 3,
      dialogVisible: false,
      trademarkForm: {
        tmName: "",
        logoUrl: "",
      },
      imageUrl: "",
    };
  },
  methods: {
    // handleSizeChange(limit) {
    //   this.getPageList(this.page, limit);
    // },
    // handleSizeChange(page) {
    //   this.getPageList(page, this.limit);
    // },
    async getPageList(page, limit) {
      try {
        const result = await this.$API.trademark.getPageList(page, limit);
        if (result.code === 200) {
          this.$message.success("获取品牌分页列表成功");

          this.limit = result.data.size;
          this.page =
            result.data.current > result.data.pages
              ? result.data.pages
              : result.data.current;
          this.trademarkList = result.data.records;
          this.total = result.data.total;
        } else {
          this.$message.error("获取失败");
        }
      } catch (e) {
        this.$message.error("获取失败");
      }
    },
    handleClose(done) {
      this.$confirm("确认关闭？")
        .then((_) => {
          done();
        })
        .catch((_) => {});
    },
  },
  mounted() {
    this.getPageList(this.page, this.limit);
  },
};
</script>
<style lang="sass">
.trademark-img
  width: 150px

.trademark-pagination
  text-align: right

>>>.el-pagination__sizes
  margin-left: 250px
</style>
