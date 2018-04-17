<template>
  <draggable v-model="lists" :options="{group: 'lists'}" class="board dragArea" @end="listMoved">
    <list v-for="(list, index) in lists" :list="list"></list>
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
    }
  },

  methods: {
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

  }
}
</script>

<style scoped>
.board {
  overflow-x: auto;
  white-space: nowrap;
}
</style>
