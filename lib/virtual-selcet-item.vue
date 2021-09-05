<template>
  <div
    @click="itemClick(source)"
    :class="{ selceted: currentVal == source.value }"
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
  mounted() {
    this.currentVal = this.$parent.$parent.$parent.selectValue;
    bus.$on("change", (val) => {
      this.currentVal = val;
    });
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