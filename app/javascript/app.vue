<template>
  <draggable v-model="lists" :options="{group: 'lists'}" class="board dragArea" @end="listMoved">
    <div v-for="(list, index) in lists" class="list">
      <h5>{{ list.name }}</h5>
      <draggable v-model="list.cards" :options="{group: 'cards'}" class="dragArea" @change="cardMoved">
        <div v-for="(card, index) in list.cards" class="card card-body mb-3">
          {{ card.name }}
        </div>
      </draggable>

      <div class="card card-body">
        <textarea v-model="messages[list.id]" class="form-control mb-1"></textarea>
        <button v-on:click="submitMessages(list.id)" class="btn btn-secondary">Add</button>
      </div>
    </div>
  </draggable>
</template>

<script>
import draggable from 'vuedraggable'
export default {

  components: { draggable },

  props: ["original_lists"],

  data: function(){
    return {
      messages: {},
      lists: this.original_lists,
    }
  },

  methods: {
    cardMoved: function(event) {
      const evt = event.added || event.moved
      if(evt == undefined) { return }

      const element = evt.element

      const list_index = this.lists.findIndex((list) => {
        return list.cards.find((card) => {
          return card.id === element.id
        })
      })

      var data = {
        list_id: this.lists[list_index].id,
        position: evt.newIndex + 1
      }

      $.ajax({
        url: `/cards/${element.id}/move`,
        type: "PATCH",
        data: JSON.stringify(data),
        dataType: "json",
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content'),
            'Content-Type': 'application/json'
         }
      })
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
    submitMessages: function(list_id) {
      var data = {
        list_id: list_id,
        name: this.messages[list_id]
      }
      console.log(JSON.stringify(data))
      $.ajax({
        url: "/cards",
        type: "POST",
        data: JSON.stringify(data),
        dataType: "json",
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content'),
            'Content-Type': 'application/json'
         },
        success: (data) => {
          const index = this.lists.findIndex(item => item.id == list_id);
          this.lists[index].cards.push(data);
          this.messages[list_id] = undefined;
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
.dragArea{
  min-height: 10px;
}

.board {
  overflow-x: auto;
  white-space: nowrap;
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
