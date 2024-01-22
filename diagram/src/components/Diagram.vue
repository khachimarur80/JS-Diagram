<template>
  <div id="contents">
    <!--<div id="zoom">
      <span style="margin: 10px">Zoom: {{ zoom }} %</span>
    </div>
    <span id="offset">x {{ offset.x }}, y {{ offset.y }}</span>-->
    <canvas 
      id="canvas" 
      :height="canvas.height" 
      :width="canvas.width"
      @mousewheel="changeZoom"
      @mousedown.right.prevent="startDragging"
      @mousemove.right="dragging"
      @mouseup.right="stopDragging"
      @mousedown.left="createNode"
      >

    </canvas>
  </div>
</template>

<script>

class Node {
  constructor() {
    this.x = null
    this.y = null
    this.text = ""
    this.id = Math.floor(Math.random() * 1000000)
  }
}
export default {
  name: 'DiagramView',
  props: {
    treeNode: {
      type: Object,
    }
  },
  data:() => ({
    zoom: 100,
    offset: {
      x: 0,
      y: 0,
    },
    minTileSize: 15, 
    maxTileSize: 60,
    baseTileSize: 0,
    currentTileSize: 0,
    canvas: {
      ctx: null,
      height: 600,
      width: 800,
    },
    isDragging: false,
    dragStart: {
      x: 0,
      y: 0,
    },
    nodes: [],
    edges: [],
  }),
  methods : {
    startDragging(event) {
      this.isDragging = true;
      this.dragStart = { x: event.clientX, y: event.clientY };
      document.addEventListener("mousemove", this.dragging);
      document.addEventListener("mouseup", this.stopDragging);

      document.getElementById('canvas').style.cursor = 'grab'
    },
    dragging(event) {
      if (this.isDragging) {
        const offsetX = event.clientX - this.dragStart.x;
        const offsetY = event.clientY - this.dragStart.y;
        this.offset.x += offsetX;
        this.offset.y += offsetY;
        this.dragStart = { x: event.clientX, y: event.clientY };
      }
    },
    stopDragging() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.dragging);
      document.removeEventListener("mouseup", this.stopDragging);

      document.getElementById('canvas').style.cursor = 'auto'
    },
    changeZoom(event) {
      this.zoom = Math.min(1000, Math.max(10, this.zoom+Math.ceil(event.deltaY*.5)))
    },
    generateTile(x, y, tileSize) {
      this.canvas.ctx.fillStyle = "transparent";
      this.canvas.ctx.strokeStyle = "black";
      this.canvas.ctx.lineWidth = 0.1;

      const halfSize = tileSize / 2;

      this.canvas.ctx.strokeRect(
        Math.floor(x - halfSize + this.canvas.width / 2) + 0.5,
        Math.floor(y - halfSize + this.canvas.height / 2) + 0.5,
        tileSize,
        tileSize
      );
    },
    generateAxis() {
      this.canvas.ctx.beginPath();
      this.canvas.ctx.moveTo(0, this.canvas.height / 2 + this.offset.y);
      this.canvas.ctx.lineTo(this.canvas.width, this.canvas.height / 2 + this.offset.y);
      this.canvas.ctx.strokeStyle = 'black';
      this.canvas.ctx.lineWidth = 2;
      this.canvas.ctx.stroke();

      this.canvas.ctx.beginPath();
      this.canvas.ctx.moveTo(this.canvas.width / 2 + this.offset.x, 0);
      this.canvas.ctx.lineTo(this.canvas.width / 2 + this.offset.x, this.canvas.height);
      this.canvas.ctx.strokeStyle = 'black';
      this.canvas.ctx.lineWidth = 2;
      this.canvas.ctx.stroke();
    },
    generateNodes() {
      for (let i=0; i<this.nodes.length; i++) {
        this.canvas.ctx.fillStyle = "red";
        this.canvas.ctx.strokeStyle = "red";
        this.canvas.ctx.lineWidth = 2;

        this.canvas.ctx.strokeRect(
          (this.nodes[i].x - .5) * this.baseTileSize * this.zoom/100 + this.canvas.width/2 + this.offset.x + 1,
          -(this.nodes[i].y+.5) * this.baseTileSize * this.zoom/100 + this.canvas.height/2 + this.offset.y + 1,
          this.baseTileSize * this.zoom/100 - 1,
          this.baseTileSize * this.zoom/100 - 1
        );
      }
    },
    generateGrid() {
      let currentTileSize = this.baseTileSize

      if (this.zoom > 100) {
        currentTileSize *= this.zoom/100
        while (currentTileSize>this.maxTileSize) {
          currentTileSize = currentTileSize/Math.floor(this.maxTileSize/this.minTileSize)
        }
      }
      else if (this.zoom < 100) {
        currentTileSize *= this.zoom/100
        while (currentTileSize<this.minTileSize) {
          currentTileSize = Math.floor(this.maxTileSize/this.minTileSize)*currentTileSize
        }
      }
      
      this.currentTileSize = currentTileSize

      let rowsNum = Math.ceil((this.canvas.height/2) / currentTileSize)
      let colsNum = Math.ceil((this.canvas.width/2) / currentTileSize)

      for (let i=1; i<=colsNum+1; i++) {
        for (let j=1; j<=rowsNum+1; j++) {
          this.generateTile(
            i*currentTileSize - currentTileSize + this.offset.x%currentTileSize,
            j*currentTileSize - currentTileSize + this.offset.y%currentTileSize,
            currentTileSize
          )
          this.generateTile(
            -(i*currentTileSize) + this.offset.x%currentTileSize,
            -(j*currentTileSize) + this.offset.y%currentTileSize,
            currentTileSize
          )
          this.generateTile(
            -(i*currentTileSize) + this.offset.x%currentTileSize,
            j*currentTileSize - currentTileSize  + this.offset.y%currentTileSize,
            currentTileSize
          )
          this.generateTile(
            (i*currentTileSize - currentTileSize) + this.offset.x%currentTileSize,
            -(j*currentTileSize) + this.offset.y%currentTileSize,
            currentTileSize
          )
        }
      }
    },
    generateBoard() {
      const canvas = document.getElementById("canvas")
      this.canvas.ctx = canvas.getContext("2d")
      this.canvas.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);

      this.generateGrid()
      //this.generateAxis()
      this.generateNodes()
    },
    createNode(event) {
      let x = Math.floor((event.x  - this.canvas.width/2 + this.currentTileSize/2) / this.currentTileSize)
      let y = Math.floor((this.canvas.height/2 - event.y) / this.currentTileSize)

      let node = new Node()

      node.x = x
      node.y = y
      this.nodes.push(node)

      this.canvas.ctx.fillStyle = "red";
      this.canvas.ctx.strokeStyle = "red";
      this.canvas.ctx.lineWidth = 1;

      this.canvas.ctx.strokeRect(
        (x-1) * this.baseTileSize * this.zoom/100 + this.canvas.width/2 + this.offset.x + 1,
        -(y+1) * this.baseTileSize * this.zoom/100 + this.canvas.height/2 + this.offset.y + 1,
        this.baseTileSize * this.zoom/100 - 1,
        this.baseTileSize * this.zoom/100 - 1
      );

      console.log(x,y)
    }
  },
  mounted() {
    if (this.treeNode) {
      this.nodes = this.treeNode.nodes
      this.edges = this.treeNode.edges



    }
    this.baseTileSize = Math.ceil((this.minTileSize+this.maxTileSize)/2)
    this.generateBoard()
  },
  watch: {
    zoom() {
      this.generateBoard()
    },
    offset: {
      handler() {
        this.generateBoard()
      },
      deep: true,
    },
    nodes: {
      handler() {
        this.generateBoard()
      },
      deep: true,
    }
  }
}
</script>

<style scoped>
#contents {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100h;
}
#canvas {
  border: 1px solid rgb(171, 171, 171);
  overflow: visible;
  display: block;
}
#zoom {
  position: absolute;
  top: 30px;
  transform: translateX(-100px);
}
#offset {
  position: absolute;
  top: 30px;
  transform: translateX(100px);
}
</style>
