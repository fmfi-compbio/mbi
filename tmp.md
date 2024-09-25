---
title: Sandbox
---

{% include pdf.html filename="p01" show=1 dot=1 %} x {% include pdf.html filename="p02" show=1 dot=0 %} {% include pdf.html filename="p02" show=0 dot=0 %}

{% include pdf.html filename="p01" show=1 dot=1 %}&nbsp;
x
y
{% include pdf.html filename="p02" show=1 dot=0 %}

<span>
  {% include pdf.html filename="p01" show=1 dot=1 %}
  x
  y
  {% include pdf.html filename="p02" show=1 dot=0 %}
</span>

{% include notes.html file="cb01" name="notes" show=1 dot=0 %}