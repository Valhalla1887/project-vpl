<template>
  <div v-if="visible">
     <q-list bordered separator class="bg-white">
      <q-item clickable v-ripple class="bg-green">
        <q-item-section>Documents</q-item-section>
      </q-item>
      <q-item clickable v-ripple>
        <q-item-section>
          <div class="row q-col-gutter-lg">
         <q-space />
         <q-checkbox label="Output" v-model="val" left-label="true" id="id"/>
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
        {{ id }}
    </q-list>
  <p></p>
  </div>
</template>

<script>
export default {
  // name: 'ComponentName',
  props: ['id'],
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
