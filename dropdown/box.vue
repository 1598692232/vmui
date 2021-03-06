<template>
<Shade ref="container" :visible="visibility">
    <Overlay ref="overlay" :visible="visibility" :position="above ? 'bottom' : 'top'" :fx="true" class="vmui-dropbox">
        <slot></slot>
    </Overlay>
</Shade>
</template>

<style>
.vmui-dropbox-transparent{
    background: transparent;
}

.vmui-dropbox{
    position: absolute;
}
</style>

<script>
import Overlay from '../overlay';
import Shade from '../shade';
import _ from '../helper';
import Resize from '../resize';

export default{
    mixins: [Overlay],

    props: {
        handler: null,

        offset: {
            type: Object,
            default(){
                return {
                    x: 0,
                    y: 0
                }
            }
        }
    },

    data(){
        return {
            above: false
        };
    },
    
    components: {
        Overlay,
        Shade
    },

    computed: {
        above(){
            var bodyHeight = _.height(document);
            var rect = _.rect(this.dom);

            return rect.top + rect.height > bodyHeight/2;
        }
    },

    mounted(){
        var self = this;

        self.handler && self.setHandler(self.handler);
        self.$nextTick(() => {
            self.initEvent();
        });
    },

    methods: {
        initEvent(){
            var $overlay = this.$refs.overlay.$el;

            _.on(this.$refs.overlay.$el, 'click', (e) => {
                e.stopPropagation();
            });

            _.on(document, 'click', (e) => {   
                if(_.contains($overlay, e.target)){
                    return false;
                }

                if(_.contains(this.dom, e.target)){
                    this.toggle();
                    return false;
                }

                this.close();
            });
        },

        setHandler(handler){
            this.dom = handler;
        },

        toggle(){
            this.visibility ? this.close() : this.open();
        },

        open(){
            var self = this, bottom = _.rect(self.dom).bottom;

            if(Overlay.methods.open.call(self) !== false){
                _.css(self.$el, {
                    top: bottom,
                    height: _.height(document) - bottom
                });

                self.$emit('open');
                setTimeout(() => {
                    Resize.resize(self.$refs.overlay.$el);
                });
            }
        },

        close(){   
            if(Overlay.methods.close.call(this) !== false){
                this.$emit('close');
            }
        }
    }
}
</script>