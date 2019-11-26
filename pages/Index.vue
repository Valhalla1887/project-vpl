<template>
  <q-page>
    <Documents  @mousedown.native="moveStart($event, 'Documents', i-1)" @mouseup.native="moveEnd($event, 'Documents', i-1)" @mousemove.native="moveActive($event, 'Documents', i-1)"
    class="movable" :ref="'Documents'+(i-1)"
     :style="{'left': positionsDocuments[i-1][0] + 'px', 'top': positionsDocuments[i-1][1] + 'px'}" v-for="i in positionsDocuments.length" :key="'Documents'+i" :index="i-1"></Documents>
    <Saturation  @mousedown.native="moveStart($event, 'Saturation', i-1)" @mouseup.native="moveEnd($event,'Saturation', i-1)" @mousemove.native="moveActive($event, 'Saturation', i-1)"
      class="movable" :ref="'Saturation'+(i-1)"
     :style="{'left': positionsSaturation[i-1][0] + 'px', 'top': positionsSaturation[i-1][1] + 'px'}" v-for="i in positionsSaturation.length" :key="'Saturation'+i"></Saturation>
     <OCR  @mousedown.native="moveStart($event, 'OCR', i-1)" @mouseup.native="moveEnd($event,'OCR', i-1)" @mousemove.native="moveActive($event, 'OCR', i-1)"
      class="movable" :ref="'OCR'+(i-1)"
     :style="{'left': positionsOCR[i-1][0] + 'px', 'top': positionsOCR[i-1][1] + 'px'}" v-for="i in positionsOCR.length" :key="'OCR'+i"></OCR>
     <SaveDoc  @mousedown.native="moveStart($event, 'SaveDoc', i-1)" @mouseup.native="moveEnd($event,'SaveDoc', i-1)" @mousemove.native="moveActive($event, 'SaveDoc', i-1)"
      class="movable" :ref="'SaveDoc'+(i-1)"
     :style="{'left': positionsSaveDoc[i-1][0] + 'px', 'top': positionsSaveDoc[i-1][1] + 'px'}" v-for="i in positionsSaveDoc.length" :key="'SaveDoc'+i"></SaveDoc>
     <button @click="addItem('Documents')">Add Documents block</button>
     <button @click="addItem('Saturation')">Add Saturation block</button>
     <button @click="addItem('OCR')">Add OCR block</button>
     <button @click="addItem('SaveDoc')">Add SaveDoc block</button>
     <hr/>
     <button @click="removeItem('Documents', index)">Remove Documents block</button>
     <button @click="removeItem('Saturation')">Remove Saturation block</button>
     <button @click="removeItem('OCR')">Remove OCR block</button>
     <button @click="removeItem('SaveDoc')">Remove SaveDoc block</button>
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

      positionsDocuments: [],
      positionsSaturation: [],
      positionsOCR: [],
      positionsSaveDoc: []
    }
  },
  methods: {
    removeItem: function (type, index) {
      this['positions' + type].splice(index, 1)
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

        // Check for collision

        var boundBox1

        // get Bounding box of the current element
        // sometimes the $el is in [0] sometimes it is not. checking for both
        if (typeof this.$refs[type + index].$el === 'undefined') {
          boundBox1 = this.$refs[type + index][0].$el.getBoundingClientRect()
        } else {
          boundBox1 = this.$refs[type + index].$el.getBoundingClientRect()
        }

        // Check Collision with Documents
        // looping for all elements of that type execept for itself

        for (var i = 0; i < this.positionsDocuments.length; i++) {
          if (index !== i) {
            let boundBox2

            // Get the bounding box of the other elements
            if (typeof this.$refs['Documents' + i].$el === 'undefined') {
              boundBox2 = this.$refs['Documents' + i][0].$el.getBoundingClientRect()
            } else {
              boundBox2 = this.$refs['Documents' + i].$el.getBoundingClientRect()
            }

            var overlap = !(boundBox1.right < boundBox2.left ||
                            boundBox1.left > boundBox2.right ||
                            boundBox1.bottom < boundBox2.top ||
                            boundBox1.top > boundBox2.bottom)

            if (overlap) {
              console.log(type + ' of index ' + index + ' collided with Documents of index ' + i)
            }
          }
        }
      }
      // You can repeat the loop with Saturation i.e positionsSaturation to check collision with that element
      // I'll leave it as an exercise
    }
  }
}
</script>
