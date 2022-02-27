<template>
  <div id="app">
    <notes />
  </div>
</template>

<script>
import Notes from "./components/Notes";


export default {
  name: "App",
  components: { Notes },
};
</script>
<style lang="scss">
$base-color: #219EBC;
$btn-color: #8ECAE6;
$nt1-color: #023047;
$nt2-color: #FFB703;
$nt3-color: #FB8500;

html,
body {
  width: 100% !important;
  height: 100% !important;
  margin:auto;
}

div#app{
  height: 100% ;
  width: 100%;
  
  overflow:  auto;
  background-color:$base-color;
}
</style>
