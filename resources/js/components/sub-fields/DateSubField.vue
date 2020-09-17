<template>
    <input
        ref="datePicker"
        :id="subField.name"
        :name="subField.name"
        :disabled="disabled"
        type="text"
        class="w-full form-control form-input form-input-bordered mx-2 mt-1"
        :class="{ '!cursor-not-allowed': disabled }"
        :value="value"
        v-bind="subField.attributes"
        :placeholder="subField.placeholder || ''"
        @input="$emit('input', $event.target.value)"
        :readonly="true"
    />
</template>

<script>
    import flatpickr from 'flatpickr'
    import 'flatpickr/dist/themes/airbnb.css'

    export default {
        props: {
            subField: {
                required: true,
            },
            value: {
                required: false,
            },
            placeholder: {
                type: String,
                default: () => {
                    return moment().format('YYYY-MM-DD')
                },
            },
            disabled: {
                type: Boolean,
                default: false,
            },
            dateFormat: {
                type: String,
                default: 'Y-m-d',
            },
            altFormat: {
                type: String,
                default: 'Y-m-d',
            },
            twelveHourTime: {
                type: Boolean,
                default: false,
            },
            enableTime: {
                type: Boolean,
                default: false,
            },
            enableSeconds: {
                type: Boolean,
                default: true,
            },
            firstDayOfWeek: {
                type: Number,
                default: 0,
            },
        },

        data: () => ({ flatpickr: null }),

        mounted() {
            this.$nextTick(() => {
                this.flatpickr = flatpickr(this.$refs.datePicker, {
                    enableTime: this.enableTime,
                    enableSeconds: this.enableSeconds,
                    onClose: this.onChange,
                    onChange: this.onChange,
                    dateFormat: this.dateFormat,
                    altInput: true,
                    altFormat: this.altFormat,
                    allowInput: true,
                    // static: true,
                    time_24hr: !this.twelveHourTime,
                    locale: { firstDayOfWeek: this.firstDayOfWeek },
                })
            })
        },

        methods: {
            onChange(event) {
                this.$emit('change', this.$refs.datePicker.value)
            },

            clear() {
                this.flatpickr.clear()
            },
        },

        beforeDestroy() {
            this.flatpickr.destroy()
        },
    }
</script>
<style scoped>
    .\!cursor-not-allowed {
        cursor: not-allowed !important;
    }
</style>

