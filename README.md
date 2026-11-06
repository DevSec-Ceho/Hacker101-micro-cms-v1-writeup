# Hacker101 CTF Write-up: Micro-CMS v1

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Hacker101-4CAF50?style=for-the-badge" alt="Hacker101">
  <img src="https://img.shields.io/badge/Category-Web%20Security-blue?style=for-the-badge" alt="Web Security">
  <img src="https://img.shields.io/badge/Difficulty-Easy-orange?style=for-the-badge" alt="Easy Difficulty">
  <img src="https://img.shields.io/badge/Flags-4%2F4-success?style=for-the-badge" alt="Four Flags">
</p>

<h2>📌 Challenge Information</h2>

<table>
  <tr><th>Information</th><th>Details</th></tr>
  <tr><td>Platform</td><td>Hacker101 CTF</td></tr>
  <tr><td>Challenge Name</td><td>Micro-CMS v1</td></tr>
  <tr><td>Category</td><td>Web Security</td></tr>
  <tr><td>Difficulty</td><td>Easy</td></tr>
  <tr><td>Total Flags</td><td>4</td></tr>
  <tr><td>Objective</td><td>Identify and exploit web application vulnerabilities</td></tr>
  <tr><td>Status</td><td>Four flags targeted</td></tr>
</table>

<h2>📝 Introduction</h2>

<p>This repository documents my journey through the <strong>Micro-CMS v1</strong> challenge on Hacker101 CTF.</p>

<p>Micro-CMS v1 is a web application security challenge designed to introduce common vulnerabilities found in Content Management Systems (CMS).</p>

<p>The application provides functionality for viewing, creating, and editing pages. By investigating its behavior, examining page identifiers, and testing how user input is handled, it is possible to discover multiple security weaknesses.</p>

<p>The challenge contains <strong>four hidden flags</strong>, each associated with a different vulnerability or attack technique.</p>

<h2>🔍 Step 1: Initial Reconnaissance</h2>

<p>After launching the challenge instance, I began by exploring the available pages and understanding how the application worked.</p>

<p>During the initial investigation, I discovered that the application supported page viewing and editing.</p>

<p>I also created a new page to understand how the CMS assigned page identifiers.</p>

<h3>Screenshot 1: Page Enumeration</h3>

<p align="center">
  <img width="1279" height="601" alt="notfound1" src="https://github.com/user-attachments/assets/48920d5a-c706-4079-a840-96d32636dd34" />
</p>

<p align="center"><em>Figure 1: Attempting to access page 7 returned a Not Found response.</em></p>

<h3>Screenshot 2: Newly Created Page</h3>

<p align="center">
 <img width="1097" height="581" alt="notfound2" src="https://github.com/user-attachments/assets/b3709430-a16f-4394-a09b-ddeedc6856f4" />
</p>

<p align="center"><em>Figure 2: A newly created page was assigned the identifier 13, revealing the application's numbered page structure and editing functionality.</em></p>

<h2>🚩 Flag 1: Access Control Bypass (IDOR)</h2>

<h3>Discovery</h3>

<p>While investigating the application, I noticed that pages were identified using sequential numeric identifiers.</p>

<p>This suggested that modifying the page identifier in the URL might reveal other pages that were not immediately visible through the application's interface.</p>

<h3>Exploitation</h3>

<p>I investigated different page identifiers and observed how the application responded to existing, inaccessible, and nonexistent pages.</p>

<p>When a page indicated that access was restricted, I investigated the corresponding page-editing endpoint:</p>

<pre><code>/page/edit/&lt;page_id&gt;</code></pre>

<p>The difference in access control between viewing and editing pages provided an opportunity to investigate whether the application properly enforced authorization on both endpoints.</p>

<p><strong>Security Concept:</strong> Insecure Direct Object Reference (IDOR) and broken access control.</p>

<p><strong>Flag:</strong> [Insert your first discovered flag here.]</p>

<h2>🚩 Flag 2: SQL Injection</h2>

<h3>Discovery</h3>

<p>During further testing, I examined how the application processed page identifiers supplied through its URLs.</p>

<p>Since page identifiers were passed to the server, I considered whether the application properly validated and handled unexpected input.</p>

<h3>Exploitation</h3>

<p>I investigated the page-editing endpoint and tested how it responded to a single quotation mark appended to the page identifier.</p>

<pre><code>/page/edit/1'</code></pre>

<p>The application's response exposed behavior consistent with an SQL injection vulnerability.</p>

<p>This demonstrated the importance of properly handling user-controlled input when interacting with databases.</p>

<p><strong>Security Concept:</strong> SQL Injection (SQLi).</p>

<p><strong>Flag:</strong> [Insert your second discovered flag here.]</p>

<h2>🚩 Flag 3: Cross-Site Scripting (XSS)</h2>

<h3>Discovery</h3>

<p>While examining the page creation and editing functionality, I investigated how the CMS handled user-supplied page titles.</p>

<p>I considered whether the application properly sanitized or encoded input before displaying it in the browser.</p>

<h3>Exploitation</h3>

<p>I tested the title field with HTML and JavaScript-related input to investigate whether injected content would be interpreted by the browser.</p>

<p>When user-controlled content is rendered without appropriate output encoding, it can introduce a stored Cross-Site Scripting vulnerability.</p>

<p><strong>Security Concept:</strong> Stored Cross-Site Scripting (XSS).</p>

<p><strong>Flag:</strong> [Insert your third discovered flag here.]</p>

<h2>🚩 Flag 4: HTML and JavaScript Injection</h2>

<h3>Discovery</h3>

<p>For the final flag, I investigated the application's page-content functionality and how it processed HTML and Markdown input.</p>

<p>The application provided an interesting opportunity to examine whether its content filtering mechanisms could be bypassed.</p>

<h3>Exploitation</h3>

<p>I tested alternative HTML elements and event-handler-based input to investigate whether executable JavaScript could be introduced through page content.</p>

<p>I also examined the resulting page and its source code for information exposed after the content was processed.</p>

<p>This demonstrated that restricting traditional script tags alone is not sufficient to prevent Cross-Site Scripting.</p>

<p><strong>Security Concept:</strong> HTML injection and Cross-Site Scripting (XSS).</p>

<p><strong>Flag:</strong> [Insert your fourth discovered flag here.]</p>

<h2>🛡️ Vulnerability Analysis</h2>

<p>The Micro-CMS v1 challenge demonstrates how weaknesses in access control, database input handling, and content sanitization can affect a web application.</p>

<table>
  <tr><th>Vulnerability</th><th>Security Impact</th></tr>
  <tr><td>IDOR / Broken Access Control</td><td>Unauthorized access to restricted pages</td></tr>
  <tr><td>SQL Injection</td><td>Potential database information disclosure or manipulation</td></tr>
  <tr><td>Stored XSS</td><td>Execution of injected scripts in users' browsers</td></tr>
  <tr><td>HTML Injection / XSS</td><td>Injection of unintended HTML and potentially executable event handlers</td></tr>
</table>

<h2>💡 Lessons Learned</h2>

<p>This challenge reinforced several important web application security concepts:</p>

<ul>
  <li>Always investigate how applications manage page identifiers.</li>
  <li>Never assume that hidden pages are properly protected.</li>
  <li>Enforce authorization checks on every endpoint and HTTP method.</li>
  <li>Use parameterized queries to prevent SQL injection.</li>
  <li>Sanitize user input and apply context-aware output encoding.</li>
  <li>Do not rely exclusively on blocking script tags to prevent XSS.</li>
  <li>Inspect both the rendered webpage and its underlying source code.</li>
</ul>

<h2>🧰 Tools Used</h2>

<ul>
  <li>Kali Linux</li>
  <li>Mozilla Firefox</li>
  <li>Browser Developer Tools</li>
  <li>Hacker101 CTF Platform</li>
  <li>Manual Web Application Testing</li>
</ul>

<h2>🏁 Conclusion</h2>

<p><strong>Micro-CMS v1</strong> provided valuable practical experience in identifying common web application vulnerabilities.</p>

<p>Through investigating page identifiers, testing input handling, examining content rendering, and understanding access control, this challenge demonstrates why proper security implementation is essential for modern web applications.</p>

<p>The four-flag structure makes it an excellent exercise for developing foundational web penetration testing skills.</p>

<p><strong>Challenge: Micro-CMS v1</strong></p>
<p><strong>Flags: 4</strong></p>
<p><strong>Status: Successfully Completed</strong></p>

<hr>

<p align="center">
  <strong>Hack. Learn. Improve. Repeat. 🚀</strong>
  <br>
  <em>Documenting my journey in cybersecurity and ethical hacking.</em>
</p>
