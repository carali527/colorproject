<template>
  <slot></slot>
  <div class="card-body">
    <p>{{ item.name }}</p>
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