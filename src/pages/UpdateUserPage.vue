<template>
  <q-page class="row items-center justify-evenly">
    <SignupComp update-mod :usernamep="username" :emailp="email" :phonep="phone" :aboutp="about" />
  </q-page>
</template>

<script lang="ts">
import SignupComp from 'src/components/SignupComp.vue';
import { store } from 'src/store';
import { defineComponent } from 'vue';


export default defineComponent({
  name: 'UpdateUserPage',
  components: { SignupComp },
  data(){
    return{
      username: '',
      email: '',
      phone:  '',
      about:  '',
    }
   },
   mounted() {
    this.$axios.get(process.env.VUE_APP_BASE_URL+'/user',{
    headers: {
      Authorization: 'Bearer ' + store.token
    }})
    .then(response => {
      console.log(response);
      this.username = response.data.username;
      this.email = response.data.email;
      this.phone = response.data.phone;
      this.about = response.data.about;
    })
   },
});
</script>
