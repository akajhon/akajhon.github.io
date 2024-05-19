---
layout: default
title: About
permalink: /about/
---

<style>
.terminal {
  font-family: monospace;
  background-color: #000;
  color: #0f0;
  padding: 10px;
  border-radius: 5px;
  overflow: hidden;
}

.prompt {
  color: #0f0;
  display: inline-block;
  width: 20px;
  animation: blink-caret 0.75s step-end infinite;
}

.output {
  color: #fff;
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
  animation: type-text 2s steps(30, end);
}

@keyframes type-text {
  from {
    width: 0;
  }
}

@keyframes blink-caret {
  from, to {
    border-color: transparent;
  }
  50% {
    border-color: #0f0;
  }
}
</style>

# Whoami

<div class="terminal">
  <span class="prompt">$</span> whoami<br>
  <span class="output"></span><br>
</div>

<script>
  const outputElement = document.querySelector('.output');
  const text = 'Bar Magnezi';
  let index = 0;

  function type() {
    if (index < text.length) {
      outputElement.textContent += text.charAt(index);
      index++;
      setTimeout(type, 100);
    }
  }

  type();
</script>
