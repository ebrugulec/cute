<template>
  <div class="list">
    <h5>{{ list.name }}</h5>
    <draggable v-model="list.cards" :options="{group: 'cards'}" class="dragArea" @change="cardMoved">
      <card v-for="card in list.cards" :card="card" :list="list"></card>
    </draggable>

    <a v-if="!editing" v-on:click="startEditing">Add a card</a>
    <textarea ref="message" v-if="editing" v-model="message" class="form-control mb-1"></textarea>
    <button v-if="editing" v-on:click="createCard" class="btn btn-secondary">Add</button>
    <a v-if="editing" v-on:click="editing=false">Cancel</a>

  </div>
</template>

<script>
import draggable from 'vuedraggable'
import card from 'components/card'

export default {

  components: { draggable, card },

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

      const list_index = window.store.state.lists.findIndex((list) => {
        return list.cards.find((card) => {
          return card.id === element.id
        })
      })

      var data = {
        list_id: window.store.state.lists[list_index].id,
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
    createCard: function() {
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
          // this.$store.commit('addCard', data)
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

</style>
