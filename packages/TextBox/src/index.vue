<template>
<div :class="['fv-'+$theme+'-TextBox', status, isFocus ? 'focus' : '', isDisabled ? 'disabled' : '', isUnderline ? 'underline': '', { shadow: isBoxShadow }]" :style="[isFocus ? focusStyles.textBox : styles.textBox, { 'border': disabledBorderWhenReveal && revealBorder ? 'none' : `` }, { borderRadius: `${borderRadius}px` }, { padding: revealBorder ? `${borderWidth}px` : ''}]" @click="isFocus = true">
    <div class="text-box-reveal-container" :style="{ background: background }">
        <div v-show="prefix != ''" class="fix-block">
            <p>{{prefix}}</p>
        </div>
        <i v-show="leftIcon != ''" class="ms-Icon icon-block" :class="[`ms-Icon--${leftIcon}`]" @click="$emit('left-icon-click', $event)"></i>
        <core v-model="thisValue" :mode="mode" :type="type" :placeholder="placeholder" :mask="mask" :flag="flag" :pattern="pattern" :readonly="readonly" :maxlength="maxlength" :disabled="disabled" :focus.sync="isFocus" @keydown="$emit('keydown', $event)" @keyup="$emit('keyup', $event)"></core>
        <i v-show="icon != ''" class="ms-Icon icon-block" :class="[`ms-Icon--${icon}`]" @click="$emit('icon-click', $event)"></i>
        <div v-show="suffix != ''" class="fix-block">
            <p>{{suffix}}</p>
        </div>
    </div>
</div>
</template>

<script>
import core from './sub/core.vue';
import { FluentRevealEffect } from "fluent-reveal-effect";

export default {
    name: "FvTextBox",
    components: {
        core
    },
    props: {
        value: {
            default: ""
        },
        mode: {
            default: "default"
        },
        placeholder: {
            default: ""
        },
        type: {
            default: "text"
        },
        mask: {
            type: String,
            default: "mask:___"
        },
        flag: {
            type: String,
            default: "_"
        },
        pattern: {
            type: String,
            default: "[\S\s]*"
        },
        readonly: {
            default: false
        },
        maxlength: {
            default: ''
        },
        prefix: {
            default: ""
        },
        suffix: {
            default: ""
        },
        leftIcon: {
            default: ""
        },
        icon: {
            default: ""
        },
        underline: {
            default: false
        },
        background: {
            default: ""
        },
        borderWidth: {
            default: 1
        },
        borderColor: {
            default: ""
        },
        focusBorderColor: {
            default: ""
        },
        borderRadius: {
            default: 0
        },
        isBoxShadow: {
            default: false
        },
        revealBorder: {
            default: false
        },
        disabledBorderWhenReveal: {
            default: true
        },
        status: {
            default: ""
        },
        disabled: {
            default: false
        },
        theme: {
            type: String,
            default: "system"
        }
    },
    data () {
        return {
            thisValue: this.value,
            isFocus: false,
            styles: {
                textBox: {
                    borderWidth: `${this.borderWidth}px`,
                    borderColor: `${this.borderColor}`
                }
            },
            focusStyles: {
                textBox: {
                    borderWidth: `${this.borderWidth}px`,
                    borderColor: `${this.focusBorderColor}`
                }
            }
        };
    },
    watch: {
        value (val) {
            this.thisValue = val;
        },
        thisValue (val) {
            this.$emit("input", val);
        },
        background (val) {
            this.stylesInit();
            this.onFocusStylesInit();
        },
        borderWidth (val) {
            this.stylesInit();
            this.onFocusStylesInit();
        },
        borderColor (val) {
            this.stylesInit();
        },
        focusBorderColor (val) {
            this.onFocusStylesInit();
        },
        revealBorder (val) {
            if(val) {
                this.FRInit();
            }
        }
    },
    computed: {
        $theme() {
            if (this.theme == "system") return this.$fvGlobal.state.theme;
            return this.theme;
        },
        isUnderline () {
            return (
                this.underline.toString() == "true" ||
                this.underline == "underline" ||
                this.underline === ""
            );
        },
        isDisabled () {
            return (
                this.disabled.toString() == "true" ||
                this.disabled == "disabled" ||
                this.disabled === ""
            );
        },
        borderLightColor () {
            if(this.$theme == 'light') {
                return 'rgba(121, 119, 117, 0.6)';
            }
            if(this.$theme == 'dark' || this.$theme == 'custom') {
                return 'rgba(255, 255, 255, 0.6)';
            }
            return 'rgba(121, 119, 117, 0.6)';
        },
        backgroundLightColor () {
            if(this.$theme == 'light') {
                return 'rgba(121, 119, 117, 0.3)';
            }
            if(this.$theme == 'dark' || this.$theme == 'custom') {
                return 'rgba(255, 255, 255, 0.3)';
            }
            return 'rgba(121, 119, 117, 0.3)';
        }
    },
    mounted () {
        if(this.revealBorder)
            this.FRInit();
        this.stylesInit();
        this.onFocusStylesInit();
    },
    methods: {
        FRInit () {
            let FR = new this.$RevealEffectsMasked("body", {
                selector: this.$el,
                borderGradientSize: 60,
                borderLightColor: this.borderLightColor,
                backgroundLightColor: this.backgroundLightColor,
                childrenSelector: this.$el.querySelectorAll('.text-box-reveal-container')[0]
            });
        },
        stylesInit () {
            this.styles.textBox.background = this.background;
            this.styles.textBox.borderWidth = `${this.borderWidth}px`;
            this.styles.textBox.borderColor = this.borderColor;
        },
        onFocusStylesInit () {
            this.focusStyles.textBox.background = this.background;
            this.focusStyles.textBox.borderWidth = `${this.borderWidth}px`;
            this.focusStyles.textBox.borderColor = this.focusBorderColor;
        }
    }
};
</script>
