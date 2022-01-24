<template lang="html">
  <ul><transition-group name="list">
    <li class="item"
      v-for="(task) in view" 
      :key="task.id" 
      @click="checkTask(task.id)" 
      @mouseover="hover(task.id)"
    >
      <div id="priority">
        <img src="../assets/prior-up.png" 
          @click.stop="priority(task.id,'up')"
        >
        <img src="../assets/prior-down.png"
          @click.stop="priority(task.id,'down')"
        >
      </div>

      <img src="../assets/icon-complete.png" 
        v-if="task.state" 
      >
      <img src="../assets/icon-active.png" 
        v-else
      >

      <div 
        :class="{complete:task.state}"
      >
        {{task.text}}
        <div id="minutes">&nbsp; {{task.minutes}} minutes ago</div>
      </div>

      <img id="close" src="../assets/close.png"
        @click.stop="delTask(task.id)"
      >
    </li></transition-group>
  </ul>
</template>

<script>
  export default  {
    name: 'list',
    props: ['view'],
    emits: ['check', 'hover', 'priority', 'delete'],

    methods: {
      checkTask(id){ this.$emit('check', id);},
      delTask(id){ this.$emit('delete', id); },
      hover(id){ this.$emit('hover', id); },
      priority(id,option){ this.$emit('priority', id, option); }
    }
  }
</script>

<style scoped>
  ul{
      padding:0;
      margin:0;
      border-radius:20px;
  }

  .item{
      cursor:pointer;

      color:black;

      height:40px;
      padding:10px;
      border-bottom:1px solid lightgrey;
      
      display:flex;
      align-items: center;
  }

  .item:hover{
      background-color: var(--tertiary);
  }

  .item>img{
      height:100%;
      padding-right: 10px;
  }

  .item:nth-child(1){
      border-top-left-radius: 20px;
      border-top-right-radius: 20px;
  }

  .item:last-of-type{
      border:none;
  }

  .complete{
      color:var(--primary_mod);
      text-decoration: line-through;
  }

  #minutes{
      color:var(--primary_mod);
      font-size: 0.8em;
      text-decoration: none;
      display:inline-block;
      visibility: hidden;
  }

  .item:hover #minutes{
      visibility: visible;
  }

  #priority{
      display: flex;
      flex-flow: column;
      height:100%;
      padding-right: 10px;
      display: none;
  }

  #priority>img{
      height:50%;
      box-sizing: border-box;
  }

  #priority>img:hover{
      background:white;
      border-radius: 50%;
      padding:5%;
  }

  .item:hover #priority{
      display: inherit;
  }

  #close{
      visibility: hidden;
      margin-left: auto;
  }

  .item:hover #close{
      visibility: visible;
  }

  .list-move{
    transition: transform 1s;
  }

  .list-enter-active,
  .list-leave-active{
    transition: all 1s linear;
  }

  .list-enter-from{
    opacity: 0;
    margin-left:-20px;
  }

  .list-leave-to{
    color:white;
    opacity:0;
    margin-top: -20px;
  }
</style>
