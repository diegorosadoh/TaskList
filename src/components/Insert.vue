<template lang="html">
  <div class="insert">
    <input class="insert" type="text" autofocus
      v-model="newTask" 
      :placeholder="errorTask"
      @keyup.enter="addTask" 
    >
  </div>
</template>

<script>

  export default  {
    name: 'insert',
    emits: ['new'],

    data () {
      return {
        newTask:"",
        errorTask: "New task"
      }
    },

    methods: {
      addTask(){
        if( this.newTask!="" ){
          this.$emit('new',this.newTask);
          this.errorTask = "New task";
          this.newTask = "";
        }else this.errorTask = "Enter a new task";
      }
    }
}
</script>

<style scoped>
  .insert{
      display:flex;
      flex-flow:column;
      align-items: center;
      height:40px;
  }

  input{
    box-sizing: border-box;
    height: 100%;
    width:100%;
    padding: 0 30px;
    font-size: 1.2em;
    border:none;
    border-radius: 20px;
    outline: none;
  }

  input:focus{
      box-shadow: 0 0 1px 4px var(--secondary);
  }
</style>
