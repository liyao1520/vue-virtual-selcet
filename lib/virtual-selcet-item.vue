<template>
  <div
    @click="itemClick(source)"
    :class="{ selceted: currentValue == source.value }"
  >
    {{ source.label }}
  </div>
</template>
<script>
import bus from "./bus";
export default {
  props: ["source"],
  methods: {
    itemClick(source) {
      bus.$emit("virtual-selcet-change", source);
      this.selceted = true;
    },
  },
  computed: {
    currentValue() {
      return this.$parent.$parent.$parent.selectValue;
    },
  },
  data() {
    return {
      selceted: false,
      currentVal: null,
    };
  },
};
</script>
<style scoped>
.selceted {
  color: #409eff;
  font-weight: 700;
}
</style>