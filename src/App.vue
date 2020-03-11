<template>
  <div>    
    <examples/>        
  </div>  
</template>

<script>
import examples from './components/examples';

export default {
  data() {
    return {
      lockToWindow: false
    }
  },
  components: {
    examples
  }    
}

</script>