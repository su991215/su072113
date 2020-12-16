<template>
  <div>
    <Category @change="getAttrList" :disabled="!isShowList" />

    <el-card v-show="isShowList" style="margin-top: 20px">
      <el-button
        type="primary"
        icon="el-icon-plus"
        v-loading.fullscreen.lock="fullscreenLoading"
        @click="openFullScreen1"
        >添加属性</el-button
      >

      <el-table :data="attrList" border style="width: 100%; margin: 20px 0">
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
    </el-card>
    <el-card v-show="!isShowList" style="margin-top: 20px">
      <el-form :model="attr" inline>
        <el-form-item label="属性名" prop="attrName">
          <el-input v-model="attr.attrName"></el-input>
        </el-form-item>
      </el-form>
      <el-button
        type="primary"
        icon="el-icon-plus"
        v-loading.fullscreen.lock="fullscreenLoading"
        @click="openFullScreen1"
        >添加属性</el-button
      >

      <el-table
        :data="attr.attrValueList"
        border
        style="width: 100%; margin: 20px 0"
      >
        <el-table-column type="index" label="序号" width="80" align="center">
        </el-table-column>
        <el-table-column label="属性值名称">
          <template v-slot="{ row, $index }">
            <el-input
              v-if="row.edit"
              v-model="row.valueName"
              @blur="editCompleted(row, $index)"
              @keyup.enter.native="editCompleted(row, $index)"
              autofocus
              ref="input"
              size="mini"
            ></el-input>
            <span
              v-else
              @click="edit(row)"
              style="display: block; width: 100%"
              >{{ row.valueName }}</span
            >
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template v-solt="{ row, $index }">
            <el-popconfirm
              @onConfirm="delAttrValue($index)"
              :title="`确定删除 ${row.valueName} 吗？`"
              ><el-button
                type="danger"
                icon="el-icon-delete"
                size="mini"
                slot="reference"
                v-loading.fullscreen.lock="fullscreenLoading"
                @click="openFullScreen1"
              ></el-button>
            </el-popconfirm>
          </template>
        </el-table-column>
      </el-table>
      <el-button
        type="primary"
        v-loading.fullscreen.lock="fullscreenLoading"
        @click="save"
        >保存</el-button
      >
      <el-button @click="isShowList = true">取消</el-button>
    </el-card>
  </div>
</template>

<script>
// categoryId:61
// categoryLevel:3
// id:3467
// attrName:"屏幕尺寸"
// attrValueList:Array[3]
//   attrId:3467
//   id:18904
//   valueName:"5.0~5.49英寸"

import Category from "@/components/Category";
export default {
  name: "AttrList",
  data() {
    return {
      attrList: [],
      fullscreenLoading: false,
      isShowList: true,

      attr: {
        attrName: "",
        attrValueList: [],
      },
    };
  },
  methods: {
    editCompleted(row, index) {
      if (!row.valueName) {
        this.attr.attrValueList.splice(index, 1);
        return;
      }
      row.edit = false;
    },
    async save() {
      this.fullscreenLoading = true;
      const result = await this.$API.attrs.saveAttrInfo(this.attr);
      if (result.code === 200) {
        this.$message.success("更新属性成功~");
        this.isShowList = true;
        this.getAttrList(this.category);
      } else {
        this.$message.error(result.message);
      }
      this.fullscreenLoading = false;
    },
    delAttrValue(index) {
      console.log(index);
      this.attr.attrValueList.splice(index, 1);
    },
    addAttrValue() {
      this.attr.attrValueList.push({ edit: true });
      this.$nextTick(() => {
        this.$refs.input.focus();
      });
    },
    edit(row) {
      this.$set(row, "edit", true);
      this.$nextTick(() => {
        this.$refs.input.focus();
      });
    },
    update(attr) {
      this.attr = JSON.parse(JSON.stringify(attr));
      isShowList = false;
    },
    getAttrList(attrList) {
      this.attrList = attrList;
    },
    openFullScreen1() {
      this.fullscreenLoading = true;
      setTimeout(() => {
        this.fullscreenLoading = false;
      }, 2000);
    },
  },
  components: {
    Category,
  },
};
</script>
