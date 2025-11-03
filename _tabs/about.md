---
layout: about
icon: fas fa-info-circle
order: 4
navigable: true
searchable: true
description: About me.
image:
  path: /commons/devices-mockup.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Responsive rendering of Chirpy theme on multiple devices.
---
<html>
<div style="text-align: center">
    <a id="avatar" class="rounded-circle">
      {%- if site.avatar != empty and site.avatar -%}
        {%- capture avatar_url -%}
          {% include media-url.html src=site.avatar %}
        {%- endcapture -%}
        <img src="{{- avatar_url -}}" width="112" height="112" alt="avatar" onerror="this.style.display='none'" style="border-radius: 100%">
      {%- endif -%}
    </a>
  </div>
  <!-- .profile-wrapper -->
</html>
[*TODO: replace this picture*]


Software Engineer focusing on game development and occasional sound designer, audio engineer, and voice actor.

Working in industry since 2021. Currently at Paraglacial.

___