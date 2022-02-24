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
      this.notes=this.notes.filter((obj)=>obj.id!==id);
    },

    async addNote() {
      /* this.notes.unshift({title:'', body:''}) */
        try{
        const res = await axios.post(`http://localhost:3000/notes`, {
          title: "",
          text: "",
        });
        this.notes = [...this.notes, res.data];

      } catch (error){
        console.log(error.message);
      }
    },
    noteUpdated(note){
      axios.put(`http://localhost:3000/notes/${note.id}`,
      {
        title: note.title,
        text: note.text,
      }
      )
    }
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

    .tc-note {
      background-color: #f0c806;
      border-radius: 8px;
      width: 280px;
      margin: 0 10px 20px;
      box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.2);
      transition: all 0.5s;
      cursor: pointer;
      font-family: "Caveat", cursive;

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

      .tc-note-title,
      .tc-note-body {
        outline: 0;
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
        box-shadow: 2px 4px 10px rgba(0, 0, 0, 0.3);
      }
    }
  }
}
</style>
