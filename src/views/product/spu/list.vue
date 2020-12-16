<template>
  <div>
    <!--
      @change 当三级分类修改的时候触发，得到所有的分类id
       @clearList 当一级分类和2级分类触发的时候触发，清空分类
       :disabled select是否可以使用
     -->
    <Category :disabled="!isShowList" />
    <SpuShowList v-show="isShowList" />
    <SpuUpdateListv-else :item="item" />
  </div>
</template>


<script>
import Category from "@/components/Category";
import SpuShowList from "./spuShowList";
import SpuUpdateList from "./spuUpdateList";

export default {
  name: "SpuList",
  data() {
    return {
      isShowList: false,
    };
  },
  components: {
    Category,
    SpuShowList,
    SpuUpdateList,
  },
};
</script>
