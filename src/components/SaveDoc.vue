<template>
  <div v-if="visible">
     <q-list bordered separator class="bg-white">
      <q-item clickable v-ripple class="bg-yellow">
        <q-item-section>Save Document As</q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <q-item-section>
          <div class="row q-col-gutter-lg">
         <q-checkbox align="right" label="Input" v-model="val"/>
       </div>
        </q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <q-item-section>
          <q-item-label >
          <q-uploader
        url="http://localhost:4444/download"
        label="Save As..."
        color="white"
        text-color="black"
        style="max-width: 200px"
      />
          </q-item-label>
        </q-item-section>
      </q-item>
        <q-item-section>
         <q-btn color="grey">Save</q-btn>
         <q-btn color="red" @click="deleteBlock()">Delete</q-btn>
        </q-item-section>
    </q-list>
  <p></p>
  </div>
</template>

<script>
export default {
  // name: 'ComponentName',
  data () {
    return {
      val: true,
      visible: true
    }
  },
  methods: {
    deleteBlock: function () {
      this.visible = false
    }
  }
}
</script>

<style scoped>
div {
}
</style>
