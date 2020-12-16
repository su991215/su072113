<template>
  <el-card style="margin-top: 20px">
    <el-form label-width="80px">
      <el-form-item label="SPU名称" prop="spuName ">
        <el-input v-model="spuName" placeholder="请输入名称"></el-input>
      </el-form-item>
      <el-form-item label="SPU品牌" prop="tmId">
        <el-select v-model="tmId" placeholder="请选择品牌">
          <el-option>你大爷</el-option>
          <el-option>你二大爷</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="SPU描述" prop="description">
        <el-input
          v-model="description"
          type="textarea"
          placeholder="请输入描述"
        ></el-input>
      </el-form-item>
      <el-form-item label="SPU图片" prop="imageList">
        <!-- <el-upload
          action="https://jsonplaceholder.typicode.com/posts/"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
        > -->
        <!-- <i class="el-icon-plus"></i>
        </el-upload> -->
        <el-upload
          class="avatar-uploader"
          list-type="picture-card"
          :action="`${$BASE_API}/admin/product/fileUpload`"
        >
          <i class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
        <span>只能上传jpg/png文件，且不超过50kb</span>
      </el-form-item>
      <el-form-item label="销售属性" prop="销售">
        <el-select placeholder="请选择品牌">
          <el-option>你大爷</el-option>
          <el-option>你二大爷</el-option>
        </el-select>
        <el-button
          type="primary"
          placeholder="请添加销售属性"
          icon="el-icon-plus"
          >添加销售属性</el-button
        >
        <el-table :data="[]" border style="width: 100%; margin: 20px 0">
          <el-table-column type="index" label="序号" width="80" align="center">
          </el-table-column>
          <el-table-column prop="attrName" label="属性名称" width="150">
          </el-table-column>

          <el-table-column label="属性值列表">
            <template v-slot="{ row }">
              <el-tag
                style="margin-right: 5px"
                v-for="attrVal in row.attrValueList"
                :key="attrVal.id"
                >{{ attrVal.valueName }}</el-tag
              >
            </template>
          </el-table-column>
          <el-table-column label="操作~~~~" width="150">
            <template v-slot="{ row }">
              <el-button
                type="warning"
                icon="el-icon-edit"
                size="mini"
                v-loading.fullscreen.lock="fullscreenLoading"
                @click="update(row)"
              ></el-button>
              <el-button
                type="danger"
                icon="el-icon-delete"
                size="mini"
                v-loading.fullscreen.lock="fullscreenLoading"
                @click="openFullScreen1"
              ></el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-form-item>
      <el-form-item>
        <el-button type="primary">保存</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script>
export default {
  name: "SpuUpdateList",
  data() {
    return {
      spuName: "",
      tmId: "",
      description: [],
    };
  },
};
</script>

<style lang='less' scoped>
</style>
