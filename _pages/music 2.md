---
layout: page
title: Music
permalink: /music/
description: A collection of music I've made. Please enjoy!
nav: true
---

Most of these are drawn from my <a href="https://soundcloud.com/jkernes">soundcloud</a> which has not been updated nearly as much as it should be. A few others however you won't find there, that I've released here. FYI I use soundloud embeddings, which may use cookies, which isn't really an issue, but... don't want to be sued.

Lastly, these were made while I was a poor grad student who couldn't afford FL studio, and was too afraid to torrent it. So... these were all made on my phone! Please excuse the quality :grin:

<h2>Top picks</h2>

This first track is a guitar beat and one of my favorites:
<iframe width="100%" height="100" scrolling="no" frameborder="yes" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/194054091&color=%233e2d2d&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true&visual=true"></iframe><div style="font-size: 10px; color: #cccccc;line-break: anywhere;word-break: normal;overflow: hidden;white-space: nowrap;text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif;font-weight: 100;"><a href="https://soundcloud.com/jkernes" title="JKernes" target="_blank" style="color: #cccccc; text-decoration: none;">JKernes</a> · <a href="https://soundcloud.com/jkernes/the_letter" title="the letter" target="_blank" style="color: #cccccc; text-decoration: none;">the letter</a></div>

This second track is based on a sample of *A change is gonna come* by Sam Cooke. It's old-school, and would sound right at home as a loading screen track for the NBA 2k1 videogame.

<iframe width="100%" height="100" scrolling="no" frameborder="yes" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/211091780&color=%233e2d2d&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true&visual=true"></iframe><div style="font-size: 10px; color: #cccccc;line-break: anywhere;word-break: normal;overflow: hidden;white-space: nowrap;text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif;font-weight: 100;"><a href="https://soundcloud.com/jkernes" title="JKernes" target="_blank" style="color: #cccccc; text-decoration: none;">JKernes</a> · <a href="https://soundcloud.com/jkernes/long-time-coming-1" title="Long time coming" target="_blank" style="color: #cccccc; text-decoration: none;">Long time coming</a></div>

<h2>By genre</h2>
<div class="projects grid">

  {% assign sorted_projects = site.music | sort: "importance" %}
  {% for project in sorted_projects %}
  <div class="grid-item">
    {% if project.redirect %}
    <a href="{{ project.redirect }}" target="_blank">
    {% else %}
    <a href="{{ project.url | relative_url }}">
    {% endif %}
      <div class="card hoverable">
        {% if project.img %}
        <img src="{{ project.img | relative_url }}" alt="project thumbnail">
        {% endif %}
        <div class="card-body">
          <h2 class="card-title">{{ project.title }}</h2>
          <p class="card-text">{{ project.description }}</p>
          <div class="row ml-1 mr-1 p-0">
            {% if project.github %}
            <div class="github-icon">
              <div class="icon" data-toggle="tooltip" title="Code Repository">
                <a href="{{ project.github }}" target="_blank"><i class="fab fa-github gh-icon"></i></a>
              </div>
              {% if project.github_stars %}
              <span class="stars" data-toggle="tooltip" title="GitHub Stars">
                <i class="fas fa-star"></i>
                <span id="{{ project.github_stars }}-stars"></span>
              </span>
              {% endif %}
            </div>
            {% endif %}
          </div>
        </div>
      </div>
    </a>
  </div>
{% endfor %}
</div>
