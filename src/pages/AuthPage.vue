<template>
  <q-page class="row items-center justify-evenly">
    <div class="q-pa-md" style="max-width: 400px">
    
    <q-form
      @submit="onSubmit"
      class="q-gutter-md"
    >
      <q-input
        filled
        type="email"
        v-model="email"
        label="Email "
        lazy-rules
        :rules="[
          val => val !== null && val !== '' || 'Please type your email',
        ]"
      />

      <q-input 
        filled
        hint = ''
        v-model="password"
        label="Password "
        :type="isPwd ? 'password' : 'text'"
      >
        <template v-slot:append>
          <q-icon
            :name="isPwd ? 'visibility_off' : 'visibility'"
            class="cursor-pointer"
            @click="isPwd = !isPwd"
          />
        </template>
      </q-input>

      <div>
        <q-btn label="Submit" type="submit" color="primary"/>
      </div>
    </q-form>

  </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { store } from '../store';

export default defineComponent({
  data(){
      return {
        isPwd: ref(true),
        email: '',
        password: '',
        store
      }
   },
   methods: {
    onSubmit() {
      this.$axios.post(process.env.VUE_APP_BASE_URL+'/user/login',
      { 
        email : this.email,
        password : this.password 
      }).then(response => {
        console.log(response);
        this.$q.notify('Logged');
        store.token = response.data.access_token;
      }).catch( (error) => {
        if(error.response.status == 401){
          this.$q.notify({
          color:  'negative',
          icon:'report_problem',
          message:'Wrong login or password',
          })
        }else if(error.response.status == 404){
          this.$q.notify({
          color:  'negative',
          icon:'report_problem',
          message:'The server is not available',
          })
        }
      })
    },
  }
});
</script>
