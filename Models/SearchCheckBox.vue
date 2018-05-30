<template>
    <div>
        <el-col :span="24">
        <div class="grid-content bg-purple">
          <!-- <span>{{item.title}}</span> -->
          <el-form :model="item" ref="item" :rules="rules">
            <el-form-item prop="value" :label='item.title'>
              <el-checkbox-group v-model="item.checkList">
                <el-checkbox v-for="(list,index) in item.check"
                :key="index" :disabled="item.disabled" :label="list"></el-checkbox>
              </el-checkbox-group>
            </el-form-item>
           </el-form>

        </div>
      </el-col>
    </div>
</template>

<script>
export default {
  name: 'SearchCheckBox',
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
.el-checkbox-group{
  width:calc(100% - 20px);
  /* padding: 10px 10px 15px; */
}
.el-form-item{
  margin-bottom: 10px;
}
</style>
