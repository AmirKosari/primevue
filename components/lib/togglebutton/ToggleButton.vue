<template>
    <div v-ripple :class="cx('root')" v-bind="getPTOptions('root')" data-pc-name="togglebutton" :data-p-highlight="active" :data-p-disabled="disabled">
        <input
            :id="inputId"
            type="checkbox"
            role="switch"
            :class="[cx('input'), inputClass]"
            :style="inputStyle"
            :value="modelValue"
            :checked="active"
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
        <slot name="icon" :value="modelValue" :class="cx('icon')">
            <span v-if="onIcon || offIcon" :class="[cx('icon'), modelValue ? onIcon : offIcon]" v-bind="getPTOptions('icon')" />
        </slot>
        <span :class="cx('label')" v-bind="getPTOptions('label')">{{ label }}</span>
    </div>
</template>

<script>
import Ripple from 'primevue/ripple';
import { ObjectUtils } from 'primevue/utils';
import BaseToggleButton from './BaseToggleButton.vue';

export default {
    name: 'ToggleButton',
    extends: BaseToggleButton,
    emits: ['update:modelValue', 'change', 'focus', 'blur'],
    methods: {
        getPTOptions(key) {
            return this.ptm(key, {
                context: {
                    active: this.active,
                    disabled: this.disabled
                }
            });
        },
        onChange(event) {
            if (!this.disabled && !this.readonly) {
                this.$emit('update:modelValue', !this.modelValue);
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
        active() {
            return this.modelValue === true;
        },
        hasLabel() {
            return ObjectUtils.isNotEmpty(this.onLabel) && ObjectUtils.isNotEmpty(this.offLabel);
        },
        hasIcon() {
            return this.$slots.icon || (this.onIcon && this.offIcon);
        },
        label() {
            return this.hasLabel ? (this.modelValue ? this.onLabel : this.offLabel) : '&nbsp;';
        }
    },
    directives: {
        ripple: Ripple
    }
};
</script>
