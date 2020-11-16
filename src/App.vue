<template>
  <v-app>
    <v-main>
      <DialogProject />
    </v-main>
  </v-app>
</template>

<script>
import DialogProject from "./components/DialogProject";

export default {
  name: "App",

  components: {
    DialogProject,
  },

  data: () => ({
    //
  }),
};
</script>
