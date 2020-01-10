<template>
  <div class="page-wrapper">
      <b-navbar toggleable="lg" class="shadow navbar" type="light">
        <b-navbar-brand class="text-light text-brand">COLEKTIA - TWITTER APP</b-navbar-brand>
        <b-navbar-toggle target="nav-collapse" type="light"></b-navbar-toggle>
      </b-navbar>
      <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld';
export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  data: () => ({
    //
  }),
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Patrick+Hand&display=swap');
.navbar {
  background: #076585;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #076585, #fff);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #076585, #fff); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.navbar-toggler-icon {
  color: black !important;
}

.text-brand {
  font-family: 'Patrick Hand', cursive;
}
</style>
