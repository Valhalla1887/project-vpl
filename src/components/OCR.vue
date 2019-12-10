<template>
  <div v-if="visible">
     <q-list bordered separator class="bg-white">
      <q-item clickable v-ripple class="bg-blue">
        <q-item-section>OCR</q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <q-item-section>
          <div class="row q-col-gutter-lg">
         <q-checkbox align="left" v-model="input" label="Input"/>
         <q-checkbox align="right" v-model="output" label="Output"/>
       </div>
        </q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <template>
  <div class="q-pa-md">
    <q-badge color="blue">
      Select Lines {{ standard.min }} to {{ standard.max }}
    </q-badge>
    <q-range v-model="standard" :min="0" :max="100"/>
  </div>
</template>
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
      input: false,
      output: false,
      standard: {
        min: 0,
        max: 80
      },
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
