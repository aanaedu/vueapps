<template>
  <div id="app">
    <h1>{{ title }}</h1>
        <div class="row">
            <div class="col-md-12">
                <form role="form" v-on:submit.prevent="onSubmit">
                    <legend>Add a New Note</legend>
        
                    <div class="form-group">
                        <label for="noteTitle">Enter Title</label>
                        <input type="text" class="form-control" id="noteTitle" placeholder="Title" v-model="note.title" required>
                    </div>
        
                    <div class="form-group">
                        <label for="noteDescription">Enter Description</label>
                        <textarea name="" id="noteDescription" placeholder="Description" class="form-control" v-model="note.description" required></textarea>
                    </div>

                    <div class="form-group">
                        <label for="noteColor">Select Color</label>
                        <select id="noteColor" class="form-control" required="required" v-model="note.color">
                            <option value="default">Default</option>
                            <option value="blue">Blue</option>
                            <option value="lightBlue">Light Blue</option>
                            <option value="lightGreen">Light Green</option>
                            <option value="red">Red</option>
                            <option value="yellow">Yellow</option>
                        </select>
                        
                    </div>
                    <button type="submit" class="btn btn-default">Reset</button>
                    <button type="submit" class="btn btn-success">{{ isEditFormMode ? 'Update' : 'Add Note' }}</button>
                </form>
            </div>
        </div>

        <div class="row">
            <h3>Notes</h3>
            <div class="col-md-3"  v-for="(note, index) of orderedNotes">
                <div class="panel" :class="getNoteColor(note.color)">
                    <div class="panel-heading">
                        {{ note.title }} <a @click="editNote(note,index)" class="text-muted" role="button" title="Edit"><span class="glyphicon glyphicon-pencil"></span></a>
                        <button type="button" class="close" data-dismiss="alert" aria-label="Close" v-on:click="onRemoveNote(index)"><span aria-hidden="true">&times;</span></button>
                    </div>
                    <div class="panel-body">
                        <div>{{ note.createdOn }}</div>
                        {{ note.description}}
                    </div>
                </div>
            </div>
        </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        title: 'Notetaker',
        isEditFormMode: false,
        note: {
          title: '',
          description: '',
          color: 'default'
        },
        panelColor: {
          default: 'panel-default',
          blue: 'panel-primary',
          lightBlue: 'panel-info',
          lightGreen: 'panel-success',
          red: 'panel-danger',
          yellow: 'panel-warning'
        },
        notes: [{
          title: 'What a game!',
          description: 'ManCity is still the team to beat.',
          createdOn: new Date().toLocaleString(),
          color: 'default'
        }]
      };
    },
    computed: {
        orderedNotes: function() {
          return this.notes.sort((a, b) => a.createdOn > b.createdOn ? -1 : 1);
        }
    },
    methods: {
        onSubmit(e) {
          e.preventDefault();
          
          if (!this.note.title.trim() && !this.note.description.trim()) return;
          
          const noteIndex = this.notes.indexOf(this.note);
          
          if (noteIndex >= 0) {
            this.notes[noteIndex] = this.note;
            this.isEditFormMode = false;
          } else {
            this.notes.push({
              title: this.note.title,
              description: this.note.description,
              color: this.note.color,
              createdOn: new Date().toLocaleString()
            });
          }
          this.resetNote();
        },
        onRemoveNote: function(index) {
          if (confirm('Are you sure you want to remove note?')) {
            this.notes.splice(index, 1);
          }
        },
        getNoteColor: function(color) {
          return this.panelColor[color];
        },
        editNote: function(note, index) {
          this.note = note;
          this.isEditFormMode = true;
        },
        resetNote: function() {
          this.isEditFormMode = false;
          this.note = {
            title: '',
            description: '',
            color: 'default'
          };
        }
    }
  };
</script>

<style>
</style>
