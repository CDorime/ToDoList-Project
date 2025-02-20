<template>
  <link rel="stylesheet" href="./CreateNewPage.scss" />
  <div class="mainPageSiteCreating">
    <div class="mainPageSiteLeftSideBarCreating">
      <div v-if="notes.length === 0" class="no-notes">У вас нет заметок!</div>
      <div v-for="(note, index) in notes" :key="index" class="note-box">
        <h3>{{ note.title }}</h3>
        <p>{{ note.text }}</p>
        <div class="status">
          <span @click="toggleStatus(index)">
            {{ note.completed ? "✅" : "❌" }}
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

<script setup>
import { ref, onMounted } from "vue";

const notes = ref([]);
const title = ref(null);
const text = ref(null);
const editingIndex = ref(null);

onMounted(() => {
  const savedNotes = localStorage.getItem("notes");
  if (savedNotes) {
    notes.value = JSON.parse(savedNotes);
  }
});

const addNote = () => {
  if (title.value.trim() && text.value.trim()) {
    if (editingIndex.value !== null) {
      notes.value[editingIndex.value] = {
        title: title.value,
        text: text.value,
        completed: false,
      };
      editingIndex.value = null;
    } else {
      notes.value.push({
        title: title.value,
        text: text.value,
        completed: false,
      });
    }
    saveNotesToLocalStorage();
    title.value = "";
    text.value = "";
  }
};

const deleteNote = (index) => {
  notes.value.splice(index, 1);
  saveNotesToLocalStorage();
};

const editNote = (index) => {
  title.value = notes.value[index].title;
  text.value = notes.value[index].text;
  editingIndex.value = index;
};

const toggleStatus = (index) => {
  notes.value[index].completed = !notes.value[index].completed;
  saveNotesToLocalStorage();
};

const saveNotesToLocalStorage = () => {
  localStorage.setItem("notes", JSON.stringify(notes.value));
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
    width: max-content;
    font-size: 20px;
    cursor: pointer;
  }
  .delete-button {
    width: max-content;
    background-color: red;
    color: white;
    border: none;
    padding: 5px;
    cursor: pointer;
    margin-top: 5px;
    border-radius: 10px;
  }
  .edit-button {
    width: max-content;
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
    width: max-content;
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
