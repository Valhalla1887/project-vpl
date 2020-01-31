<template>
  <q-page>
    <Documents  @mousedown.native="moveStart($event, 'Documents', i-1)" @mouseup.native="moveEnd($event, 'Documents', i-1)" @mousemove.native="moveActive($event, 'Documents', i-1)"
    class="movable" :ref="'Documents'+(i-1)" :id="'Doc'+i" v-on:connect="connect"
     :style="{'left': positionsDocuments[i-1][0] + 'px', 'top': positionsDocuments[i-1][1] + 'px'}" v-for="i in positionsDocuments.length" :key="'Documents'+i"></Documents>
    <Saturation  @mousedown.native="moveStart($event, 'Saturation', i-1)" @mouseup.native="moveEnd($event,'Saturation', i-1)" @mousemove.native="moveActive($event, 'Saturation', i-1)"
      class="movable" :ref="'Saturation'+(i-1)"
     :style="{'left': positionsSaturation[i-1][0] + 'px', 'top': positionsSaturation[i-1][1] + 'px'}" v-for="i in positionsSaturation.length" :key="'Saturation'+i"></Saturation>
     <OCR  @mousedown.native="moveStart($event, 'OCR', i-1)" @mouseup.native="moveEnd($event,'OCR', i-1)" @mousemove.native="moveActive($event, 'OCR', i-1)"
      class="movable" :ref="'OCR'+(i-1)"
     :style="{'left': positionsOCR[i-1][0] + 'px', 'top': positionsOCR[i-1][1] + 'px'}" v-for="i in positionsOCR.length" :key="'OCR'+i"></OCR>
     <SaveDoc  @mousedown.native="moveStart($event, 'SaveDoc', i-1)" @mouseup.native="moveEnd($event,'SaveDoc', i-1)" @mousemove.native="moveActive($event, 'SaveDoc', i-1)"
      class="movable" :ref="'SaveDoc'+(i-1)"
     :style="{'left': positionsSaveDoc[i-1][0] + 'px', 'top': positionsSaveDoc[i-1][1] + 'px'}" v-for="i in positionsSaveDoc.length" :key="'SaveDoc'+i"></SaveDoc>
     <q-btn @click="addItem('Documents')">Add Documents block</q-btn>
     <q-btn @click="addItem('Saturation')">Add Saturation block</q-btn>
     <q-btn @click="addItem('OCR')">Add OCR block</q-btn>
     <q-btn @click="addItem('SaveDoc')">Add SaveDoc block</q-btn>
     <hr/>
     <q-btn color="red" @click="clearAll()">Clear All</q-btn>
     <q-btn color="orange" @click="connect('Doc1', 'Doc2')">Test Connection</q-btn>
     <hr/>

  </q-page>
</template>

<style>
.movable {
  display: inline-block;
  padding: 0px;
  position: absolute;
}
</style>

<script>
import Documents from '../components/Documents.vue'
import Saturation from '../components/Saturation.vue'
import OCR from '../components/OCR.vue'
import SaveDoc from '../components/SaveDoc.vue'
import { TouchPan } from 'quasar'
import { jsPlumb } from 'jsplumb' // '../../node_modules/jsplumb/dist/js/jsplumb.js'
export default {
  name: 'PageIndex',
  components: {
    Documents,
    Saturation,
    OCR,
    SaveDoc
  },
  directives: {
    TouchPan
  },
  data () {
    return {
      moving: false,
      // Separate arrays for tracking different elements
      // Any addition to the array will create a new element to the screen
      positionsDocuments: [],
      positionsSaturation: [],
      positionsOCR: [],
      positionsSaveDoc: []
    }
  },
  methods: {
    jsPlumbInit: function (i1, i2) {
      // ready is not a function?
      jsPlumb.ready(function (i1, i2) {
        var firstInstance = jsPlumb.getInstance()
        firstInstance.importDefaults({
          Connector: [ 'Bezier', { curviness: 150 } ],
          Anchors: [ 'TopCenter', 'BottomCenter' ]
        })
        jsPlumb.connect({ source: 'i1', target: 'i2' })
        // firstInstance.addEndpoint(i1)
        // firstInstance.addEndpoint(i2)
      })
    },
    toggleItem: function () {
      this.show = !this.show
    },
    // Clear all Blocks, empty all arrays
    clearAll: function () {
      this.positionsDocuments.splice(0, this.positionsDocuments.length)
      this.positionsSaturation.splice(0, this.positionsSaturation.length)
      this.positionsOCR.splice(0, this.positionsOCR.length)
      this.positionsSaveDoc.splice(0, this.positionsSaveDoc.length)
    },
    connect: function (id1, id2) {
      jsPlumb.ready(function () {
        var firstInstance = jsPlumb.getInstance()
        firstInstance.importDefaults({
          Connector: [ 'Bezier', { curviness: 150 } ],
          Anchors: [ 'TopCenter', 'BottomCenter' ]
        })
        jsPlumb.connect({ source: id1, target: id2 })
        jsPlumb.draggable(id1)
        jsPlumb.draggable(id2)
      })
      console.log(id1 + ' , ' + id2)
    },
    // Find out which element moved
    addItem: function (type) {
      if (type === 'Documents') {
        this['positions' + type].push([Math.random() * 1000, 100])
      } else if (type === 'Saturation') {
        this['positions' + type].push([Math.random() * 1000, 300])
      } else if (type === 'OCR') {
        this['positions' + type].push([Math.random() * 1000, 500])
      } else {
        this['positions' + type].push([Math.random() * 1000, 700])
      }
    },
    moveStart: function (event, type, index) {
      this.moving = true
      this['offsetInitX' + type + index] = event.offsetX
      this['offsetInitY' + type + index] = event.offsetY
    },
    moveEnd: function (event, type, index) {
      this.moving = false
    },
    moveActive: function (event, type, index) {
      if (this.moving) {
        // console.log(type + ' of index ' + index + ' is moving')
        var posVar = 'positions' + type
        this['movingTypeCurent'] = type
        this['movingIndexCurrent'] = index
        var deltaX = event.offsetX - this['offsetInitX' + type + index]
        var deltaY = event.offsetY - this['offsetInitY' + type + index]
        this.$set(this[posVar][index], 0, this[posVar][index][0] + deltaX)
        this.$set(this[posVar][index], 1, this[posVar][index][1] + deltaY)
        jsPlumb.animate(this)
      }
    }
  }
}
</script>
