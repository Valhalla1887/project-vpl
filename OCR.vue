<template>
  <div>
     <q-list bordered separator>
      <q-item clickable v-ripple>
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
         <q-btn>Save</q-btn>
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
      }
    }
  }
}
</script>

<style scoped>
div {
}
</style>
