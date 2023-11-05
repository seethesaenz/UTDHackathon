<script setup>
import { ref } from 'vue';
import { RouterLink, RouterView } from 'vue-router'
import Chat from './components/Chat.vue';

const showChat = ref(false);
</script>

<template>


  <Chat v-if="showChat" style="position: absolute; top: 60%; left: 70%; background-color: white; height: 200px; padding: 10px; border: 2px solid #1e78a6;"/>
  <button v-if="!showChat" @click="showChat = true" class="chat-btn">
    <img alt="Chat" src="@/assets/robot.png" height="50" />
  </button>

  <header>
    <router-link to="/">
      <!-- <img alt="Vue logo" class="logo" src="@/assets/fannie-mae-logo-white.webp" height="30" /> -->
      <img alt="Logo" class="logo" src="@/assets/fannie-slae.png" height="60" />
    </router-link>
  </header>

  <main>
    <RouterView />
  </main>


  <footer>
    <section>
      <h2>Resources</h2>
      <ul>
        <li>
          <a href="https://www.fanniemae.com/content/guide/selling/b3/6/02.html" target="_blank" rel="noopener noreferrer">Fannie Mae Selling Guide</a>
        </li>
        <li>
          <a href="https://www.fanniemae.com/content/guide/selling/b3/6/03.html" target="_blank" rel="noopener noreferrer">Fannie Mae Eligibility Matrix</a>
        </li>
        <li>
          <a href="https://www.fanniemae.com/content/guide/selling/b3/6/05.html" target="_blank" rel="noopener noreferrer">Fannie Mae Credit Score</a>
        </li>
        <li>
          <a href="https://www.fanniemae.com/content/guide/selling/b3/6/04.html" target="_blank" rel="noopener noreferrer">Fannie Mae LTV</a>
        </li>
        <li>
          <a href="https://www.fanniemae.com/content/guide/selling/b3/6/07.html" target="_blank" rel="noopener noreferrer">Fannie Mae DTI</a>
        </li>
      </ul>
    </section>

    <section>
      <h2>Links</h2>
      <ul>
        <li>
          <a href="/">Home</a>
        </li>
        <li>
          <a href="/single">Qualification Calculator</a>
        </li>
        <li>
          <a href="/bulk">Report Generator</a>
        </li>
      </ul>
    </section>

    <section>
      <h2>Disclaimer</h2>
      <p>
        This tool is for informational purposes only. It is not intended to provide legal, financial, or accounting advice. Please consult with the appropriate professional(s) for advice specific to your situation.
      </p>
    </section>
  </footer>
</template>

<style scoped>
.chat-btn {
  position: absolute;
  top: 80%;
  left: 90%;
  background-color: white;
  border: 3px solid #1e78a6;
  border-radius: 50%;
  padding: 10px;
  cursor: pointer;
  z-index: 1;
}

.chat-btn:hover {
  background-color: #1e78a6;
}

header {
  background-color: #1e78a6;
  color: #fff;
  padding: 20px;
}

footer {
  background-color: whitesmoke;
  color: #1e78a6;
  padding: 20px 200px;
  display: flex;
  gap: 20px;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  
}

footer section {
  flex: 1;
}

footer section h2 {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 0;
}

footer section ul {
  list-style: none;
  padding: 0;
}

footer section ul li {
  margin-bottom: 10px;
}

footer section ul li a {
  color: #1e78a6;
}

footer section ul li a:hover {
  text-decoration: underline;
}

footer section p {
  margin-top: 0;
}

main {
  padding: 20px;
}
</style>