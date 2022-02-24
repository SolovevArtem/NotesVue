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
  /* mounted() {
    fetch("http://localhost:3000/notes")
    .then(res=> res.json())
    .then(data => this.notes = data)
    .catch(err=> console.log(err.message))
  } */
};
</script>

<style lang="scss" scoped>
.tc-notes-wrapper {
  .new-note-btn {
    width: 200px;
    display: block;
    margin: 0 auto 20px;
    background-color: #fff;
    padding: 10px 32px;
    border: 1px solid #e0e0e0;
    font-size: 26px;
    outline: 0;
    transition: all 0.3s;
    cursor: pointer;
    font-family: "Caveat", cursive;

    &:hover {
      box-shadow: 0 5px 7px rgba(0, 0, 0, 0.1);
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
// "Deep" Style selectors to obtain child style property
/deep/ .tc-note:nth-of-type(1n) {
  transform: rotate(-5deg);
  position: relative;
  top: 5px;
}
/deep/ .tc-note:nth-of-type(even) {
  transform: rotate(4deg);
  position: relative;
  top: 5px;
}

/deep/ .tc-note:nth-of-type(3n) {
  transform: rotate(-3deg);
  position: relative;
  top: -5px;
}
/deep/ .tc-note:nth-of-type(5n) {
  transform: rotate(5deg);
  position: relative;
  top: -10px;
}
</style>
