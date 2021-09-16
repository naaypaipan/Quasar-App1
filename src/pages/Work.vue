<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-white">
    
      <q-input 
      v-model="newTasks" 
      @keyup.enter="addTask"
      filled 
      bg-color="grey-1"
      class="col"
      
      label="Add Work" 
      dense>
        <template v-slot:before>
          <q-icon name="event" />
        </template>

        <template v-slot:hint>
          Field hint
        </template>

        <template v-slot:append>
          <q-btn 
          @click="addTask"
          round dense flat icon="add" />
        </template>
      </q-input>
    </div>
   <q-list 
   class="bg-white"
   separator
   bordered>
     <q-item 
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{'done bg-grey-5':task.done}"
        clickable
        v-ripple>
        <q-item-section avatar>
          <q-checkbox 
          v-model="task.done" 
          class="no-pointer-events"
          color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
          <q-item-section
          v-if="task.done"
          side >
          <q-btn @click.stop="deleteTask(index)" 
          push  
          dense 
          color="primary" 
          round icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div 
    v-if="!tasks.length"
    class="no-tasks absolute-center">
      
      <div class="text-h5 text-primary text-center">You don't have a job to do</div>
    </div>
  </q-page>
</template>

<script> 
import { defineComponent } from 'vue';

export default{
  data(){
    return{
      newTasks: '',
      tasks: [
        /*{
          title: 'sss',
          done: false
        },
         {
          title: 'ddd',
          done: false
        },
         {
          title: 'fff',
          done: false
        }*/
      ]
    }
  },
  methods:{
    deleteTask(index){
      this.$q.dialog({
        title: 'Delete',
        message: 'Are you sure',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index,1)
      })
    },
    addTask(){
      this.tasks.push({
        title: this.newTasks,
        done: false
      })
      this.newTasks = ''
    }
  }

}
</script>

<style lang="scss">
  .done{
    .q-item__label{
      text-decoration: line-through ;
      color: rgb(245, 9, 9);
    }
  }
</style>
