<template>
  <div>
     <q-list bordered separator class="bg-white">
      <q-item clickable v-ripple class="bg-green">
        <q-item-section>Documents</q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <q-item-section>
          <div class="row q-col-gutter-lg">
         <q-checkbox align="right" left-label="Output" v-model="val"/>
       </div>
        </q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <q-item-section>
          <q-item-label >
          <q-uploader
        url="http://localhost:4444/upload"
        label="Select Document"
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
      <q-item clickable v-ripple>
        {{ index }}
      </q-item>

    </q-list>
  <p></p>
  </div>
</template>

<script>
export default {
  // name: 'ComponentName',
  props: ['index'],
  data () {
    return {
      val: true
    }
  },
  methods: {
    deleteBlock: function () {
      this.$emit('deleteblock', this.index)
    }
  }
}
</script>

<style scoped>
div {
}
</style>
