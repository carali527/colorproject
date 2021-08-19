<template>
  <slot></slot>
  <div class="card-body">
    <p>Name：{{ item.name }}</p>
    <p>Red：{{ item.red }}</p>
    <p>Green：{{ item.green }}</p>
    <p>Blue：{{ item.blue }}</p>
    <button class="btn" @click="editedTheColors">edit</button>
    <button class="btn" @click="deleteTheColor">delete</button>
  </div>
</template>

<script>
export default {
  props: ["item"],
  emits: ["editedTheColors", "deleteTheColor"],
  methods: {
    editedTheColors() {
      this.$emit("editedTheColors", {
        editing: this.item,
      });
    },
    deleteTheColor() {
      this.$emit("deleteTheColor", {
        item: this.item,
      });
    },
  },
};
</script>

<style></style>
