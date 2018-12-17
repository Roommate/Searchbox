SearchBox主要是作为后台管理页面的查询条件使用，所覆盖功能包括输入框、下拉框、日期、时间、文本框及开关等。
使用时仅需要引入模块并配置好参数即可。

组件对应功能如下：
SearchCheckBox 复选框（不常用，使用时尽量安排在最后一排）
SearchCityCascader 是用来查询省市区三级联动的
SearchDataPicker 用来查询日期
SearchDataTimePicker 用来查询日期加时间
SearchDataTimeRangePicker 用来查询日期时间范围
SearchInput 普通的输入框
SearchInputNumber 数字的输入框（不常用，一般用普通输入框加对输入值的验证来模拟数字输入框）
SearchInputNumber2 数字范围的输入框（同上，不常用）
SearchMultiSelect 多选下拉框，可选多个下拉框值
SearchSelect 普通的下拉框
SearchSelect2 可查询的下拉框
SearchSelect3 可查询并且可以自定义内容的下拉框
SearchSelect4 name--code 的下拉框，根据查询条件下拉框显示name与code值
SearchSelectAndInput 由下拉框和输入框组成，下拉框定义项，输入框定义项的值
SearchSwitch 开关
SearchTextarea 文本框

<template>
  <div class="searchWarp">
      <p v-if="title" class = "title">{{$t('libsKey.key28')}}</p>
      <el-row>
        <div class="grid-content">
          <component
          v-for="(item, index) in items"
          v-if="!newFold||index<items[0].num"
          :key="index"
          :is="item.type"
          :item="item"
          :form="form"
          :ref="item.name"
          @dateBlur="dateBlur(item,index,items)"
          @keyup.native.13="focusJump(index,items)"
          @change="handleChange"></component>
        </div>
      </el-row>
      <div v-if="!items[0].fade">
          <el-row  v-if="newFold" class="FR MT45">
              <div v-if="!hideBtn">
              <el-button plain @click="reset()">{{$t('libsKey.key29')}}</el-button>
              <el-button type="primary" @click="print()">{{ btnVal }}</el-button>
              <span v-if="items.length>items[0].num" class="switch-up"
                  @click="foldBox">{{$t('libsKey.key30')}}<i class="el-icon-arrow-down"></i></span>
              </div>
              <div v-else>
                <span v-if="items.length>items[0].num" class="switch-up"
                  @click="foldBox">{{$t('libsKey.key30')}}<i class="el-icon-arrow-down"></i></span>
              </div>
          </el-row>
          <el-row  v-else class="FR">
              <el-button plain @click="reset()">{{$t('libsKey.key29')}}</el-button>
              <span v-if="!hideBtn">
                <el-button type="primary" @click="print()">{{ btnVal }}</el-button>
              </span>
              <el-button type="primary" v-for="(item, index) in btnList" :key="index"
                @click="print(item.info)">{{ item.val }}</el-button>
              <span v-if="items.length>items[0].num" class="switch-up"
                  @click="foldBox">{{$t('libsKey.key31')}}<i class="el-icon-arrow-up"></i></span>
          </el-row>
      </div>
  </div>
</template>

<script>
import { renderSelect } from '@/libs/utils/datatransform';
import Locale from '@/libs/mixins/locale';
import { t } from '@/libs/Locale';

import SearchInput from './Models/SearchInput';
import SearchSelectAndInput from './Models/SearchSelectAndInput';
import SearchSelect from './Models/SearchSelect';
import SearchMultiSelect from './Models/SearchMultiSelect';
import SearchSelect2 from './Models/SearchSelect2';
import SearchSelect3 from './Models/SearchSelect3';
import SearchSelect4 from './Models/SearchSelect4';
import SearchSwitch from './Models/SearchSwitch';
import SearchDataPicker from './Models/SearchDataPicker';
import SearchDataTimePicker from './Models/SearchDataTimePicker';
import SearchDataTimeRangePicker from './Models/SearchDataTimeRangePicker';
import SearchTextarea from './Models/SearchTextarea';
import SearchInputNumber from './Models/SearchInputNumber';
import SearchInputNumber2 from './Models/SearchInputNumber2';
import SearchCheckBox from './Models/SearchCheckBox';
import SearchCityCascader from './Models/SearchCityCascader';

export default {
  mixins: [Locale],
  name: 'searchBox',
  props: {
    // 要传入的表格
    items: {
      type: Array,
      default() {
        return [];
      },
    },
    // 级联选择时传入的数组
    contact: {
      type: Array,
      default() {
        return [];
      },
    },
    form: {
      type: Array,
      default() {
        return [];
      },
    },
    inpSel: {
      type: Boolean,
      default() {
        return false;
      },
    },
    // SearchBox 标题
    title: {
      type: Boolean,
      default() {
        return false;
      },
    },
    // 是否隐藏“查询”按钮
    hideBtn: {
      type: Boolean,
      default() {
        return false;
      },
    },
    // “查询”按钮的显示内容，默认为“查询”
    btnVal: {
      type: String,
      default() {
        return t('libsKey.key32');
      },
    },
    // 拓展按钮，通常隐藏“重置”和“查询”按钮后根据自身需求设置
    btnList: {
      type: Array,
      default() {
        return [];
      },
    },
    // 查询时是否要带上未填写的查询项
    searchAllInfo: {
      type: Boolean,
      default() {
        return false;
      },
    },
    // 折叠标识
    fold: {
      type: Boolean,
      default() {
        return true;
      },
    },
  },
  data() {
    return {
      newFold: this.fold,
    };
  },
  components: {
    SearchInput,
    SearchSelectAndInput,
    SearchSelect,
    SearchMultiSelect,
    SearchSelect2,
    SearchSelect3,
    SearchSelect4,
    SearchSwitch,
    SearchDataPicker,
    SearchDataTimePicker,
    SearchDataTimeRangePicker,
    SearchTextarea,
    SearchInputNumber2,
    SearchInputNumber,
    SearchCheckBox,
    SearchCityCascader,
  },
  methods: {
    // 查询时处理数据
    print(val) {
      // 判断是否通过验证
      const result = this.multiValidate();
      let param = {};
      // 通过验证并且只保留有值的项
      if (result && !this.searchAllInfo) {
        Object.keys(result).forEach((key) => {
          if (typeof (result[key]) === 'string') {
            if (result[key].trim() !== '') {
              param[key] = result[key].trim();
            }
          } else if (result[key] !== '') {
            param[key] = result[key];
          }
        });
      } else { // 通过验证并且搜索所有的项
        param = result;
        Object.keys(result).forEach((key) => {
          if (typeof (result[key]) === 'string') {
            param[key] = result[key].trim();
          } else {
            param[key] = result[key];
          }
        });
      }
      if (param) this.$emit('search', param, val);
      console.log(param, val);
      return param;
    },
    // 验证方法
    multiValidate() {
      let valids = true;
      const result = {};
      const validates = (dom) => {
        if (dom) {
          dom.$refs.item.validate((valid) => {
            if (!valid) {
              valids = false;
              return false;
            }
            if (dom.item.name) {
              result[dom.item.name] = dom.item.value;
            }
            return true;
          });
        }
      };
      const obj = this.$refs;
      Object.keys(obj).forEach((key) => {
        validates(obj[key][0]);
      });
      if (!valids) return false;
      return result;
    },
    // 折叠
    foldBox() {
      this.newFold = !this.newFold;
    },
    // 重置
    reset() {
      const obj = this.$refs;
      Object.keys(obj).forEach((key) => {
        if (obj[key][0]) {
          obj[key][0].reset();
        }
      });
    },
    // 搜索框的change事件，用来级联处理
    handleChange(val, item) {
      this.$emit('searchChange', item);
      for (let i = 0, l = this.contact.length; i < l; i += 1) {
        const one = this.contact[i];
        for (let j = 0, ll = one.length; j < ll; j += 1) {
          if (item.name === one[j].id) {
            if (j === one.length - 1) return;
            one[j].func({
              [item.name]: val,
            }).then((res) => {
              if (res.data) {
                renderSelect(this.items, res.data, one[j + 1].id, one[j + 1].id, one[j + 1].name);
              }
            });
          }
        }
      }
    },
    // 光标流转
    focusJump(index, items, isloop) {
      if (!isloop) {
        this.$refs[items[index].name][0].blur();
      }
      for (let i = index + 1; i < items.length; i += 1) {
        if (!items[i].disabled) {
          this.$refs[items[i].name][0].focus();
          return;
        }
      }
    },
    // 失去焦点（辅助光标流转功能）
    dateBlur(item, index, items) {
      if (item.value && item.value.length) {
        this.focusJump(index, items, true);
      }
    },
  },
};
</script>

<style scoped>
.searchWarp{
    border-radius: 3px;
    background: #fff;
    min-height: 140px;
    overflow: auto;
}
.FR{
    float: right;
    margin:25px;
    line-height: 40px;
}
.FR>span{
    margin:0 10px;
}
.grid-content{
    margin:10px;
}
.el-switch{
    display: block;
    line-height: 40px;
    height:40px;
}
.MT45{
    margin-top: -50px;
}
.title{
  line-height: 60px;
  background: #fff;
  font-size: 18px;
  color:#777;
  padding-left: 22px;
  border-bottom: 1px solid #e0e0e0;
}
/* span{
    display: block;
} */
</style>
