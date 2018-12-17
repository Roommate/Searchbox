<template>
      <el-col :span="item.span">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-cascader v-model="item.value"
                :disabled="item.disabled"
                :placeholder="item.placeholder"
                :options="cityList||[]"
                :props="props"
                :clearable="true"
                filterable
                change-on-select
              ></el-cascader>
            </el-form-item>
          </el-form>
      </el-col>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  name: 'SearchCascader',
  props: {
    item: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      rules: {
        value: this.item.rule,
      },
      props: {
        value: this.item.typeValue || 'id',
        label: 'name',
      },
    };
  },
  computed: {
    ...mapGetters([
      'cityList',
    ]),
  },
  methods: {
    reset() {
      this.item.value = [];
    },
  },
};
</script>

<style scoped>
/* span{
    display: block;
    margin: 5px 0 0 15px;
    font-size: 14px;
} */
.el-cascader{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
