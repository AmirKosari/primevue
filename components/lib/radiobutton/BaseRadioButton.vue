<script>
import BaseComponent from 'primevue/basecomponent';
import RadioButtonStyle from 'primevue/radiobutton/style';

export default {
    name: 'BaseRadioButton',
    extends: BaseComponent,
    props: {
        value: null,
        modelValue: null,
        name: {
            type: String,
            default: null
        },
        disabled: {
            type: Boolean,
            default: false
        },
        readonly: {
            type: Boolean,
            default: false
        },
        tabindex: {
            type: Number,
            default: null
        },
        inputId: {
            type: String,
            default: null
        },
        inputClass: {
            type: [String, Object],
            default: null
        },
        inputStyle: {
            type: Object,
            default: null
        },
        ariaLabelledby: {
            type: String,
            default: null
        },
        ariaLabel: {
            type: String,
            default: null
        }
    },
    style: RadioButtonStyle,
    provide() {
        return {
            $parentInstance: this
        };
    }
};
</script>
