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

<div class="resume-link">
  📄 <a href="/assets/docs/Resume_Joao.pdf" target="_blank">Download My Resume here!</a>
</div>

<script>
  const outputElement = document.querySelector('.output');
  const text1 = "João Cezarino, \nI'm an Security Analyst. \nSpecialized in: \n* Malware Analysis \n* Reverse Engineering \n* Cyber Threat Intelligence \n* Detection Engineering \n* Security Automation \n* ICS/OT Security ";

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
    setTimeout(type1, 17); // Constant typing speed of 100ms
  }
}

  setTimeout(type1, 100); // Delay the second typing animation by 2000ms
</script>
