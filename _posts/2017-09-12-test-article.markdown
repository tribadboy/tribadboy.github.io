---
title: It is a test article
---

<h1>Head</h1>
<p>Hello, world</p>


<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-109909640-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-109909640-1');
</script>

<script>
function f() {
    document.write(Date())
    ga('send', 'event', 'video', 'play', 'label_x');
}
</script>
<button type="button" onclick="f()"> button </button>
