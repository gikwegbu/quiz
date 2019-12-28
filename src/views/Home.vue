<template>
  <div class="home">
    <b-contain> 
      <br><br>
        <b-col sm='6' offset-sm="3"> 
          <FormField> </FormField>
        </b-col>  
      
    </b-contain>
  </div>
</template>

<script>
// @ is an alias to /src 
import FormField from '@/components/Form.vue' 
export default {
  name: "home", 
  components: {
    FormField
  }
};
</script>
