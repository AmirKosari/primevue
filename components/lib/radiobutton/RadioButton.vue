<template>
    <div :class="cx('root')" v-bind="getPTOptions('root')" data-pc-name="radiobutton" :data-p-highlight="checked" :data-p-disabled="disabled">
        <input
            :id="inputId"
            type="radio"
            :class="[cx('input'), inputClass]"
            :style="inputStyle"
            :value="value"
            :name="name"
            :checked="checked"
            :tabindex="tabindex"
            :disabled="disabled"
            :readonly="readonly"
            :aria-labelledby="ariaLabelledby"
            :aria-label="ariaLabel"
            @focus="onFocus"
            @blur="onBlur"
            @change="onChange"
            v-bind="getPTOptions('input')"
        />
        <div :class="cx('box')" v-bind="getPTOptions('box')">
            <div :class="cx('icon')" v-bind="getPTOptions('icon')"></div>
        </div>
    </div>
</template>

<script>
import { ObjectUtils } from 'primevue/utils';
import BaseRadioButton from './BaseRadioButton.vue';

export default {
    name: 'RadioButton',
    extends: BaseRadioButton,
    emits: ['update:modelValue', 'change', 'focus', 'blur'],
    methods: {
        getPTOptions(key) {
            return this.ptm(key, {
                context: {
                    checked: this.checked,
                    disabled: this.disabled
                }
            });
        },
        onChange(event) {
            if (!this.disabled && !this.readonly) {
                this.$emit('update:modelValue', this.value);
                this.$emit('change', event);
            }
        },
        onFocus(event) {
            this.$emit('focus', event);
        },
        onBlur(event) {
            this.$emit('blur', event);
        }
    },
    computed: {
        checked() {
            return this.modelValue != null && ObjectUtils.equals(this.modelValue, this.value);
        }
    }
};
</script>
