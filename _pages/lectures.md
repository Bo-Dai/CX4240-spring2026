---
layout: schedule
permalink: /lectures/
title: Lectures
---

{% assign current_module = 0 %}
{% assign skip_classes = 0 %}
{% assign prev_date = 0 %}

{% for item in site.data.lectures %}
{% if item.date %}
{% assign lecture = item %}
{% assign event_type = "upcoming" %}
{% assign today_date = "now" | date: "%s" | divided_by: 86400 %}
{% assign lecture_date = lecture.date | date: "%s" | divided_by: 86400 %}
{% if today_date > lecture_date %}
    {% assign event_type = "past" %}
{% elsif today_date <= lecture_date and today_date > prev_date %}
    {% assign event_type = "warning" %}
{% endif %}
{% assign prev_date = lecture_date %}

<tr class="{{ event_type }}">
    <th scope="row">{{ lecture.date }}</th>
    {% if lecture.title contains 'No Class' or forloop.last %}
    {% assign skip_classes = skip_classes | plus: 1 %}
    <td colspan="4" align="center">{{ lecture.title }}</td>
    {% else %}
    <td>
        Lecture #{{ forloop.index | minus: current_module | minus: skip_classes }}
        {% if lecture.lecturer %}({{ lecture.lecturer }}){% endif %}:
        <br />
        {{ lecture.title }}
        <br />
        {% if lecture.slides or lecture.notes or lecture.assignment or lecture.code_data or lecture.solution or lecture.makeup_solution or lecture.tex or lecture.reading or lecture.video%}
        [
            {% if lecture.slides %}
            <a href="{{ lecture.slides }}" target="_blank">slides</a>
            {% endif %}

            {% if lecture.slides and lecture.assignment %}
            |
            {% endif %}
            {% if lecture.assignment %}
            <a href="{{ lecture.assignment }}" target="_blank">assignment(pdf)</a>
            {% endif %}

            {% if lecture.assignment and lecture.tex %}
            |
            {% endif %}
            {% if lecture.tex %}
            <a href="{{ lecture.tex }}" target="_blank">assignment(tex)</a>
            {% endif %}

            {% if lecture.tex and lecture.solution %}
            |
            {% endif %}
            {% if lecture.solution %}
            <a href="{{ lecture.solution }}" target="_blank">solution</a>
            {% endif %}

            {% if lecture.solution and lecture.makeup_solution %}
            |
            {% endif %}
            {% if lecture.makeup_solution %}
            <a href="{{ lecture.makeup_solution }}" target="_blank">makeup solution</a>
            {% endif %}


            {% if lecture.solution and lecture.code_data %}
            |
            {% endif %}
            {% if lecture.code_data %}
            <a href="{{ lecture.code_data }}" target="_blank">code & data</a>
            {% endif %}

            {% if lecture.code_data and lecture.notes %}
            |
            {% endif %}
            {% if lecture.notes %}
            <a href="{{ lecture.notes }}" target="_blank">notes</a>
            {% endif %}

            {% if lecture.code_data and lecture.video %}
            |
            {% endif %}
            {% if lecture.video %}
            <a href="{{ lecture.video }}" target="_blank">recording</a>
            {% endif %}

            {% if lecture.video and lecture.reading %}
            |
            {% endif %}
            {% if lecture.reading %}
            <a href="{{ lecture.reading }}" target="_blank">reading</a>
            {% endif %}
            {% if lecture.reading and lecture.practice %}
            |
            {% endif %}
            {% if lecture.practice %}
            <a href="{{ lecture.practice }}" target="_blank">| midterm practice</a>
            {% endif %}
        ]
        {% endif %}
    </td>
<td>
  {% if lecture.readings %}
  <ul>
    {% for reading in lecture.readings %}
      <li>
        {% if reading.url %}
          <a href="{{ reading.url }}" target="_blank">{{ reading.text }}</a>
        {% else %}
          {{ reading.text }}
        {% endif %}
      </li>
    {% endfor %}
  </ul>
  {% endif %}
</td>
    <td>
        <p>{{ lecture.logistics }}</p>
    </td>
    {% endif %}
</tr>
{% else %}
{% assign current_module = current_module | plus: 1 %}
{% assign module = item %}
<tr class="info">
    <td colspan="5" align="center"><strong>{{ module.title }}</strong></td>
</tr>
{% endif %}
{% endfor %}
