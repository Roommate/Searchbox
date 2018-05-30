<template>
     <div>
      <el-col :span="item.span">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-switch v-model="item.value"
              active-color="#13ce66"
              :disabled="item.disabled"
              inactive-color="#d7d7d7">
              </el-switch>
            </el-form-item>
          </el-form>
        </div>
      </el-col>
    </div>
</template>

<script>
export default {
  name: 'SearchSwitch',
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
    };
  },
  methods: {
    reset() {
      this.item.value = '';
    },
  },
};
</script>

<style scoped>
span{
    display: block;
    margin: 15px 0 5px 15px;
    font-size: 14px;
}
.el-switch{
  height:40px;
  line-height: 40px;
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
