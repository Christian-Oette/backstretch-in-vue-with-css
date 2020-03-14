<template>
  <div :style="divstyle">
    <slot />
  </div>
</template>

<script>
export default {
    props: {
        resizeToWindowView : {
            type: Boolean,
            default: true
        },
        heightPercentageOfWindow : {
            type: Number,
            default: 100
        },
        widthPercentageOfWindow : {
            type: Number,
            default: 100
        },
        width : {
            type: Number,
            default: undefined
        },
        height : {
            type: Number,
            default: undefined
        },
        image : {
            type: String,
            required: true
        },
        focusXinPercent : {
            type: Number,
            default: 100
        },
        focusYinPercent : {
            type: Number,
            default: 100
        }
    },    
    watch: {
        width: function() {
            this.computeStyle();
        },
        height: function() {
            this.computeStyle();
        },
        image: function() {       
            this.computeStyle();
        },
        focusXinPercent: function(val) {
             this.computeStyle();   
        },
        focusYinPercent: function(val) {
             this.computeStyle();   
        }

    },
    data() {
        return {
            divstyle: {}
        }
    },
    methods: {       
        inPixels(size) {
            return size + 'px';
        },
        inPercent(size) {
            return size + '%';
        },
        handleResize() {                           
            this.computeStyle();
        },
        computeStyle() {            
           let style = {};
           console.log(this.yOffset);
           const backgroundImage = this.image ? `url('${this.image}')` : 'none'                
           const defaultStyle = {
                'background-repeat': 'no-repeat',
                'background-size': 'cover',            
                'background-position-y': this.focusYinPercent ? this.inPercent(this.focusYinPercent) : this.inPercent(0), 
                'background-position-x': this.focusXinPercent ? this.inPercent(this.focusXinPercent) : this.inPercent(0),
                backgroundImage,
            }

            if (this.resizeToWindowView) {   
                const computedHeight = this.heightPercentageOfWindow === 100 ? window.outerHeight : window.outerHeight * (this.heightPercentageOfWindow / 100);                                
                const computedWidth = this.widthPercentageOfWindow === 100 ? window.outerWidth: window.outerWidth * (this.widthPercentageOfWindow / 100);   

                style = {
                    width: this.inPixels(computedWidth), 
                    height: this.inPixels(computedHeight),                                         
                    ...defaultStyle
                } 
            }
            else 
            {                                
                style = {width: this.inPixels(this.width), height: this.inPixels(this.height), ...defaultStyle}                
            }                                  
            this.divstyle = style;         
        }
    },
    created() {          
        window.addEventListener("resize", this.handleResize);        
        this.handleResize();                
    },
    destroyed() {
        window.removeEventListener("resize", this.handleResize);
    },
}
</script>