<template>
  <div>
    <div class="row">
      <div class="image-gallery">
        <div
          v-for="(data,index) in initial_imdata"
          :key="imdata_keys[index]"
        >
          <m-annotator
            :class="image_accepted(data) ? 'gallery-accepted':''"
            ref="annotators"
            :initial_imdata="data"
            :read_only="true"
            @cclick="$emit('selection', index);"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import {defineComponent} from 'vue';
import MAnnotator from './m-annotator.vue';
import {image_accepted} from '../util'

 export default defineComponent({
  name : 'MImageGallery',
  components: { 'm-annotator':MAnnotator },
  props: { initial_imdata:{type:Array, default: () => []}, imdata_keys:{type:Array} },
  emits: ['selection'],
  data : function() { return { selection:null }},
  created : function (){},
  mounted : function (){
      // this.$refs.gallery_modal.addEventListener('show.bs.modal',this.modalclick);
  },
  methods : {
    // kinds of feedback: 
    // 1. this image does not have what I'm looking for (checkbox?)
    // 2. 
    get_class(index){
      let ldata = this.initial_imdata[index];
      if (ldata.boxes == null){
        return 'unknown'
      } else if (ldata.boxes.length > 0) {
        return 'accepted'
      } else if (ldata.boxes.length === 0){
        return 'rejected'
      }
    },
    image_accepted(imdata){ // make it accessible from the <template>
          return image_accepted(imdata)
    },
  }
})
</script>
<style scoped>
/* makes a grid https://css-tricks.com/seamless-responsive-photo-grid/
  with no whitespace gaps... not quite clear to me how it picks layout */

.image-gallery {
  /* Prevent vertical gaps */
  display: flex;
  flex-wrap: wrap;
  line-height: 0;
  row-gap: 0px;
  justify-content: center;
  /* column-count:         5;*/
  column-gap: 0px;
}

.image-gallery img {
  /* Just in case there are inline attributes */
  /* width: auto !important;
  height: 300px !important; */
  width: auto !important;
  height: auto !important;
  max-height: 200px;
  /*max-width: 400px; */
  object-fit: scale-down; /* never rescale up */ 
  /* flex-grow: 2; */
  /* flex-shrink: 2; */
  /* box-shadow: 0 1px 2px rgba(0,0,0,.15); */
  /* transition: all 100ms ease-out; */
  transition: opacity 100ms;
}

.gallery-accepted{
  opacity: .7;
}

.image-gallery img.rejected {
  opacity: .5;
  border: 5px solid red;
}

.image-gallery .rejected:hover {
  opacity: 1;
}

.image-gallery .accepted {
  border: 5px solid green;
  opacity: .5;
}

.image-gallery .accepted:hover {
  opacity: 1;
}

.image-gallery .unknown:hover {
  /* box-shadow: 0 3px 5px rgba(0,0,0,.3); */
  opacity: .5;
  /* translateY(.5%) scale(1.01, 1.01); */
  /* translate bc top image wraps otherwise */ 
}

</style>