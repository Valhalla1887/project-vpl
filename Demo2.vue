<template>
  <div>
     <q-list bordered separator>
      <q-item clickable v-ripple>
        <q-item-section>Documents</q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <q-item-section>
         <q-checkbox v-model="val"/>
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
      val: true
    }
  }
}
</script>

<style scoped>
div {
}
</style>
