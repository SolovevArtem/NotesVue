<template>
  <div class="tc-notes-wrapper">
    <div class="header">
      <div class="title">Notes App</div>
      <!-- <add-new-button @addNote="addNote" /> -->
      <button v-on:click="addNote" class="new-note-btn btn">
        <a href="#">New Note</a>
      </button>

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
  a {
    color: black !important;
    text-decoration: none;
  }
  .header {
    display: inline-flex;
    justify-content: space-between;
    margin: auto;
    width: 50%;
  }
  .github {
    width: 200px;
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
    width: 200px;
    margin: auto 5%;
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

// BUTTON CSS

$yellow: #ffe800;
$brown: #404040;
$blue: #1cd3f8;
$angle: 8px;
$angle-o: 4px;

html,
body {
  height: 100%;
  font-family: "Open Sans", Arial, Helvetica, sans-serif;
  color: #f2f2f2;
  text-transform: uppercase;
  letter-spacing: 1px;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: $blue;
}

.btn {
  
  top: 0;
  left: 0;
  transition: all 0.15s linear 0s;
  position: relative;
  display: inline-block;
  padding: 15px 25px;
  background-color: $yellow !important;

  text-transform: uppercase;
  color: $brown;
  font-family: arial;
  letter-spacing: 1px;

  box-shadow: -6px 6px 0 $brown;
  text-decoration: none;

  &:hover {
    top: 3px;
    left: -3px;
    box-shadow: -3px 3px 0 $brown;

    &::after {
      top: 1px;
      left: -2px;
      width: $angle-o;
      height: $angle-o;
    }

    &::before {
      bottom: -2px;
      right: 1px;
      width: $angle-o;
      height: $angle-o;
    }
  }

  &::after {
    transition: all 0.15s linear 0s;
    content: "";
    position: absolute;
    top: 2px;
    left: -4px;
    width: $angle;
    height: $angle;
    background-color: $brown;
    transform: rotate(45deg);
    z-index: -1;
  }

  &::before {
    transition: all 0.15s linear 0s;
    content: "";
    position: absolute;
    bottom: -4px;
    right: 2px;
    width: $angle;
    height: $angle;
    background-color: $brown;
    transform: rotate(45deg);
    z-index: -1;
  }
}

a.btn {
  position: relative;
}

a:active.btn {
  top: 6px;
  left: -6px;
  box-shadow: none;

  &:before {
    bottom: 1px;
    right: 1px;
  }

  &:after {
    top: 1px;
    left: 1px;
  }
}
</style>
