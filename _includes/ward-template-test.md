---
---
<p>
Attempt: 35
</p>
{% comment %}
Try to eat up all leading slashes and the trailing .html . 
This is gross but it works.
{% endcomment %}
{% assign ward-id = page.url | replace: '.html', '' | split: '/' | last -%}
{% assign ward-info = site.data.site-data.position-tags |
where:"PositionUniqueName",ward-id | first -%}
<p>
Ward Info: {{ ward-info }}
</p><p>
Site URL: {{ site.url }}
</p><p>
Site BaseURL: {{ site.baseurl }}
</p>
<h2>Who is Running?</h2>

{% assign municipal-info = site.data.site-data.municipality-map |
where:"Name",ward-info.WardMunicipality | first %}
{% assign races-split = municipal-info.Races | split: ',' %}
<p>
Municipal-info: {{ municipal-info }} 
</p><p>
Race Array: {{ races-split }}
</p><p>
{% for race in races-split -%}
  {% if race == "_SELF" %}
    {% assign race = ward-id %}
  {% endif %}
  
  {% assign race-info = site.data.site-data.position-tags |
  where:"PositionUniqueName",race | first -%}

<h3>{{ race-info.PositionDesc }}</h3>

  
  {% assign these-nominees = site.data.site-data.nominees 
    | where:"PositionUniqueName",race %}
  {% assign sorted-nominees = these-nominees | sort: "Last_Name" %}

  <ul>
  {% for nominee in sorted-nominees %}
    <li>
    {% if nominee.Website -%}
    <a href="{{ nominee.Website}}">{{ nominee.Given_Names }} 
      {{ nominee.Last_Name }}</a>
    {%- else -%}
      {{ nominee.Given_Names}} {{ nominee.Last_Name }}
    {%- endif -%}
    </li>
  {% endfor %}
  </ul>

{% endfor %}


<h3>All positions</h3>

<ul>
{% for position in site.data.site-data.position-tags %}
  <li>{{ position.PositionUniqueName }} : {{ position.PositionDesc }}</li>
{% endfor %}
</ul>


<h3>All nominees</h3>

<ul>
{% for nominee in site.data.site-data.nominees %}
  <li>{{ nominee.Given_Names }} {{ nominee.Last_Name }} : 
  {{- nominee.PositionUniqueName }}</li>
{% endfor %}
</ul>