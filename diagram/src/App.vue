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
        nodes: [{ id: 'A' }, { id: 'B' }, { id: 'C' }],
        edges: [
          ['A' , 'B'],
          ['A' , 'C']
        ]
      },
      {
        nodes: [{ id: 'D' }, { id: 'E' }],
        edges: [
          ['D', 'E' ]
        ]
      },
      {
        nodes: [
          { id: 'A' }, { id: 'B' }, { id: 'C' }, { id: 'D' }, { id: 'E' },
          { id: 'F' }, { id: 'G' }, { id: 'H' }, { id: 'I' }, { id: 'J' }, { id: 'K' }
        ],
        edges: [
          ['A', 'B'], ['A', 'C'], ['B', 'D'], ['C', 'E'],
          ['D', 'F'], ['D', 'G'], ['E', 'H'], ['F', 'I'],
          ['G', 'J'], ['H', 'K'], ['I', 'J'], ['J', 'K'],
          ['F', 'G'], ['G', 'H'], ['H', 'I']
        ]
      },
      {
        nodes: [
          { id: 'A' }, { id: 'B' }, { id: 'C' }, { id: 'D' }, { id: 'E' }, { id: 'F' }, { id: 'G' }, { id: 'H' }, { id: 'I' }, { id: 'J' }, { id: 'K' }
        ],
        edges: [
          ['A', 'B'], ['A', 'C'], ['A', 'D'], ['A', 'E'], ['B', 'F'], ['F', 'G'], ['G', 'H'], ['H', 'I'], ['I', 'J'], ['C', 'K'], 
        ]
      },
      {
        nodes: [
          { id: 'X' }, { id: 'Y' }, { id: 'Z' }, { id: 'W' }, { id: 'V' },
          { id: 'U' }, { id: 'T' }, { id: 'S' }, { id: 'R' }, { id: 'Q' },
          { id: 'P' }, { id: 'O' }, { id: 'N' }, { id: 'M' }, { id: 'L' },
          { id: 'K' }, { id: 'J' }, { id: 'I' }, { id: 'H' }, { id: 'G' },
          { id: 'F' }, { id: 'E' }, { id: 'D' }, { id: 'C' }, { id: 'B' },
          { id: 'A' }
        ],
        edges: [
          ['A', 'B'], ['A', 'C'], ['B', 'D'], ['C', 'E'],
          ['D', 'F'], ['D', 'G'], ['E', 'H'], ['F', 'I'],
          ['G', 'J'], ['H', 'K'], ['I', 'J'], ['J', 'K'],
          ['K', 'L'], ['L', 'M'], ['M', 'N'], ['N', 'O'],
          ['O', 'P'], ['P', 'Q'], ['Q', 'R'], ['R', 'S'],
          ['S', 'T'], ['T', 'U'], ['U', 'V'], ['V', 'W'],
          ['W', 'X'], ['X', 'Y'], ['Y', 'Z'], ['Z', 'X'],
          ['Z', 'W'], ['W', 'U'], ['U', 'Q'], ['Q', 'L'],
          ['L', 'G'], ['G', 'D'], ['D', 'A']
        ]
      },
      {
        nodes: [
          { id: 'S' },
          { id: 'Q' },
          { id: 'V' },
          { id: 'C' },
          { id: 'N' },
          { id: 'E' }
        ],
        edges: [
          ['S', 'Q'],
          ['Q', 'V'],
          ['Q', 'C'],
          ['C', 'N'],
          ['N', 'V'],
          ['C', 'V'],
          ['V', 'E']
        ]
      },
      {
        nodes: [
          { id: 'A' }, { id: 'B' }, { id: 'C' }, { id: 'D' },
          { id: 'E' }, { id: 'F' }, { id: 'G' }, { id: 'H' }
        ],
        edges: [
          ['A', 'B'], ['A', 'C'], ['B', 'D'], ['C', 'E'],
          ['D', 'F'], ['E', 'G'], ['F', 'H'], ['G', 'H']
        ]
      },
      {
        nodes: [
          { id: 'X' }, { id: 'Y' }, { id: 'Z' }, { id: 'W' },
          { id: 'V' }, { id: 'U' }, { id: 'T' }, { id: 'S' }
        ],
        edges: [
          ['X', 'Y'], ['Y', 'Z'], ['Z', 'W'], ['W', 'V'],
          ['V', 'U'], ['U', 'T'], ['T', 'S'], ['S', 'X']
        ]
      },
      {
        nodes: [
          { id: 'M' }, { id: 'N' }, { id: 'O' }, { id: 'P' },
          { id: 'Q' }, { id: 'R' }, { id: 'S' }, { id: 'T' }
        ],
        edges: [
          ['M', 'N'], ['N', 'O'], ['O', 'P'], ['P', 'Q'],
          ['Q', 'R'], ['R', 'S'], ['S', 'T'], ['T', 'M']
        ]
      },
      {
        nodes: [
          { id: 'I' }, { id: 'J' }, { id: 'K' }, { id: 'L' },
          { id: 'M' }, { id: 'N' }, { id: 'O' }, { id: 'P' }
        ],
        edges: [
          ['I', 'J'], ['J', 'K'], ['K', 'L'], ['L', 'M'],
          ['M', 'N'], ['N', 'O'], ['O', 'P'], ['P', 'I']
        ]
      }
    ]

  }),
 
  mounted() {
    //let iteration = Math.floor(Math.random() * this.options.length)
    this.treeNode = this.options[5]
    /*setInterval(()=>{
      iteration = Math.floor(Math.random() * this.options.length)
      this.treeNode = this.options[iteration]
    }, 1000)*/
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