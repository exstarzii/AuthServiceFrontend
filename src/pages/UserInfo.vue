<template>
  <div class="q-pa-md">
    <q-table
      title="User Info"
      :rows="rows"
      row-key="name"
      hide-pagination
      hide-header
    >
    </q-table>
    <div class="q-pa-md q-gutter-sm">
      <q-btn href='/#/userupdate' label="Edit Info" color="primary"/>
      <q-btn label="Delete Account" color="red" @click="confirm"/>
    </div>
  </div>
</template>

<script lang="ts">
import { store } from 'src/store';
import { defineComponent } from 'vue';


const columns = [
  { name: 'col1', align: 'left', label: '', field: 'col1'},
  { name: 'col2', align: 'left', label: '', field: 'col2'},
]


export default defineComponent({
   setup(){
    return{
      columnsT: columns,
    }
   },
   data(){
    return{
      username:'',
      rows : [
        {
          col1: 'Username',
          col2: '',
        },
        {
          col1: 'Email',
          col2: '',
        },
        {
          col1: 'Phone',
          col2: '',
        },
        {
          col1: 'About',
          col2: '',
        },
      ]
    }
   },
   mounted() {
    this.$axios.get(process.env.VUE_APP_BASE_URL+'/user',{
    headers: {
      Authorization: 'Bearer ' + store.token
    }})
    .then(response => {
      console.log(response);
      this.rows[0].col2 = response.data.username
      this.rows[1].col2 = response.data.email
      this.rows[2].col2 = response.data.phone
      this.rows[3].col2 = response.data.about
    })
  },
  methods:{
    confirm () {
      this.$q.dialog({
        title: 'Deleting',
        message: 'Are you sure to delete account?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.delete();
      })
    },
    delete(){
      this.$axios.delete(process.env.VUE_APP_BASE_URL+'/user',{
      headers: {
        Authorization: 'Bearer ' + store.token
      }})
      .then(response => {
        console.log(response);
        this.$q.notify('Account has been deleted');
        store.token='';
        this.$router.push({path:'/'});
      })
      .catch( (error) => {
        if(error.response.status == 401){
          this.$q.notify({
          color:  'negative',
          icon:'report_problem',
          message:'You are not logged in',
          })
        }else if(error.response.status == 404){
          this.$q.notify({
          color:  'negative',
          icon:'report_problem',
          message:'The server is not available',
          })
        }
      })
    }
  }
});
</script>
