<template>
  <div class="full-height p-d-flex p-jc-center p-ai-center">
      <login-form class="login-card" />
  </div>
</template>
<script>
import LoginForm from '@/components/LoginForm'
export default {
  components:{
    LoginForm
  },
  setup() {
    
  },
}
</script>
<style scoped>
.full-height { 
  height: 100vh;
}

.login-card{
  width: 40%;
}

</style>