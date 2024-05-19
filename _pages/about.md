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

# &lt;Whoami&gt;

<div class="terminal">
  <span class="prompt">$</span> whoami<br>
  <span class="output"></span><br>
</div>

<script>
  const outputElement = document.querySelector('.output');
  const text1 = "Bar Magnezi, \nI'm an Ethical Hacker from Israel. \nSpecialize in: \n* Malware Analysis \n*Reverse Engineering \n*Penetration Testing";

  let index1 = 0;
  let wordCount1 = 0;

  function type1() {
  if (index1 < text1.length) {
    if (text1.charAt(index1) === '\n') {
      outputElement.innerHTML += '<br>';
    } else {
      outputElement.innerHTML += text1.charAt(index1);
    }
    index1++;
    setTimeout(type1, 10); // Constant typing speed of 100ms
  }
}

  setTimeout(type1, 100); // Delay the second typing animation by 2000ms

  const text2 = 'Bar Magnezi';
  let index2 = 0;

</script>
