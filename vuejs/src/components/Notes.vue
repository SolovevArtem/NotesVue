<template>
  <div class="tc-notes-wrapper">
    <!-- <add-new-button @addNote="addNote" /> -->
    <button v-on:click="addNote" class="new-note-btn">New Note</button>
    <div class="tc-notes">
      <note
        v-for="(note, id) in notes"
        :key="id"
        :note="note"
        @deleteNote="deleteNote(note.id)"
        @noteUpdated="noteUpdated"
      />
    </div>
  </div>
</template>

<script>
// import AddNewButton from "./AddNewButton";
import Note from "./Note";
import axios from "axios";

export default {
  components: { Note },
  name: "Notes",
  data() {
    return {
      notes: [],
    };
  },
  methods: {
    deleteNote(id) {
      // this.notes.splice(this.notes.indexOf(note), 1);
      // console.log("deleted");
      axios.delete(`http://localhost:3000/notes/${id}`);
      this.notes = this.notes.filter((obj) => obj.id !== id);
    },

    async addNote() {
      /* this.notes.unshift({title:'', body:''}) */
      try {
        const res = await axios.post(`http://localhost:3000/notes`, {
          title: "",
          text: "",
        });
        this.notes = [...this.notes, res.data];
      } catch (error) {
        console.log(error.message);
      }
    },
    noteUpdated(note) {
      axios.put(`http://localhost:3000/notes/${note.id}`, {
        title: note.title,
        text: note.text,
      });
    },
  },
  async created() {
    try {
      const res = await axios.get("http://localhost:3000/notes");
      this.notes = res.data;
    } catch (error) {
      console.log(error.message);
    }
  },
};
</script>

<style lang="scss" scoped>
$base-color: #219ebc;
$btn-color: #8ecae6;
$nt1-color: #023047;
$nt2-color: #ffb703;
$nt3-color: #fb8500;

.tc-notes-wrapper {
  
  .new-note-btn {
    width: 200px;
    display: block;
    margin: 0 auto 20px;
    background-color: $btn-color;
    padding: 10px 32px;
    border: 3px solid white;
    font-size: 26px;
    outline: 2;
    transition: all 0.5s;
    cursor: pointer;
    font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
    font-weight: bold;

    &:hover {
      box-shadow: 8px 8px 8px rgba(0, 0, 0, 0.1);
      background-color: #4b8098;
      color: white;
    }

    &:active {
      position: relative;
      top: 1px;
    }
  }

  .tc-notes {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    margin: 0 auto;
  }
}
// // "Deep" Style selectors to obtain child style property
/deep/ .tc-note:nth-of-type(1) {
  transform: rotate(-5deg);
  position: relative;
  top: 5px;
  background-color: $nt1-color;

  .tc-note-header .tc-note-close,
  .tc-note-title,
  .tc-note-body {
    color: $nt2-color;
  }
}
/deep/ .tc-note:nth-of-type(even) {
  transform: rotate(4deg);
  position: relative;
  top: 5px;
  background-color: $nt2-color;
}

/deep/ .tc-note:nth-of-type(3n) {
  transform: rotate(3deg);
  position: relative;
  top: -5px;
  background-color: $nt3-color;
}
/deep/ .tc-note:nth-of-type(5n) {
  transform: rotate(-5deg);
  position: relative;
  top: -10px;
  background-color: $nt1-color;

  .tc-note-header .tc-note-close,
  .tc-note-title,
  .tc-note-body {
    color: $nt2-color;
  }
}
</style>
