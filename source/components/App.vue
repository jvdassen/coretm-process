<template>
  <div id="front">
    <div class="wrapper">
      <div class="item" :class="{ undone: !step1 }">
        <span>1</span>
        <div class="label">Methodology</div>
      </div>
      <div class="glue" :class="{ undone: !step1 }"></div>
      <div class="item" :class="{ undone: !step2 }">
        <span>2</span>
        <div class="label">Model Assets</div>
      </div>
      <div class="glue" :class="{ undone: !step2 }"></div>
      <div class="item" :class="{ undone: !step3 }">
        <span>3</span>
        <div class="label">Model Threats</div>
      </div>
      <div class="glue" :class="{ undone: !step3 }"></div>
      <div class="item" :class="{ undone: !step4 }">
        <span>4</span>
        <div class="label">Communicate Findings</div>
      </div>
    </div>
    <div class="instruction-wrapper">
      <div class="define-method-info" v-if="!step1">
        <p>Define a threat modeling methodology to support your use case. The <i>Questionnaire</i> may help you find an appropriate methodology</p>
        <div class="action-buttons">
          <button @click="openQuestionnaire">Open Questionnaire</button>
          <button @click="done('step1')">Done</button>
        </div>
      </div>
      <div class="model-assets-info"  v-else-if="!step2">
        <p>Create a model that depicts relevant assets of your organization using the <i>Editor</i>. If you already hold a model, you can upload it. If not, follow the previously selected methodology to model assets.</p>
        <div class="action-buttons">
          <button @click="openEditor()">Open Editor</button>
          <button @click="done('step2')">Done</button>
        </div>
      </div>
      <div class="model-threats-info" v-else-if="!step3">
        <p>Elicit, brainstorm, discuss and annotate relevant threats in the <i>Editor</i>.</p>

        <p v-if="methodology === 'stride'">Since you previously selected the <i>STRIDE</i> methodology, you can use the <i>STRIDE</i> component for guidance.</p>
        <div class="action-buttons">
          <button v-if="methodology === 'stride'">Open STRIDE method</button>

          <button>Open Editor</button>
          <button @click="done('step3')">Done</button>
        </div>
      </div>
      <div class="communicate-info" v-else-if="!step4">
        <p>Once you are happy with your model, make sure to communicate your findings. For example, you could create a GitHub issue out of a finding of the <i>Report</i> to communicate it with Software Developers. </p>

        <div class="action-buttons">
          <button>Open Report</button>
          <button @click="done('step4')">Done</button>
        </div>
      </div>
      <div class="catch-info" v-else-if="step1 && step2 && step3 && step4">
        <p>You are done!</p>
      </div>
      <div class="catch-info" v-else>
        <p>Something went wrong :-(</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    step1: false,
    step2: false,
    step3: false,
    step4: false,
    methodology: undefined
  }),
  watch: {
    step1: function () {
      this.store()
    },
    step2: function () {
      this.store()
    },
    step3: function () {
      this.store()
    },
    step4: function () {
      this.store()
    }
  },
  mounted: function () {
    window.app = this
    var progress = dizmo.publicStorage.getProperty('progress') || defaultStruc ()
    this.step1 = progress.step1
    this.step2 = progress.step2
    this.step3 = progress.step3
    this.step4 = progress.step4
    dizmo.publicStorage.subscribeToProperty('progress', function (_, progress) {
      this.step1 = progress.step1
      this.step2 = progress.step2
      this.step3 = progress.step3
      this.step4 = progress.step4
    })

    function defaultStruc () {
      return { step1: false, step2: false, step3: false, step4: false }
    }
  },
  methods: {
    done (stepX) {
      this.$data[stepX] = true
    },
    store () {
      var result = dizmo.publicStorage.getProperty('progress') || {}
      result.step1 = this.step1
      result.step2 = this.step2
      result.step3 = this.step3
      result.step4 = this.step4
      dizmo.publicStorage.setProperty('progress', result)
    },
    openQuestionnaire () {
      var bundle = viewer.getBundleById('com.vonderassen.coretm_questionnaire') 
      bundle.instantiateDizmo({
        'geometry/x': dizmo.getAttribute('geometry/x'),
        'geometry/y': dizmo.getAttribute('geometry/y') + dizmo.getHeight() + 80
      }, {}, {}, function (questionnaire) {
        questionnaire.focus()
        questionnaire.publicStorage.subscribeToProperty('discoverymethod', function (_, method) {
          if (method) {
            app.step1 = true
          }
        })
      })
    },
    openEditor () {
      var bundle = viewer.getBundleById('com.vonderassen.coretm') 
      bundle.instantiateDizmo({
        'geometry/x': dizmo.getAttribute('geometry/x') + 420 + 40,
        'geometry/y': dizmo.getAttribute('geometry/y') + dizmo.getHeight() + 80
      }, {}, {}, function (editor) {
        editor.focus()
        editor.publicStorage.subscribeToProperty('xyz', function (_, method) {
        })
      })
    }
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

.instruction-wrapper {
  padding: 1em;
  color: white;
}

.action-buttons {
  display: flex;
}

.action-buttons > button {
  flex-grow: 1;
  flex-basis: 100%;
  margin-left: 2px;
  box-shadow: none;
  background-color: var(--dizmo-font-color);
  color: var(--dizmo-background-color);
}
.action-buttons > button:active {
  background-color: var(--dizmo-background-color);
  color: var(--dizmo-font-color);
}

</style>
