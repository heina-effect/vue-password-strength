<template>
  <v-app>
    <v-main>
      <password-strength />
    </v-main>
  </v-app>
</template>

<script>
import PasswordStrength from './components/password-strength.vue';
export default {
  components: { PasswordStrength },
  name: 'App',

  data: () => ({
    //
  }),
};
</script>