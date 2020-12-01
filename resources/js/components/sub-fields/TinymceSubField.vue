<template>
  <editor
      v-model="localValue"
      :api-key="subField.options.apiKey"
      cloud-channel="5"
      :init="initOptions"
      :plugins="subField.options.plugins"
      :toolbar="subField.options.toolbar"
      :placeholder="subField.placeholder"
      :name="subField.name"
      @input="changed"
  />
</template>

<script>
import Editor from '@tinymce/tinymce-vue';

export default {
  components: {'editor': Editor},
  props: ['subField', 'value'],
  data() {
    return {
      initOptions: {...this.subField.options.init, input : this.changed},
      localValue: this.value,
    };
  },
  methods:{
     changed(event){
       this.$emit('input', this.localValue);
     }
  }
}
</script>