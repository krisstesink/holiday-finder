<template>
      <!-- A closable error popup that creates a popup with an error message -->
    <div v-if="visible" class="error-popup">
      <p>{{ message }}</p>
      <button class="close-button" @click="closePopup">✖</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        visible: false,
        message: '',
      };
    },
    methods: {
      showError(msg) {
        this.message = msg;
        this.visible = true;
      },
      closePopup() {
        this.visible = false;
      },
    },
  };
  </script>
  
  <style scoped>
  .error-popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: red;
    color: white;
    padding: 20px 30px;
    border-radius: 8px;
    font-size: 16px;
    z-index: 9999;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 10px;
  }
  
  .close-button {
    background-color: transparent;
    color: white;
    border: none;
    font-size: 18px;
    cursor: pointer;
    margin-top: 10px;
  }
  
  .close-button:hover {
    color: #ffcccc;
  }
  </style>