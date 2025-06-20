---
title: Publications
layout: page
---
<div id="pubs" class="pure-g">
  <div id="content" class="pure-u-1 pure-u-md-3-4">
    <h1 class="title">Publications</h1>

    {% assign pubYears = site.pubs | group_by:"year" | sort: "name" | reverse %}
    {% for year in pubYears %}

        <!-- Our "Selected Prior Publications" are from 2021 and earlier -->
        <!--{% if year.name == "2021" %}
        <h1 class="title">Selected Prior Publications</h1>
        {% endif %}-->


        <div id="year-{{year.name}}" class="year pure-g">
          <div class="pure-u-1-3 pure-u-md-1-5"></div>
            <div class="pure-u-2-3 pure-u-md-4-5">
              <h2>{{year.name}}</h2>
            </div>
        </div>


      {% assign pubs = year.items | sort: 'date' | reverse %}
      {% for pub in pubs %}
        {% assign url = pub.external_url | default: pub.url | relative_url | replace: 'index.html', '' %}
        <div id="{{pub.slug}}" class="pub pure-g" data-pub='{{ pub | jsonify_pub }}'>

          <div class="thumbnail pure-u-1-3 pure-u-md-1-5">
          <a href="{{ url }}">

            {%- comment -%}── build the two candidate paths ──{%- endcomment -%}
            {% assign thumb_src  = "/imgs/thumbs/"  | append: pub.slug | append: ".png" %}
            {% assign teaser_src = "/imgs/teasers/" | append: pub.slug | append: ".png" %}

            {%- comment -%}── does the thumb really exist? ──{%- endcomment -%}
            {% assign thumb_file = site.static_files | where: "path", thumb_src | first %}

            <img src="{% if thumb_file %}{{ thumb_src }}{% else %}{{ teaser_src }}{% endif %}" alt="" />

          </a>
        </div>

          <div class="pure-u-2-3 pure-u-md-4-5">
            <h3><a href="{{url}}">{{pub.title}}</a></h3>
            <p class="authors">
            {% for author in pub.authors %}
              {% assign person = site.data.people[author.key] %}
              {% assign name = author.name | default:person.name %}
              {% if person.url %}
                <a href="{{person.url}}">{{name}}</a>{% if author.equal %}*{% endif %}{% unless forloop.last %}, {% endunless %}
              {% else %}
                {{name}}{% if author.equal %}*{% endif %}{% unless forloop.last %}, {% endunless %}
              {% endif %}
            {% endfor %}
            </p>
            <p class="venue">
              {% if pub.preprint %}
                {{pub.preprint.server}}: {{pub.preprint.id}}
              {% else %}
                {{site.data.venues[pub.venue].full}} ({{site.data.venues[pub.venue].short}}), {{pub.year}}
              {% endif %}
            </p>
            {% if pub.award %}
              <p class="award">
                <i class="fas fa-award"></i> {{pub.award}}
              </p>
            {% endif %}
            <p class="links">
              {% if pub.external_url %}
                <a href="{{pub.external_url}}">
                  {% if pub.preprint %}Preprint{% else %}Article{% endif %}
                </a>
              {% else %}
              {% if pub.has_pdf %}
                <a href="/pubs/{{pub.slug}}.pdf">PDF</a>&middot; 
              {% endif %}
              {% endif %}
              {% for material in pub.materials %}
                <a href="{{material.url}}">{{material.name}}</a>
              {% endfor %}
            </p>
          </div>
        </div>
      {% endfor %}
    {% endfor %}
  </div> 
  
  <div id="sidebar" class="pure-u-1 pure-u-md-1-4">
    <h4>Search</h4>
    <input type="text" id="search" placeholder="Search title, abstract, or authors...">

    <!--<h4>Publication Type</h4>
    <div id="types">
      {% assign types = site.pubs | map: 'type' | uniq %}
      {% for type in types %}
        <a id="type-{{type}}" class="tag" data-tag="{{type}}">{{type | capitalize}} <span>()</span></a>
      {% endfor %}
    </div>-->

    <h4>Tags</h4>
    <div id="tags">
      {% for group in site.data.tags %}
        <p>
          {% for tag in group %}
            <a id="tag-{{tag | replace: ' ', '-'}}" class="tag" data-tag="{{tag}}">
              {% assign words = tag | split: ' ' %}
              {% for word in words %}
                {% if word == 'human-ai' %}
                  Human-AI
                {% else %}
                  {{word | capitalize}}
                {% endif %}
              {% endfor %}
              <span>()</span>
            </a>
          {% endfor %}
        </p>
      {% endfor %}
    </div>
  </div>  
</div>