<template>
  <div id="front">
    <div class="wrapper">
    <div class="item">
      1
      <div class="label">Model Assets</div>
    </div>
    <div class="glue"></div>
    <div class="item">
      2
      <div class="label">Identify Threats</div>
    </div>
    <div class="glue"></div>
    <div class="item">
      3
      <div class="label">Manage Threats</div>
    </div>
    <div class="glue"></div>
    <div class="item undone">
      4
      <div class="label">Done</div>
    </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'App',
      data: () => ({
      message: 'MetaProcess'
    })
};
</script>

<style lang="scss" scoped>
.item {
  height: 3em;
  width: 3em;
  border-radius: 50%;
  background-color: #7B886B;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  border: solid 0.2em #7B886B;
}
.item.undone {
  background:  #2a2a2a;
}
.wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.glue {
  height: 0px;
  width: 10em;
  border-bottom: 0.6em solid #7B886B;
}
.label {
  position: absolute;
  top: 5.5em;
  color: white;
}

body {
  background: #2a2a2a;
  padding: 1em;
  font-family: Helvetica;
  font-size: 20px;
}

</style>
