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

.resume-link {
  margin-top: 20px;
  font-size: 20px;
  text-align: center;
}

.resume-link a {
  color: #f21368;
  text-decoration: none;
  font-weight: bold;
}

.resume-link a:hover {
  text-decoration: underline;
}

</style>

# &lt;Whoami?&gt;

<div class="terminal">
  <span class="prompt">$</span> whoami<br>
  <span class="output"></span><br>
</div>

<div class="resume-link">
  <br>
  📄 <a href="/assets/docs/Resume_Joao.pdf" target="_blank">Download My Resume here!</a>
</div>

<script>
  const outputElement = document.querySelector('.output');
  const text1 = "João Cezarino, \nI'm a Committed Security Analyst. \n\n" +
              "Specialized in: \n" +
              "* Cyber Threat Intelligence (CTI) \n" +
              "* Detection Engineering \n" +
              "* ICS/OT Security \n" +
              "* Malware Analysis & Reverse Engineering \n" +
              "* Security Automation \n" +
              "* Incident Response \n\n" +
              "Passionate about cybersecurity, I develop detection rules, automate threat intelligence processes, \n" +
              "and work on securing critical infrastructure. \n\n" +
              "Always open to sharing knowledge and learning from others. \n" +
              "Let's connect, exchange ideas, and collaborate on security challenges! \n\n" +
              "📄 Feel free to check out my resume below for more details!";

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
