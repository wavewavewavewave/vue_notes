<template>
  <!--  header-->
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <!--          <h1 class="title">{{ title }}</h1>-->
          <h1 class="title">Notes App</h1>

          <!-- предупреждение -->
          <Message v-if="message" :message="message"/>

          <!--          <new-note-creator :note="note" @addNote="addNewNote"/>-->
          <NewNoteCreator :note="note"/>

          <!-- режим отображения и поиск -->

          <div class="note-header">
            <h1>Created Notes</h1>

            <Search :value="search" placeholder="Find a note" @search="search = $event"/>

            <!--            <div class="icons" v-if="!isMobile()">-->
            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg"
                   width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                   stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg"
                   width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                   stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
          </div>

          <!-- созданные заметки -->
          <Notes :notes="getFilteredNotes" @remove="removeNote"/>
        </div>
      </section>
    </div>
  </div>
  <!--  footer-->
</template>

<script>

import Message from "@/components/Massage";
import NewNoteCreator from "@/components/NewNoteCreator";
import Search from "@/components/Search";
import Notes from "@/components/Notes";

export default {
  name: 'App',
  components: {
    Notes,
    Search,
    NewNoteCreator,
    Message
  },
  data() {
    return {
      uniqueId: 2,
      title: 'Notes App',
      message: null,
      grid: true,
      search: '',
      note: {
        id: null,
        title: '',
        description: '',
        priorityLevel: '',
        titleEditModeEnabled: false,
        descriptionEditModeEnabled: false
      },
      notes: [
        {
          id: 0,
          title: 'Welcome Note',
          description: 'Start Creating Notes Now!',
          date: new Date(Date.now()).toLocaleString(),
          priorityLevel: -1,
          titleEditModeEnabled: false,
          descriptionEditModeEnabled: false
        },
        {
          id: 1,
          title: 'Shopping List',
          description: 'Milk, coffee, cookies',
          date: new Date(Date.now()).toLocaleString(),
          priorityLevel: 1,
          titleEditModeEnabled: false,
          descriptionEditModeEnabled: false
        },
        {
          id: 2,
          title: 'TODO List',
          description: 'Clean house, cook dinner',
          date: new Date(Date.now()).toLocaleString(),
          priorityLevel: 0,
          titleEditModeEnabled: false,
          descriptionEditModeEnabled: false
        }
      ]
    }
  },

  computed: {
    getFilteredNotes() {
      let notesToShow = this.notes,
          notesToSearch = this.search;
      if (!notesToSearch) return sortByPriority(notesToShow);

      notesToSearch = notesToSearch.trim().toLowerCase();
      notesToShow = notesToShow.filter(function (item) {
        if (item.title.toLowerCase().indexOf(notesToSearch) !== -1
            || item.description.toLowerCase().indexOf(notesToSearch) !== -1) {
          return item;
        }
      });

      return sortByPriority(notesToShow);

      // сортировка по приоритету (от высокого к низкому: от красного к синему)
      function sortByPriority(notes) {
        return notes.sort((firstElement, secondElement) => secondElement.priorityLevel - firstElement.priorityLevel);
      }
    },
  },

  methods: {
    addNewNote() {
      let {title, description, priorityLevel} = this.note;
      if (title === '') {
        this.message = "Title can't be blank";
        return false;
      }
      this.notes.push({
        id: this.getUniqueId(),
        title,
        description,
        date: new Date(Date.now()).toLocaleString(),
        priorityLevel
      });
      this.note.id = null;
      this.note.title = "";
      this.note.description = "";
      this.note.priorityLevel = "";
      this.note.titleEditModeEnabled = false;
      this.note.descriptionEditModeEnabled = false;
      this.message = null;
    },

    removeNote(noteId) {
      let allNotes = this.notes;
      for (let index = 0; index < allNotes.length; index++) {
        if (allNotes[index].id === noteId)
          allNotes.splice(index, 1);
      }
    },

    getUniqueId() {
      return this.uniqueId += 1;
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  font-family: 'Arial';
  font-size: 18px;
  font-weight: 400;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
  background-color: #f7f7f7;
}

*, *:before, *:after {
  box-sizing: border-box;
}

h1, h2, h3, h4, h5, h6 {
  margin: 0;
}

input, textarea, select, button {
  border: 1px solid #DCDFE6;
}

a {
  text-decoration: none;
}

ul, li {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  margin: 0 10%;
}


.wrapper-content {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}


.wrapper-content.wrapper-content–fixed {
  margin-top: 80px;
}

.container {
  width: 100%;
  flex: 0 0 auto;
  position: relative;
  max-width: 100%;
  margin: 0 auto;
}

.note-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.note-header p {
  font-weight: bold;
  color: #494ce8;
}

.note-header svg {
  margin-right: 12px;
  color: grey;
  cursor: pointer;
}

.note-header svg.active {
  color: #494ce8;
}

.note-header svg:last-child {
  margin-right: 0;
}
</style>
