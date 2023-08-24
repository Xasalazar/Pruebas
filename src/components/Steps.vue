<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="step-by-step-container">
    <div class="step-by-step-form">
      <div class="steps">
        <div class="title-label">{{ title }}</div>
        <div
          v-for="(step, index) in steps"
          :key="index"
          class="step-item"
          @click="setCurrentStep(index)"
        >
          <div :class="{ active: index === currentStep }" class="step">
            {{ index + 1 }}
          </div>
          <div
            v-show="ShowText"
            :class="['step-label', { active: index === currentStep }]"
          >
            {{ step }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentStep: 0,
    };
  },

  props: {
    steps: {
      type: Array,
      // eslint-disable-next-line vue/require-valid-default-prop
      default: ["Step 1", "Step 2", "Step 3"],
    },
    title: {
      type: String,
      default: "Prueba",
    },
    ShowText: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    setCurrentStep(stepIndex) {
      this.currentStep = stepIndex;
    },
  },
};
</script>

<style>
.step-by-step-container {
  background-color: var(--secondary-color);
  border: 1px solid #e0e0e0;
  padding: 1% 4%;
  margin: 0 auto;
  max-width: 70%;
  border-radius: 10px;
}
.step-by-step-form {
  margin: 0 auto;
}

.steps {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.step-item {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.title-label {
  color: var(--tertiary-color);
  white-space: nowrap;
  font-weight: bold;
  font-size: 120%;
}

.step {
  width: 30px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--secondary-color);
  border: 1px solid var(--primary-color);
  color: var(--primary-color);
  border-radius: 50%;
  margin-right: 10px;
}

.step-label {
  white-space: nowrap;
}

.step.active {
  background-color: var(--primary-color);
  color: var(--secondary-color);
}

.step-label.active {
  color: var(--primary-color);
}

/* .content {
  /* Estilos del contenido del paso 
} 
*/

.step-content {
  display: none;
}

.step-content.active {
  display: block;
}
</style>
