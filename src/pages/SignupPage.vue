<template>
  <q-page class="row items-center justify-evenly">
    <SignupComp></SignupComp>
  </q-page>
</template>

<script lang="ts">
import SignupComp from 'src/components/SignupComp.vue';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'IndexPage',
  components: { SignupComp },

});
</script>
