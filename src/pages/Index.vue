<template>
  <q-page>
    <Documents  @mousedown.native="moveStart($event, 'Documents', i-1)" @mouseup.native="moveEnd($event, 'Documents', i-1)" @mousemove.native="moveActive($event, 'Documents', i-1)"
    class="movable" :ref="'Documents'+(i-1)"
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
     <q-btn @click="removeItem('Documents')">Remove Documents Block</q-btn>
     <q-btn @click="removeItem('Saturation')">Remove Saturation Block</q-btn>
     <q-btn @click="removeItem('OCR')">Remove OCR Block</q-btn>
     <q-btn @click="removeItem('SaveDoc')">Remove SaveDoc Block</q-btn>
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
    toggleItem: function () {
      this.show = !this.show
    },
    removeItem: function (type) {
      this['positions' + type].pop()
    },
    // Find out which element moved
    addItem: function (type) {
      // Read this:
      // https://vuejs.org/2016/02/06/common-gotchas/
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
        console.log(type + ' of index ' + index + ' is moving')
        var posVar = 'positions' + type
        this['movingTypeCurent'] = type
        this['movingIndexCurrent'] = index
        var deltaX = event.offsetX - this['offsetInitX' + type + index]
        var deltaY = event.offsetY - this['offsetInitY' + type + index]
        console.log('I am here')
        // update the positions of the element in that specific index
        // we use this.$set because vue cannot track simple array assignments
        // https://vuejs.org/2016/02/06/common-gotchas/
        console.log(posVar)
        this.$set(this[posVar][index], 0, this[posVar][index][0] + deltaX)
        this.$set(this[posVar][index], 1, this[posVar][index][1] + deltaY)
      }
    }
  }
}
</script>
