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

<!-- Google Analytics -->
<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
</script>
<!-- End Google Analytics -->

<script>
function f() {
    document.write(Date())
    ga('create', 'UA-109909640-1', 'auto');
    ga('send', 'event', 'video', 'play', 'label_x');
}
</script>
<button type="button" onclick="f()"> button </button>
