<template>
  <insert 
    @new="addTask"
  />

  <div class="list">
    <list 
      :view = "view" 
      @check = "checkTask" 
      @hover = "clock" 
      @priority = "priority" 
      @delete = "delTask"
    />

    <menu-bar 
      :total = "tasks.length" 
      :active = "active" 
      :view_state = "view_state" 
      @filter = "filter" 
      @clean = "clean" 
      @search = "search"
    />
  </div>
</template>

<script>
import Insert from './components/Insert.vue';
import List from './components/List.vue';
import MenuBar from './components/MenuBar.vue';

export default {
  name: 'App',

  components: {
    Insert,
    List,
    MenuBar
  },

  data(){
    return{
      tasks: (localStorage.list) ? JSON.parse(localStorage.list) : [],
      lastID: -1,
      view_state: 'all',
      regex: ''
    }
  },

  created() {
    if (this.tasks.length > 0)
      this.lastID = this.tasks[this.tasks.length-1].id;
  },

  methods:{
    addTask(newTask){
      this.tasks.push({
          id: ++this.lastID,
          text: newTask,
          date: new Date(),
          minutes: 0,
          priority: -1,
          state: false
      });
      this.update();
    },

    checkTask(id){
      let task = this.tasks.find(task => task.id == id);
      task.state = !task.state;
      this.update();
    },

    delTask(id){
      this.tasks = this.tasks.filter(task => task.id != id);
      this.updateID();
    },

    clean(){
      this.tasks = this.tasks.filter(task => task.state == false);
      this.updateID();
    },

    search(regex){ this.regex = regex; },

    filter(option){
      switch (option){
        case 'all':
          this.view_state = 'all';
          break;
        case 'active':
          this.view_state = 'active';
          break;
        case 'completed':
          this.view_state = 'completed';
          break;
        default:
          this.view_state = 'all';
      }
    },

    sort(){
      this.tasks.sort((a,b) => {
          if (a.priority > b.priority) return -1;
          else if (a.priority < b.priority) return 1;
          else return 0;
      });
    },

    clock(id){
      let task = this.tasks.find(task => task.id == id);
      let now = new Date();
      let list = new Date(task.date);
      task.minutes = Math.floor((now - list) / 60000);
    },

    priority(id, option){
      let task = this.tasks.find(task => task.id == id);
      if (option == 'up' && task.priority < 1)
        task.priority++;
      else if (option == 'down' && task.priority > -1)
        task.priority--;
      this.sort();
    },

    updateID(){
      this.lastID = -1;
      for (let task of this.tasks) task.id = ++this.lastID
      this.update();
    },

    update(){
      localStorage.setItem("list",JSON.stringify(this.tasks));
      if (this.tasks.length > 0) this.lastID = this.tasks[this.tasks.length-1].id;
      this.filter(this.view_state);
    }
  },
  computed:{
    active(){
      return this.tasks.filter(task => task.state == false).length;
    },
    
    view(){
      let result = [];
      switch (this.view_state){
          case 'all':
              result = this.tasks;
              break;
          case 'active':
              result = this.tasks.filter(task => task.state == false);
              break;
          case 'completed':
              result = this.tasks.filter(task => task.state == true);
              break;
          default:
              result = this.tasks;
      }

      if(this.regex !== "")
          result = result.filter(task => task.text.includes(this.regex));

      return result;
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

  :root{
    --primary:rgb(211, 211, 211);
    --secondary:rgb(35, 37, 145);
    --tertiary:rgb(240, 240, 240);

    --primary_mod:rgb(148, 148, 148);
    --secondary_mod:rgb(110, 111, 165);
  }

  body{
    background:url("./assets/background.jpg");
    background-size:100vw 50vh;
    background-repeat: no-repeat;
    background-color:var(--primary);
    color:black;
    font-family: 'Montserrat', Avenir, Helvetica, Arial, sans-serif;
  }

  #app{
    display:flex;
    flex-flow:column;
    justify-content: center;
    align-items: center;
  }

  #app>*{
    background-color:white;
    box-shadow: 0 0 50px black;

    border-radius:20px;
    margin:30px;
    width:40%;
  }

  h1{
    background:none;
    text-align: center;
    font-weight: 600;
    font-size:4em;
    margin-bottom:0;
  }

  .list{
    min-height: 60vh;
    display:flex;
    flex-flow: column;
  }
</style>
