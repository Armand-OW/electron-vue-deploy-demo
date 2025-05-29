<script setup>
import { ref } from 'vue'

const selectedImage = ref('https://picsum.photos/1600/800')
const MessageTo = ref('3rd-years')
const Message = ref('May you always create bugs no one else can fix.')
const MessageFrom = ref('Armand')

</script>

<template>
  <div class="fullscreen-message" :style="`background-image: url('${selectedImage}');`">
    <div class="message-content">
      <h4>Dear {{ MessageTo }},</h4>
      <h2>
        {{ Message }}
      </h2>
      <h5>Sincerely,<br/>{{ MessageFrom }}</h5>
    </div>
  </div>
    
</template>


<style scoped>
body {
  color: black;
}

.fullscreen-message {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 2rem;
  z-index: 0;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}


.message-content {
  z-index: 2;
  font-family: sans-serif;
  font-size: 2rem;
  line-height: 1.5;
  color: white;
}

.fullscreen-message::before {
  content: '';
  position: absolute;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 0;
  color: white;
}
</style>
