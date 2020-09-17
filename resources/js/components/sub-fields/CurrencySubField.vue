<template>
    <div class="w-full">
        <div class="flex flex-wrap items-stretch w-full relative mt-1">
            <div class="flex -mr-px">
                  <span
                      class="flex items-center leading-normal rounded rounded-r-none border border-r-0 border-60 px-3 whitespace-no-wrap bg-30 text-80 text-sm font-bold"
                  >
                    $
                  </span>
            </div>
            <input
                :id="subField.name"
                :name="subField.name"
                type="text"
                class="flex-shrink flex-grow flex-auto leading-normal w-px flex-1 rounded-l-none form-control form-input form-input-bordered"
                :value="value"
                v-bind="subField.attributes"
                :placeholder="subField.placeholder"
                @input="$emit('input', $event.target.value)"
            />
        </div>
    </div>
</template>

<script>

    export default {
        props: ['subField', 'value'],

        computed: {
            defaultAttributes() {
                return {
                    type: 'number',
                    min: this.subField.min,
                    max: this.subField.max,
                    step: this.subField.step,
                    pattern: this.subField.pattern,
                    placeholder: this.subField.placeholder || this.subField.name,
                    class: this.errorClasses,
                }
            },
            extraAttributes() {
                const attrs = this.subField.extraAttributes

                return {
                    // Leave the default attributes even though we can now specify
                    // whatever attributes we like because the old number field still
                    // uses the old field attributes
                    ...this.defaultAttributes,
                    ...attrs,
                }
            },
        },
    }
</script>
