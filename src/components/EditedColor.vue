<template>
  <slot></slot>
  <div class="card-body-editing">
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
      >Red：
      <input
        id="red"
        type="number"
        placeholder="Max with three numbers"
        maxlength="3"
        :value="editedRed"
        @input="$emit('update:editedRed', $event.target.value)"
        oninput = "value=value.replace(/[^\d]/g,'')"
        required
    /></label>
    <label for="green"
      >Green：
      <input
        id="green"
        type="number"
        placeholder="Max with three numbers"
        maxlength="3"
        :value="editedGreen"
        @input="$emit('update:editedGreen', $event.target.value)"
        oninput = "value=value.replace(/[^\d]/g,'')"
        required
    /></label>
    <label for="blue"
      >Blue：
      <input
        id="blue"
        type="number"
        placeholder="Max with three numbers"
        maxlength="3"
        :value="editedBlue"
        @input="$emit('update:editedBlue', $event.target.value)"
        oninput = "value=value.replace(/[^\d]/g,'')"
        required
    /></label>
    <button class="btn" @click="completeColorItem">Confirm</button>
    <button class="btn" @click="editingCancel">Cancel</button>
  </div>
</template>

<script>
export default {
    props: ['editedName','editedRed','editedGreen','editedBlue','editing'],
    emits: ["editingCancel","completeColorItem","update:editedName","update:editedRed","update:editedGreen","update:editedBlue"],
    methods: {
        completeColorItem() {
        this.$emit("completeColorItem", {
            editedRed: this.editedRed,
            editedGreen: this.editedGreen,
            editedBlue: this.editedBlue,
            editedName: this.editedName
        });
        },
        editingCancel() {
            this.$emit("editingCancel", {
                editedRed : "",
                editedGreen : "",
                editedBlue : "",
                editedName : "",
            });
        }
    }
};
</script>

<style>
</style>