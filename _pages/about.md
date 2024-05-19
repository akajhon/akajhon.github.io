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

# Whoami

<div class="terminal">
  <span class="prompt">$</span> whoami<br>
  <span class="output"></span><br>
  <span class="prompt">$</span> echo "Bar Magnezi, a security automation engineer and malware analyst"<br>
  <span class="output"></span><br>
</div>

<script>
  const outputElement = document.querySelector('.output');
  const text1 = 'Bar Magnezi, a security automation engineer and malware analyst';
  let index1 = 0;

  function type1() {
    if (index1 < text1.length) {
      outputElement.textContent += text1.charAt(index1);
      index1++;
      setTimeout(type1, 100); // Constant typing speed of 100ms
    }
  }

  setTimeout(type1, 2000); // Delay the second typing animation by 2000ms

  const text2 = 'Bar Magnezi';
  let index2 = 0;

  function type2() {
    if (index2 < text2.length) {
      outputElement.textContent += text2.charAt(index2);
      index2++;
      setTimeout(type2, 100); // Constant typing speed of 100ms
    }
  }

  type2();
</script>
