<script setup>
import { ref, onMounted, watch } from 'vue'

const code = ref('')
const submitted = ref(false)
const error = ref('')

function handleSubmit() {
  if (code.value.trim().toUpperCase() === 'ENGLISH') {
    submitted.value = true
    error.value = ''
  } else {
    error.value = 'Invalid code. Please try again.'
  }
}

// Watch for changes to submitted state and load Delphi scripts when true
watch(submitted, (newValue) => {
  if (newValue) {
    // Use setTimeout to ensure DOM is updated before adding scripts
    setTimeout(() => {
      loadDelphiScripts()
    }, 100)
  }
})

function loadDelphiScripts() {
  // Create and add the configuration script
  const configScript = document.createElement('script')
  configScript.id = 'delphi-page-script'
  configScript.textContent = `
    window.delphi = {...(window.delphi ?? {}) };
    window.delphi.page = {
      config: "7e4ca7b1-d916-45a9-8062-6a2c4b5707c0",
      overrides: {
        landingPage: "OVERVIEW",
      },
      container: {
        width: "100%",
        height: "800px",
      },
    };
  `
  document.getElementById('delphi-container').appendChild(configScript)
  
  // Create and add the bootstrap script
  const bootstrapScript = document.createElement('script')
  bootstrapScript.id = 'delphi-page-bootstrap'
  bootstrapScript.src = 'https://embed.delphi.ai/loader.js'
  document.getElementById('delphi-container').appendChild(bootstrapScript)
}
</script>

<template>
  <div class="container">
    <div class="card">
      <template v-if="!submitted">
        <h1>Welcome to <span class="highlight">Ilya Clone</span></h1>
        <p class="subtitle">Want to talk with Ilya clone? First insert the code!</p>
        
        <form @submit.prevent="handleSubmit" class="form">
          <input
            v-model="code"
            type="text"
            placeholder="Enter code here"
            class="input"
            :class="{ 'input-error': error }"
          />
          <button type="submit" class="button">
            <span>Submit</span>
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
          </button>
        </form>
        <p v-if="error" class="error">{{ error }}</p>
      </template>
      <template v-else>
        <div class="success-container">
          <svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 24 24" fill="none" stroke="#42b883" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path><polyline points="22 4 12 14.01 9 11.01"></polyline></svg>
          <h2>Welcome to Ilya Clone!</h2>
          <p class="success-message">Your access has been verified.</p>
          
          <div id="delphi-container" class="delphi-container"></div>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
.container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

.card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 2.5rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.1);
  width: 90%;
  max-width: 500px;
  text-align: center;
}

h1 {
  font-size: 2.2rem;
  margin-bottom: 0.5rem;
  background: linear-gradient(90deg, #42b883, #64d8cb);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 700;
}

.highlight {
  color: #42b883;
  -webkit-text-fill-color: initial;
}

.subtitle {
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 2rem;
  font-size: 1.1rem;
}

.form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin: 1.5rem 0;
}

.input {
  padding: 1rem 1.5rem;
  font-size: 1rem;
  background: rgba(255, 255, 255, 0.07);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  color: white;
  width: 100%;
  transition: all 0.3s ease;
}

.input:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 2px rgba(66, 184, 131, 0.2);
}

.input::placeholder {
  color: rgba(255, 255, 255, 0.4);
}

.input-error {
  border-color: #ff3b30;
  box-shadow: 0 0 0 2px rgba(255, 59, 48, 0.2);
}

.button {
  padding: 1rem 1.5rem;
  font-size: 1rem;
  background: linear-gradient(90deg, #42b883, #347474);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-weight: 600;
}

.button:hover {
  background: linear-gradient(90deg, #369870, #2a5c5c);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(66, 184, 131, 0.3);
}

.button:active {
  transform: translateY(0);
}

.icon {
  transition: transform 0.3s ease;
}

.button:hover .icon {
  transform: translateX(4px);
}

.error {
  color: #ff3b30;
  margin-top: 0.5rem;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.error::before {
  content: "⚠️";
}

.success-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

.success-container h2 {
  font-size: 1.8rem;
  color: #42b883;
  margin: 1rem 0 0.5rem;
}

.success-message {
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 0.5rem;
}

.coming-soon {
  color: rgba(255, 255, 255, 0.6);
  font-style: italic;
  margin-top: 1rem;
}

.delphi-container {
  width: 100%;
  margin-top: 2rem;
  border-radius: 8px;
  overflow: hidden;
}

@media (min-width: 768px) {
  .form {
    flex-direction: row;
  }
  
  .button {
    width: auto;
  }
  
  .input {
    width: auto;
    flex: 1;
  }
}
</style>
