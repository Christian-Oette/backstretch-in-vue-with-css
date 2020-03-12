<template>
  <div :style="divstyle">
    <slot />
  </div>
</template>

<script>
export default {
    props: {
        resizeToWindowView: true,
        heightPercentageOfWindow: Number,        
        widthPercentageOfWindow: Number,
        width : undefined,
        height : undefined,        
        image: undefined,
        focusXinPercent: Number,
        focusYinPercent: Number    
    },    
    watch: {
        width: function(val) {
            this.configuredWidth = val;       
            this.computeStyle();                    
        },
        height: function(val) {
            this.configuredHeight = val;         
            this.computeStyle();
        },
        image: function(val) {
            this.configuredImage = val;         
            this.computeStyle();
        },
        focusXinPercent: function(val) {
             this.xOffset = val;  
             this.computeStyle();   
        },
        focusYinPercent: function(val) {
             this.yOffset = val;  
             this.computeStyle();   
        }

    },
    data() {
        return {
            divstyle: {},
            configuredWidth: undefined,
            configuredHeight: undefined,
            configuredImage: undefined,
            xOffset: undefined,
            yOffset: undefined
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
           const backgroundImage = this.configuredImage ? `url('${this.configuredImage}')` : 'none'                
           const defaultStyle = {
                'background-repeat': 'no-repeat',
                'background-size': 'cover',            
                'background-position-y': this.yOffset ? this.inPercent(this.yOffset) : this.inPercent(0), 
                'background-position-x': this.xOffset ? this.inPercent(this.xOffset) : this.inPercent(0),
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
                style = {width: this.inPixels(this.configuredWidth), height: this.inPixels(this.configuredHeight), ...defaultStyle}                
            }                                  
            this.divstyle = style;         
        }
    },
    created() {          
        this.configuredWidth = this.width;                  
        this.configuredHeight = this.height;
        this.configuredImage = this.image;
        this.yOffset = this.focusYinPercent;
        this.xOffset = this.focusXinPercent;
        window.addEventListener("resize", this.handleResize);        
        this.handleResize();                
    },
    destroyed() {
        window.removeEventListener("resize", this.handleResize);
    },
}
</script>