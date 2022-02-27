<template>
  <div class="tc-notes-wrapper">
    <div class="header">
      <div class="title">Notes App</div>
      <!-- <add-new-button @addNote="addNote" /> -->
      <button v-on:click="addNote" class="new-note-btn btn">New Note</button>

      <button class="github btn">
        <a href="https://github.com/SolovevArtem/NotesVue" target="_blank"
          >GitHub</a
        >
      </button>
    </div>
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
      axios.delete(`https://notes-app-json-server.herokuapp.com/notes/${id}`);
      this.notes = this.notes.filter((obj) => obj.id !== id);
    },

    async addNote() {
      /* this.notes.unshift({title:'', body:''}) */
      try {
        const res = await axios.post(
          `https://notes-app-json-server.herokuapp.com/notes`,
          {
            title: "",
            text: "",
          }
        );
        this.notes = [...this.notes, res.data];
      } catch (error) {
        console.log(error.message);
      }
    },
    async noteUpdated(note) {
      try {
        axios.put(
          `https://notes-app-json-server.herokuapp.com/notes/${note.id}`,
          {
            title: note.title,
            text: note.text,
          }
        );
      } catch (error) {
        console.log(error.message);
      }
    },
  },
  async created() {
    try {
      const res = await axios.get(
        "https://notes-app-json-server.herokuapp.com/notes"
      );
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
  .btn {
    outline: none !important;
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
    display: inline-block !important;

    .btn-4 {
  width: 130px;
  height: 40px;
  line-height: 42px;
  padding: 0;
  border: none;
}
.btn-4 span {
  position: relative;
  display: block;
  width: 100%;
  height: 100%;
}
.btn-4:before,
.btn-4:after {
  position: absolute;
  content: "";
  right: 0;
  top: 0;
  background: #000;
  transition: all 0.3s ease;
}
.btn-4:before {
  height: 50%;
  width: 2px;
}
.btn-4:after {
  width: 20%;
  height: 2px;
}
.btn-4:hover:before {
  height: 100%;
}
.btn-4:hover:after {
  width: 100%;
}
.btn-4 span:before,
.btn-4 span:after {
  position: absolute;
  content: "";
  left: 0;
  bottom: 0;
  background: #000;
  transition: all 0.3s ease;
}
.btn-4 span:before {
  width: 2px;
  height: 50%;
}
.btn-4 span:after {
  width: 20%;
  height: 2px;
}
.btn-4 span:hover:before {
  height: 100%;
}
.btn-4 span:hover:after {
  width: 100%;
}
  }
  .header {
    display: inline-flex;
    justify-content: space-between;
    margin: auto;
    width: 50%;
  }
  .github {
    width: 350px;
    margin: auto 0%;
    display: inline-block;
    background-color: #1fe0;
    color: white;
    border: none;
    padding: 10px 20px;
    font-size: 15px;
    cursor: pointer;
    font-family: "Amatic SC", cursive;
    font-family: "Roboto Mono", monospace;
    font-family: "Sora", sans-serif;

    a {
      color: inherit;
      text-decoration: none;
    }

    
  }
  .title {
    width: 400px;
    margin: auto;
    display: inline-block;
    background-color: #1fe0;
    color: white;
    border: none;
    padding: 10px 32px;
    font-size: 60px;
    cursor: pointer;
    font-family: "Amatic SC", cursive;
    font-family: "Roboto Mono", monospace;
    font-family: "Sora", sans-serif;
  }
  .new-note-btn {
    width: 350px;
    margin-left: auto;
    margin-right: 0%;
    display: inline-block;
    background-color: #1fe0;
    color: white;
    border: none;
    padding: 10px 70px;
    font-size: 15px;
    cursor: pointer;
    font-family: "Amatic SC", cursive;
    font-family: "Roboto Mono", monospace;
    font-family: "Sora", sans-serif;
  }

  .tc-notes {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    margin: 5% auto;
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
