<template>
  <div class="new-note-creator">
    <label>Priority <br>
<!--      <select class="priority" v-model="note.priorityLevel">-->
      <select class="priority">
        <option disabled value="">Choose note priority</option>
        <option value="-1">Low</option>
        <option value="0">Normal</option>
        <option value="1">High</option>
      </select>
    </label>

      <label>Note Title
<!--      <input v-model="note.title" type="text" maxlength="30">-->
      <input type="text" maxlength="30">
    </label>

    <label> Note Description
<!--      <textarea v-model="note.description" maxlength="50"></textarea>-->
      <textarea maxlength="50"></textarea>
    </label>

    <button class="btn" @click="addNewNote">Create New Note</button>
  </div>
</template>

<script>

export default {
  name: "NewNoteCreator",
  props: ["note"],
  methods: {
    addNewNote() {
      this.$emit('addNote', this.note)
    }
  }
}

</script>

<style scoped>

.priority {
  width: 100%;
  height: 50px;
  padding: 0 20px;
  background-color: white;
  color: black;
  border-radius: 20px;
  margin-bottom: 30px;
}
input {
  width: 100%;
  height: 50px;
  padding: 0 20px;
  background-color: white;
  color: black;
  border-radius: 20px;
  margin-bottom: 30px;
}
textarea {
  width: 100%;
  height: 50px;
  padding: 0 20px;
  background-color: white;
  color: black;
  border-radius: 20px;
  margin-bottom: 30px;
  font-family: "Arial Black";
  font-size: 20px;
}

select {
  background-image:
      linear-gradient(45deg, transparent 50%, gray 50%),
      linear-gradient(135deg, gray 50%, transparent 50%),
      linear-gradient(to right, #ccc, #ccc);
}

select {
  background-position:
      calc(99% - 20px) calc(1em + 5px),
      calc(99% - 15px) calc(1em + 5px),
      calc(99% - 2.5em) 0.75em;
}

select {
  background-size:
      5px 5px,
      5px 5px,
      1px 1.5em;
}

select.priority {
  background-repeat: no-repeat;
}

.new-note-creator {
  text-align: center;
  margin: 30px 0;
}
.btn {
  display: inline-block;
  padding: .8em 2em;
  line-height: 1;
  text-transform: uppercase;
  cursor: pointer;
  border-radius: 30px;
  margin: 20px 0;
  color: #ffffff;
  border: 1px solid #494ce8;
  background-color: #494ce8;
}
</style>