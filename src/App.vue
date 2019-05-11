<template>
  <v-app id="id">
    <v-content>
      <v-container fluid fill-height>
        <v-layout justify-center align-center>
            <v-flex text-xs-center>
              <router-view/>
            </v-flex>
        </v-layout>
      </v-container>
    </v-content>
</v-app>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
