<template>
  <label for="category"
      >Category:<input
        id="category"
        type="text"
        placeholder="Category"
        :value="editedCategory"
        @input="$emit('update:editedCategory', $event.target.value)"
        required
    /></label>
  <label for="name"
      >Name:<input
        id="name"
        type="text"
        placeholder="Name"
        :value="editedName"
        @input="$emit('update:editedName', $event.target.value)"
        required
    /></label>
  <label for="red"
    >Red：<input
      id="red"
      placeholder="The range is 0 to 255."
      maxlength="3"
      :value="editedRed"
      @input="$emit('update:editedRed', $event.target.value)"
      oninput = "value=value.replace(/[^\d]/g,'')"
      required
    /></label>
    <label for="green"
    >Green：<input
      id="green"
      placeholder="The range is 0 to 255."
      maxlength="3"
      :value="editedGreen"
      @input="$emit('update:editedGreen', $event.target.value)"
      oninput = "value=value.replace(/[^\d]/g,'')"
      required
    /></label>
    <label for="blue"
    >Blue：<input
      id="blue"
      placeholder="The range is 0 to 255."
      maxlength="3"
      :value="editedBlue"
      @input="$emit('update:editedBlue', $event.target.value)"
      oninput = "value=value.replace(/[^\d]/g,'')"
      required
    /></label>
    <button type="button" class="btn" @click="createColor">Confirm</button>
</template>

<script>
export default {
  props: ['editedName','editedCategory','editedRed','editedGreen','editedBlue'],
  emits: ["createColor","update:editedCategory","update:editedName","update:editedRed","update:editedGreen","update:editedBlue"],
  methods: {
    createColor() {
      this.$emit("createColor", {
        editedRed: this.editedRed,
        editedGreen: this.editedGreen,
        editedBlue: this.editedBlue,
        editedCategory: this.editedCategory,
        editedName: this.editedName,
      });
    }
  },
};
</script>
