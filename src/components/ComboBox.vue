<template>
  <div>
    <v-combobox
      outlined
      :label="label"
      :items="data"
      v-model="currentItem"
      :item-text="textProp"
    >
    </v-combobox>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentItem: "",
    };
  },
  props: {
    data: {
      type: Array,
    },
    label: {
      type: String,
    },
    width: {
      type: Number,
      default: 600,
    },
  },
  watch: {
    currentItem() {
      this.$emit("input", this.currentItem);
    },
  },
};
</script>
