<template>
  <div class="item">
    <span v-if="!isbeingchanged">{{ title }}</span>
    <button v-if="!isbeingchanged" v-on:click="onChange">
      Изменить
    </button>
    <input
      v-if="isbeingchanged"
      v-model="changedTodoText"
      placeholder="Например, покормить кота"
      class="changingTask-input"
    />
    <button v-if="isbeingchanged" v-on:click="onSave">
      Сохранить
    </button>
    <button v-if="isbeingchanged" v-on:click="onCancel">
      Отменить изменения
    </button>
    <button v-on:click="$emit('remove')">Удалить задачу</button>
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    title: String,
  },
  data() {
    return {
      changedTodoText: "",
      isbeingchanged: false,
    };
  },
  methods: {
    stateBeingChangedSwitch: function() {
      this.isbeingchanged = !this.isbeingchanged;
    },
    onSave() {
      this.$emit("savechange", this.changedTodoText);
      this.stateBeingChangedSwitch();
    },
    onCancel() {
      this.changedTodoText = "";
      // this.stateBeingChangedSwitch;
      this.stateBeingChangedSwitch();
    },
    onChange() {
      this.changedTodoText = this.title;
      this.stateBeingChangedSwitch();
    },
  },
};
</script>

<style scoped>
input {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  font-size: large;
}
::placeholder {
  font-size: larger;
}
.changingTask-input {
  width: 15%;
  padding: 0.4em;
}
button {
  padding: 0.4em 1.6em;
  /* margin-right: 1em; */
  font-size: large;
}
span {
  margin-right: 40px;
}
</style>
