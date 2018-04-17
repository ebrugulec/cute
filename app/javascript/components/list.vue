<template>
  <div class="list">
    <h5>{{ list.name }}</h5>
    <draggable v-model="list.cards" :options="{group: 'cards'}" class="dragArea" @change="cardMoved">
      <div v-for="(card, index) in list.cards" class="card card-body mb-3">
        {{ card.name }}
      </div>
    </draggable>

    <a v-if="!editing" v-on:click="startEditing">Add a card</a>
    <textarea ref="message" v-if="editing" v-model="message" class="form-control mb-1"></textarea>
    <button v-if="editing" v-on:click="submitMessage" class="btn btn-secondary">Add</button>
    <a v-if="editing" v-on:click="editing=false">Cancel</a>

  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {

  components: { draggable },

  props: ["list"],

  data: function(){
    return {
      message: "",
      editing: false,
    }
  },

  methods: {
    startEditing: function(){
      this.editing = true
      this.$nextTick(() => { this.$refs.message.focus() })
    },

    cardMoved: function(event) {
      const evt = event.added || event.moved
      if(evt == undefined) { return }

      const element = evt.element

      const list_index = window.store.lists.findIndex((list) => {
        return list.cards.find((card) => {
          return card.id === element.id
        })
      })

      var data = {
        list_id: window.store.lists[list_index].id,
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
    submitMessage: function() {
      var data = {
        list_id: this.list.id,
        name: this.message
      }
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
          const index = window.store.lists.findIndex(item => item.id == this.list.id);
          window.store.lists[index].cards.push(data);
          this.message = undefined;
          this.$nextTick(() => { this.$refs.message.focus() })
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
