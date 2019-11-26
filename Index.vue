<template>
  <q-page>
    <demo2  @mousedown.native="moveStart($event, 'Demo2', i-1)" @mouseup.native="moveEnd($event, 'Demo2', i-1)" @mousemove.native="moveActive($event, 'Demo2', i-1)"
    class="movable" :ref="'Demo2'+(i-1)"
     :style="{'left': positionsDemo2[i-1][0] + 'px', 'top': positionsDemo2[i-1][1] + 'px'}" v-for="i in positionsDemo2.length" :key="'demo2'+i"></demo2>
    <demo3  @mousedown.native="moveStart($event, 'Demo3', i-1)" @mouseup.native="moveEnd($event,'Demo3', i-1)" @mousemove.native="moveActive($event, 'Demo3', i-1)"
      class="movable" :ref="'Demo3'+(i-1)"
     :style="{'left': positionsDemo3[i-1][0] + 'px', 'top': positionsDemo3[i-1][1] + 'px'}" v-for="i in positionsDemo3.length" :key="'demo3'+i"></demo3>
     <button @click="addItem('Demo2')">Add document</button>
     <button @click="addItem('Demo3')">Add output block</button>
     <hr/>
     Open the console and move a demo2  element over an other demo2 element. Also, move a demo3 element over another demo2 element

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
import Demo2 from '../components/Demo2.vue'
import Demo3 from '../components/Demo3.vue'
import { TouchPan } from 'quasar'
export default {
  name: 'PageIndex',
  components: {
    Demo2,
    Demo3
  },
  directives: {
    TouchPan
  },
  data () {
    return {
      moving: false,
      // Separate arrays for tracking different elements

      // The arrays can also hold other properties of the elements apart from their positions
      // I'll leave that as an exercise

      // Any addition to the array will create a new element to the screen
      positionsDemo2: [[100, 100], [200, 200], [300, 300]],
      positionsDemo3: [[400, 400], [500, 500], [600, 600]]
    }
  },
  methods: {
    toggleItem: function () {
      this.show = !this.show
    },
    // Find out which element moved
    addItem: function (type) {
      // Read this:
      // https://vuejs.org/2016/02/06/common-gotchas/
      if (type === 'Demo2') {
        this['positions' + type].push([50, 50])
      } else {
        this['positions' + type].push([100, 160])
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

        // Check Collision with Demo2
        // looping for all elements of that type execept for itself

        for (var i = 0; i < this.positionsDemo2.length; i++) {
          if (index !== i) {
            let boundBox2

            // Get the bounding box of the other elements
            if (typeof this.$refs['Demo2' + i].$el === 'undefined') {
              boundBox2 = this.$refs['Demo2' + i][0].$el.getBoundingClientRect()
            } else {
              boundBox2 = this.$refs['Demo2' + i].$el.getBoundingClientRect()
            }

            var overlap = !(boundBox1.right < boundBox2.left ||
                            boundBox1.left > boundBox2.right ||
                            boundBox1.bottom < boundBox2.top ||
                            boundBox1.top > boundBox2.bottom)

            if (overlap) {
              console.log(type + ' of index ' + index + ' collided with Demo2 of index ' + i)
            }
          }
        }
      }
      // You can repeat the loop with Demo3 i.e positionsDemo3 to check collision with that element
      // I'll leave it as an exercise
    }
  }
}
</script>
