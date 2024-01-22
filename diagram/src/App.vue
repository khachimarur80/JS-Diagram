<template>
  <Diagram :treeNode="treeNode"/>
</template>

<script>
import Diagram from './components/Diagram';

export default {
  name: 'App',

  components: {
    Diagram,
  },

  data: () => ({
    treeNode: {
      nodes: [],
      edges: []
    },
    options: [
      {
        nodes: ['A', 'B', 'C'],
        edges: [
          ['A', 'B'],
          ['A', 'C']
        ]
      },
      {
        nodes: ['D', 'E'],
        edges: [
          ['D', 'E']
        ]
      },
      {
        nodes: ['F', 'G', 'H', 'I', 'J'],
        edges: [
          ['F', 'G'],
          ['F', 'H'],
          ['H', 'I'],
          ['H', 'J'],
          ['J', 'G'],
        ]
      },
    ]
  }),
 
  mounted() {
    let iteration = 0
    this.treeNode = this.options[iteration]

    setInterval(()=>{
      iteration += 1
      this.treeNode = this.options[iteration%3]
    }, 1000)
  }
};
</script>
<style>
  body, html {
    margin: 0px;
    padding: 0px;
    overflow: hidden;
  }
  ::-webkit-scrollbar {
    display: none;
  }
</style>