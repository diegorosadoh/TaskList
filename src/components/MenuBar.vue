<template lang="html">
  <p id="selector">
    <span id="cont">{{total}} items, {{active}} left</span>

    <span id="filter">
      <span 
        :class="{ filter_active : view_state=='all' }"
        @click="filter('all')"
      >
        All
      </span>

      <span 
        :class="{ filter_active : view_state=='active' }"
        @click="filter('active')"
      >
        Active
      </span>

      <span 
        :class="{ filter_active : view_state=='completed' }"
        @click="filter('completed')"
      >
        Completed
      </span>
    </span>

    <span id="clear" 
      @click="clean"
    >
      Clear Completed
    </span>
  </p>
  
  <input id="search" type="text" placeholder="Search"
    v-model="regex"
    @input="search"
  >
</template>

<script>
  export default  {
    name: 'menu-bar',
    props: ['total','active','view_state'],
    emits: ['filter','clean','search'],

    data () {
      return { regex:"" }
    },

    methods: {
      filter(option){ this.$emit('filter', option); },
      clean(){ this.$emit('clean'); },
      search(){ this.$emit('search',this.regex); }
    }
}
</script>

<style scoped>
  #selector{
    margin:0;
    margin-top:auto;

    display:flex;
    justify-content: space-around;
    
    color:var(--primary_mod);
    border-top:1px solid lightgrey;
    border-bottom:1px solid lightgrey;
    padding:20px 0;
  }

  #filter,#clear{
      cursor:pointer;
  }

  #clear:hover{
      color:var(--secondary_mod)
  }

  #filter span:not(.filter_active):hover{
      color:black;
  }

  .filter_active{
      color: var(--secondary);
  }

  #clear,#cont{
      font-weight: 300;
      font-size: 0.8em;
  }

  #filter>span{
      font-weight: 500;
      margin: 0 10px;
  }

  #search{
    border:none;
    outline:none;
    box-shadow: none;
    padding:20px;
    font-size: 1em;
    text-align: center;
    border-radius:20px;
  }
</style>
