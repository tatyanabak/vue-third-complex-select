<template>
  <v-app>
    <v-main>
      <UserForm />
    </v-main>
  </v-app>
</template>

<script>
import UserForm from './components/UserForm.vue'

export default {
  name: 'App',

  components: {
    UserForm
  }
}
</script>
