<template>
  <div class="mainPageSiteCreating">
    <div class="mainPageSiteLeftSideBarCreating">
      <div v-if="notes.length === 0" class="no-notes">У вас нет заметок!</div>
      <div v-for="(note, index) in notes" :key="index" class="note-box">
        <h3>{{ note.title }}</h3>
        <p>{{ note.text }}</p>
        <div class="status">
          <span @click="toggleStatus(index)">
            {{ note.completed ? "✅" : "❌" }}
            <!-- Галочка или крестик -->
          </span>
        </div>
        <button @click="deleteNote(index)" class="delete-button">
          Удалить
        </button>
        <button @click="editNote(index)" class="edit-button">
          Редактировать
        </button>
      </div>
    </div>

    <div class="mainPageSiteNoteWindowCreating">
      <input
        type="text"
        v-model="title"
        placeholder="Заголовок"
        class="mainPageSiteNoteWindowTitleCreating"
      />
      <input
        type="text"
        v-model="text"
        placeholder="Текст"
        class="mainPageSiteNoteWindowTextCreating"
      />
      <button @click="addNote" class="buttonAccept">
        {{ editingIndex !== null ? "Сохранить" : "Отправить" }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notes: [],
      title: null,
      text: null,
      editingIndex: null,
    };
  },
  mounted() {
    const savedNotes = localStorage.getItem("notes");
    if (savedNotes) {
      this.notes = JSON.parse(savedNotes);
    }
  },
  methods: {
    addNote() {
      if (this.title.trim() && this.text.trim()) {
        if (this.editingIndex !== null) {
          this.notes[this.editingIndex] = {
            title: this.title,
            text: this.text,
            completed: false, // Статус выполнения
          };
          this.editingIndex = null;
        } else {
          this.notes.push({
            title: this.title,
            text: this.text,
            completed: false,
          });
        }
        this.saveNotesToLocalStorage();
        this.title = "";
        this.text = "";
      }
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
      this.saveNotesToLocalStorage();
    },
    editNote(index) {
      this.title = this.notes[index].title;
      this.text = this.notes[index].text;
      this.editingIndex = index;
    },
    toggleStatus(index) {
      this.notes[index].completed = !this.notes[index].completed;
      this.saveNotesToLocalStorage();
    },
    saveNotesToLocalStorage() {
      localStorage.setItem("notes", JSON.stringify(this.notes));
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
}
.mainPageSiteCreating {
  display: flex;
  width: 100%;
  height: 100vh;
  background-color: aliceblue;
}

.mainPageSiteLeftSideBarCreating {
  width: 30%;
  padding: 10px;
  border-right: 2px solid #000;
  overflow-y: auto;
  .note-box {
    background: white;
    padding: 10px;
    margin: 10px 0;
    border-radius: 5px;
    box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.1);
  }
  .status {
    font-size: 20px;
    cursor: pointer;
  }
  .delete-button {
    background-color: red;
    color: white;
    border: none;
    padding: 5px;
    cursor: pointer;
    margin-top: 5px;
    border-radius: 10px;
  }
  .edit-button {
    background-color: orange;
    color: white;
    border: none;
    padding: 5px;
    cursor: pointer;
    margin-top: 5px;
    margin-left: 10px;
    border-radius: 10px;
  }
}

.mainPageSiteNoteWindowCreating {
  width: 50%;
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  .mainPageSiteNoteWindowTitleCreating,
  .mainPageSiteNoteWindowTextCreating {
    width: 100%;
    margin: 5px 0;
    padding: 10px;
  }
  .buttonAccept {
    background-color: green;
    color: white;
    border: none;
    padding: 10px;
    cursor: pointer;
    user-select: none;
  }
}
</style>
