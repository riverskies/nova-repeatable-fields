<template>
    <div>
        <div  class="mt-4 flex items-center">
            <input
                class="w-full form-control form-input form-input-bordered form-file-input select-none"
                :id="`${inputId}_file`"
                v-bind="subField.attributes"
                :placeholder="subField.placeholder"
                ref="fileField"
                type="file"
                @change="fileChange"
            />
            <label
                :for="`${inputId}_file`"
                class="form-file-btn btn btn-default btn-primary select-none"
            >
              <span v-if="uploading"
              >{{ __('Uploading') }} ({{ uploadProgress }}%)</span
              >
                <span v-else>{{ __('Choose File') }}</span>
            </label>
          <div class="ml-4">
            <span v-if="error" class="italic text-red-400 text-xs flex-shrink">{{error}}</span>
            <span v-if="uploading" class="italic text-xs flex-shrink">Uploading...</span>
            <span v-if="!uploading && value" class="italic text-xs flex-shrink">{{ fileName }}</span>
          </div>
        </div>
    </div>

</template>

<script>
    import axios from 'axios';

    export default {
        props: [
            'subField',
            'value'
        ],
        data() {
            return {
                inputId: null,
                uploading: false,
                error: '',
                fileName: this.value,
            };
        },
        methods: {
            fileChange(event){
                this.uploading = false;
                const data = new FormData();
                data.append('document', event.target.files[0]);

                axios({
                  method: 'post',
                  url: '/documents',
                  data: data,
                  headers: {'Content-Type': 'multipart/form-data'}
                })
                .then( (response) => {
                   this.uploading = false;
                   this.fileName = response.data.fileName;
                   this.$emit('input', this.fileName);
                   console.log(response);
                })
                .catch((response) => {
                  this.uploading = false;
                  this.error = response.message;
                  console.log(response);
                });
            },
        },
        created() {
            this.inputId = `${this.subField.name}_${Math.floor(Math.random() * 1000)}`;
        },
    }
</script>