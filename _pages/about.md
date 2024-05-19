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
}

.output {
  color: #fff;
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
}
</style>

# <Whoami>

<div class="terminal">
  <span class="prompt">$</span> whoami<br>
  <span class="output"></span><br>
</div>

<script>
  const outputElement = document.querySelector('.output');
  const text1 = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.';
  let index1 = 0;

  function type1() {
  if (index1 < text1.length) {
    if (text1.charAt(index1) === ' ') {
      outputElement.innerHTML += '<br>';
    }
    outputElement.textContent += text1.charAt(index1);
    index1++;
    setTimeout(type1, 20); // Constant typing speed of 100ms
  }
}

  setTimeout(type1, 100); // Delay the second typing animation by 2000ms

  const text2 = 'Bar Magnezi';
  let index2 = 0;


</script>
