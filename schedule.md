---
layout: default
---

{% assign allpagetotal = 0 %}
{% assign allminutestotal = 0 %}

<!--<p>&#x24d8;=recommended supplemental</p>-->

<h3 style="text-align: center">Schedule</h3>

<table class="table table-striped"> 
  <tbody>
    <tr>
      <th style="text-align: center">Module</th>
      <th style="text-align: center">Week</th>
      <th class="col-xs-1">Topic</th>
      <th class="col-xs-5">Readings, Videos, Quizzes, Assignments, Logs, & Exams</th>
      <th class="col-xs-2">Complete prior to</th>
      <th class="col-xs-2">Totals (approximate)</th>
    </tr>
    {% for lecture in site.data.schedule %}
        {% assign pagetotal = 0 %}
        {% assign optionalpagetotal = 0 %}
        {% assign minutestotal = 0 %}
        {% assign optionalminutestotal = 0 %}
    <tr>
      <td style="text-align: center">{% if lecture.module %}Module {{ lecture.module }}{% endif %}</td>
      <td style="text-align: center">{% if lecture.week %}Week {{ lecture.week }}{% endif %}</td>
      <td>
        {% if lecture.slides %}<a href="{{ lecture.slides }}">{{ lecture.title }}</a>
        {% elsif lecture.title %}{{ lecture.title }}{% endif %}
      </td>
      <td>
        {% if lecture.reading %}
          <ul>
          {% for reading in lecture.reading %}
            <li>
            {% if reading.grad_level %}&#x2605;
            {% elsif reading.optional %}&#x24d8;
            {% else %}{% endif %}
            {% if reading.url %}
            <a href="{{ reading.url }}">{{ reading.title }}</a>
            {% else %}
            {{ reading.title }} 
            {% endif %}
            {% if reading.pages %}
            (p.&nbsp;{{ reading.pages }})
            {% elsif reading.times %}
            ({{ reading.times }})
            {% elsif reading.length and reading.length.unit and reading.length.value %}
            ({{ reading.length.value }} {{ reading.length.unit }})
            {% endif %}
            </li>
            {% if reading.length and reading.length.unit and reading.length.value %}
                {% if reading.length.unit == "pages" %}
                    {% if reading.optional %}
                        {% capture optionalpagetotal %}{{ optionalpagetotal | plus: reading.length.value }}{% endcapture %}
                    {% else %}
                        {% capture pagetotal %}{{ pagetotal | plus: reading.length.value }}{% endcapture %}
                        {% capture allpagetotal %}{{ allpagetotal | plus: reading.length.value }}{% endcapture %}
                    {% endif %}
                {% elsif reading.length.unit == "minutes" %}
                    {% if reading.optional %}
                        {% capture optionalminutestotal %}{{ optionalminutestotal | plus: reading.length.value }}{% endcapture %}
                    {% else %}
                        {% capture minutestotal %}{{ minutestotal | plus: reading.length.value }}{% endcapture %}
                        {% capture allminutestotal %}{{ allminutestotal | plus: reading.length.value }}{% endcapture %}
                    {% endif %}                
                {% endif %}
            {% endif %}
          {% endfor %}
          </ul>
        {% endif %}
      </td>
      <td>{{ lecture.date | date: "%a %b %d" }}<br/>{{ lecture.date | date: " %I %p %Z" }}</td>
      <td>
          <!--<ul class="fa-ul">-->
			  {% if pagetotal != 0 %}
			      <p>📖 {{ pagetotal }} pages</p>
			  {% endif %}
			  {% if minutestotal != 0 %}
			      <p>📺 {{ minutestotal }} minutes</p>
			  {% endif %}
			  {% if optionalpagetotal != 0 %}
			      <p>ℹ️ 📖 {{ optionalpagetotal }} pages</p>
			  {% endif %}
			  {% if optionalminutestotal != 0 %}
			      <p>ℹ️ 📺 {{ optionalminutestotal }} minutes</p>
			  {% endif %}
          <!--</ul>      -->
      </td>
    </tr>
    {% endfor %}

  </tbody>
</table>

<p>This course entails approximately {{ allpagetotal }} total pages of required readings and {{ allminutestotal | divided_by: 60 }} total hours of videos for the semester.</p>
