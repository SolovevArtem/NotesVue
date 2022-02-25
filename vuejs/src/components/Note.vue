<template>
  <div class="tc-note">
    <div tabindex="0" class="tc-note-header">
      <span v-on:click="deleteNote" class="tc-note-close"> X </span>
    </div>
    <div class="tc-note-title" contenteditable="" @blur="titleChanged">
      {{ note.title }}
    </div>
    <div class="tc-note-body" contenteditable="" @blur="bodyChanged">
      {{ note.text }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Note",
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  methods: {
    /* Instead of deleting the note here, 
    we just passing the index of a note that needs to be deleted
    to the parrent */
    deleteNote(event) {
      this.$emit("deleteNote", this.note);
    },
    titleChanged($event) {
      this.note.title = $event.target.innerHTML;
      this.$emit("noteUpdated", this.note);
    },
    bodyChanged($event) {
      this.note.text = $event.target.innerHTML;
      this.$emit("noteUpdated", this.note);
    },
  },
};
</script>

<style lang="scss" scoped>
$base-color: #219EBC;
$btn-color: #8ECAE6;
$nt1-color: #023047;
$nt2-color: #FFB703;
$nt3-color: #FB8500;
.tc-note {
  overflow:hidden;
  background-color: #f3d52f;
  border-radius: 1px;
  width: 225px;
  height: 225px;
  margin: 0 15px 40px;
  box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.2);
  transition: all 0.5s;
  cursor: pointer;
  &:hover, &:focus {
    box-shadow: 10px 10px 7px rgba(0, 0, 0, 0.7);
    transform: scale(1.25);
    position: relative;
    z-index: 5;
  }

  .tc-note-header {
    padding: 10px 16px 0;

    .tc-note-close {
      display: inline-block;
      width: 24px;
      height: 24px;
      border-radius: 50%;
      line-height: 24px;
      text-align: center;
      transition: all 0.3s;

      &:hover {
        background-color: rgba(0, 0, 0, 0.2);
      }

      &:focus {
        box-shadow: inset 2px 3px 0px rgba(0, 0, 0, 0.8);
      }
    }

    .tc-note-close {
      float: right;
    }
  }

  .tc-note-title {
    outline: 0;
    font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
    font-weight: bold;
    color: black;
    text-align: center;
  }
  .tc-note-body {
    outline: 0;
    font-family: "Reenie Beanie";
    font-size: 1.5rem !important;
    color: black;
  }

  .tc-note-title {
    font-size: 24px;
    padding: 10px 16px;
    font-weight: bold;
  }

  .tc-note-body {
    font-size: 20px;
    padding: 10px 16px 16px;
  }

  &:hover {
    box-shadow: 10px 10px 10px 7px rgba(33, 33, 33, 0.7);
  }
}
</style>
