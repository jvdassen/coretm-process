<template>
  <div id="front">
    <div class="wrapper">
      <div class="item" :class="{ undone: !step1 }">
        <span>1</span>
        <div class="label">Model Assets</div>
      </div>
      <div class="glue" :class="{ undone: !step2 }"></div>
      <div class="item" :class="{ undone: !step2 }">
        <span>2</span>
        <div class="label">Identify Threats</div>
      </div>
      <div class="glue" :class="{ undone: !step3 }"></div>
      <div class="item" :class="{ undone: !step3 }">
        <span>3</span>
        <div class="label">Manage Threats</div>
      </div>
      <div class="glue" :class="{ undone: !step4 }"></div>
      <div class="item" :class="{ undone: !step4 }">
        <span>4</span>
        <div class="label">Done</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    step1: true,
    step2: true,
    step3: false,  
    step4: false
  }),
  mounted: function () {
   window.app = this
  }
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
  flex-grow: 0;
  flex-shrink: 0;
}
.glue {
  height: 0.5em;
  width: 10em;
  border-bottom: 0.1em solid #7B886B;
  border-top: 0.1em solid #7B886B;
  background: #7B886B;
  flex-grow: 1;
}
.item.undone {
  background:  #2a2a2a;
}
.glue.undone {
  background: none;
}
.wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  height: 7em;
  padding: 3em;
}
.label {
  position: absolute;
  top: 6em;
  color: white;
}

body {
  background: #2a2a2a;
  padding: 1em;
  font-family: Helvetica;
  font-size: 20px;
}

</style>
