<template>
  <div class="virtual-selcet">
    <input
      :value="inputValue"
      @input="inputChange"
      @focus="focus"
      @blur="blur"
      ref="input"
      class="input__inner"
      :readonly="filterable ? false : 'readonly'"
      :placeholder="$attrs.placeholder ? $attrs.placeholder : '请选择'"
    />
    <svg
      t="1630764985662"
      class="icon"
      :class="{ 'is-down': isDown }"
      viewBox="0 0 1024 1024"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      p-id="6653"
      width="24"
      height="24"
    >
      <path
        d="M500.8 461.909333L267.306667 695.296l-45.226667-45.269333 278.741333-278.613334L779.306667 650.026667l-45.248 45.226666z"
        p-id="6654"
        fill="#c0c4cc"
      ></path>
    </svg>
    <transition name="el-zoom-in-top">
      <virtual-list
        v-show="isListShow"
        class="virtual-list"
        :data-component="virtualSelcetItem"
        :data-key="dataKey"
        :data-sources="selectOptions"
        style="height: 360px; overflow-y: auto"
        item-class="virtual-list-item-class"
      />
    </transition>
  </div>
</template>
<script>
import VirtualList from "vue-virtual-scroll-list";
import virtualSelcetItem from "./virtual-selcet-item.vue";
import bus from "./bus";
export default {
  inheritAttrs: false,
  name: "virtualSelcet",
  watch: {
    options(newOptions) {
      this.cloneOptions = newOptions;
    },
  },
  props: {
    value: {
      type: String | Number | Object,
    },
    options: {
      type: Array,
      default() {
        return [];
      },
    },
    dataKey: {
      type: Function,
      default: function (option) {
        return option.value;
      },
    },
    filterable: {
      type: Boolean,
      default: true,
    },
    filterMethod: {
      type: Function,
      default(val, optionList) {
        return optionList.filter((item) =>
          item.label.toLowerCase().indexOf(val.toLowerCase()) == -1
            ? false
            : true
        );
      },
    },
  },
  components: {
    VirtualList,
  },
  data() {
    return {
      inputValue: null,
      selectValue: null,
      isListShow: false,
      virtualSelcetItem: virtualSelcetItem,
      placeholder: null,
      inputModel: false,
      cloneOptions: [],
      isDown: true, //箭头是否向下
    };
  },
  computed: {
    selectOptions() {
      return this.cloneOptions;
    },
  },
  methods: {
    inputChange(e) {
      let val = e.target.value;
      this.inputValue = val;
      this.inputModel = true; //输入模式
      if (this.filterMethod) {
        this.cloneOptions = this.filterMethod(val, this.options);
      }
    },
    focus(e) {
      this.cloneOptions = this.options;
      this.isListShow = true;
      this.inputValue = null;
      this.isDown = false;
      this.$emit("focus", e);
    },
    blur(e) {
      setTimeout(() => {
        if (this.inputValue == null) {
          this.inputValue = this.placeholder;
        }
        this.isListShow = false;
        this.isDown = true;
      }, 200);
      this.$emit("blur", e);
    },
  },
  mounted() {
    var unwatch = this.$watch(
      "value",
      (initVal) => {
        if (unwatch) {
          unwatch();
        } else {
          this.selectValue = initVal;
          const timer = setInterval(() => {
            if (this.options) {
              const option = this.options.find(
                (item) => this.dataKey(item) == initVal
              );
              if (option) {
                this.inputValue = option.label;
                this.placeholder = option.label;
              }
              clearInterval(timer);
            }
          }, 100);
        }
      },
      {
        immediate: true,
      }
    );
    bus.$on("virtual-selcet-change", (e) => {
      this.$refs.input.setAttribute("placeholder", e.label);
      this.inputValue = e.label;
      this.selectValue = e.value;
      this.$emit("input", e.value);
      this.$emit("change", e.value);
      bus.$emit("change", e.value);
      this.placeholder = e.label;
      this.isListShow = false;
    });
  },
};
</script>
<style scoped>
.virtual-selcet {
  position: relative;
}
.virtual-list {
  position: absolute;
  margin-top: 10px;
  border: 1px solid #e4e7ed;
  border-radius: 4px;
  background-color: #fff;
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%);
  left: 0;
  font-size: 14px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: #606266;
  line-height: 34px;
  box-sizing: border-box;
  cursor: pointer;
  min-width: 100%;
}
.virtual-list ::v-deep .virtual-list-item-class:hover {
  background-color: #f5f7fa;
}
.virtual-list::-webkit-scrollbar {
  width: 8px;
}
.virtual-list::-webkit-scrollbar-thumb {
  border-radius: 4px;
  background: rgb(223, 221, 221);
}
.input__inner {
  -webkit-appearance: none;
  background-color: #fff;
  background-image: none;
  border-radius: 4px;
  border: 1px solid #dcdfe6;
  box-sizing: border-box;
  color: #606266;
  display: inline-block;
  font-size: 14px;
  height: 40px;
  line-height: 40px;
  outline: none;
  padding: 0 35px 0 15px;
  transition: border-color 0.2s cubic-bezier(0.645, 0.045, 0.355, 1);
  width: 100%;
  cursor: pointer;
}

.input__inner:hover {
  border-color: #c0c4cc;
}
.input__inner:focus {
  border-color: #409eff !important;
}
.input__inner::-webkit-input-placeholder {
  color: #ccc4cc;
}
.input__inner::-moz-placeholder {
  /* Mozilla Firefox 19+ */
  color: #ccc4cc;
}
.input__inner:-moz-placeholder {
  /* Mozilla Firefox 4 to 18 */
  color: #ccc4cc;
}
.input__inner:-ms-input-placeholder {
  /* Internet Explorer 10-11 */
  color: #ccc4cc;
}
.icon {
  position: absolute;
  top: 8px;
  right: 5px;
  transition: 0.5s ease;
}
.is-down {
  transform: rotateZ(-180deg);
}
</style>