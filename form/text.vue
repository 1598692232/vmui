<template>
    <v-box :label="label" class="vmui-form-text-box" @icon:click="clear"> 
        <input v-if="!multiline" 
            ref="input" 
            type="text" 
            class="vmui-form-text" 
            :name="name" 
            :placeholder="placeholder" 
            v-model="val" 
            @focus="$emit('focus')" 
            @blur="$emit('blur')" 
            @click="$emit('click')" 
            :readonly="readonly"
        /> 

        <template v-else>
            <div 
                ref="input" 
                class="vmui-form-text" 
                :contenteditable="!readonly" 
                v-text="val" 
                @focus="$emit('focus')"
                @blur="$emit('blur')"
                @click="$emit('click')"
                @input="_input"
            ></div>
            <span v-if="!val" class="vmui-form-text-placeholder" v-text="placeholder"></span>
        </template>

        <template slot="msg-left">
            <slot name="msg-left"></slot>
        </template>    

        <template slot="msg-right">
            <slot name="msg-right"></slot>
        </template>    

        <template slot="icon" v-if="clearable && val">
            &times;
        </template>
    </v-box>
</template>

<style>
    .vmui-form-text-box{
        .vmui-form-box-icon{
            content: '';
            height: 0.28rem;
            width: 0.28rem;
            line-height: 0.28rem;
            font-size: 0.28rem;
        }
    }

    .vmui-form-text{
        width: 100%;
        font-size: .16rem;
        color: #222222;
        line-height: .28rem;
        &::-webkit-input-placeholder,
        &:-moz-placeholder,
        &::-moz-placeholder,
        &:-ms-input-placeholder {
    　　     color: #E1E1E1;
    　　}
    }

    div.vmui-form-text{
        min-height: .28rem;
        max-height: 1rem;
        height: auto;
        resize: none;
        overflow: auto;
        &:focus {
            border: 0;
            outline: 0;
        }
    }

    .vmui-form-text-placeholder{
        position: absolute;
        left: 0rem;
        top: 0rem;
        height: 0.28rem;
        font-size: .16rem;
        color: #E1E1E1;
        line-height: .28rem;
    }
</style>

<script>
import vBox from './box';
import {Single} from './abstract';

export default{
    mixins: [vBox, Single],

    props: {
        multiline: {
            type: Boolean,
            default: false
        },

        placeholder: {
            type: String,
            default: null
        },

        readonly: {
            type: Boolean,
            default: false
        },

        clearable: {
            type: Boolean,
            default: true
        }
    },

    components: {
        vBox
    },

    methods:{
        focus(){
            this.$refs.input.focus();
        },

        blur(){
            this.$refs.input.blur();
        },

        _input(){
            this.val = this.$refs.input.innerText;
        },

        clear(){
            this.val = '';
            this.$emit('clear');
        }
    }
}
</script>
