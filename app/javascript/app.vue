<template>
  <draggable v-model="lists" :options="{group: 'lists'}" class="board dragArea" @end="listMoved">
    <list v-for="(list, index) in lists" :list="list"></list>

    <div class="list">
      <a v-if="!editing" v-on:click="startEditing">Add a List</a>
      <textarea ref="message" v-if="editing" v-model="message" class="form-control mb-1"></textarea>
      <button v-if="editing" v-on:click="submitMessage" class="btn btn-secondary">Add</button>
      <a v-if="editing" v-on:click="editing=false">Cancel</a>
    </div>
  </draggable>
</template>

<script>
import draggable from 'vuedraggable'
import list from 'components/list.vue'

export default {

  components: { draggable, list },

  props: ["original_lists"],

  data: function(){
    return {
      lists: this.original_lists,
      editing: false,
      message: ""
    }
  },

  methods: {
    startEditing: function(){
      this.editing = true
      this.$nextTick(() => { this.$refs.message.focus() })
    },

    listMoved: function(event) {
      var data = {
        position: event.newIndex + 1
      }

      $.ajax({
        url: `lists/${this.lists[event.newIndex].id}/move`,
        type: "PATCH",
        data: JSON.stringify(data),
        dataType: "json",
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content'),
            'Content-Type': 'application/json'
         }
      })
    },
    submitMessage: function() {
      var data = {
        name: this.message
      }
      $.ajax({
        url: "/lists",
        type: "POST",
        data: JSON.stringify(data),
        dataType: "json",
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content'),
            'Content-Type': 'application/json'
         },
        success: (data) => {
          window.store.lists.push(data);
          this.message = ""
          this.editing = false
        },
        error: function(xhr, textStatus, errorThrown){
           console.log('request failed');
        }
      })
    }
  }
}
</script>

<style scoped>
.board {
  overflow-x: auto;
  white-space: nowrap;
}

.dragArea{
  min-height: 10px;
}

.list {
  background: #E2E4E6;
  border-radius: 3px;
  padding: 20px;
  display: inline-block;
  margin-right: 20px;
  width: 270px;
  vertical-align: top;
}
</style>
