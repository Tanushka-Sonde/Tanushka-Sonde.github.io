---
layout: about
title: About
permalink: /
subtitle: B.Tech Mechanical Engineering (Minor in AI) &middot; IIT Gandhinagar

profile:
  align: right
  image: prof_pic.jpg
  image_circular: false
  more_info: >
    <p>Indian Institute of Technology Gandhinagar</p>
    <p>Gujarat, India</p>

selected_papers: false # no publications yet
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false # News is included in the page body below, so it sits above Projects
  scrollable: false
  limit: 5

latest_posts:
  enabled: false
---

Third-year B.Tech student in Mechanical Engineering (Minor in AI) at IIT Gandhinagar, working on applied ML: computer vision and LLM systems.

- **Research:** camera-controlled scene generation from sketches, Computer Vision Lab, IITGN (Prof. Shanmuganathan Raman)
- **Internships (summer 2026):** agentic multimodal RAG assistant at EY GDS; local RAG policy assistant at Deloitte India
- **Built from scratch:** a vLLM-style paged KV cache inference engine and a quant research and backtesting framework
- **Robotics:** vision-based arm teleoperation, and a sim-to-real pipeline for OpenVLA on a Franka FR3 (ROS 2, MuJoCo)
- **Open to:** research and engineering internships. Reach me by [email](mailto:tanushka.sonde@iitgn.ac.in)

## News

{% include news.liquid limit=true %}

## Projects

{% assign selected = "sketchworld,agentic-multimodal-rag,paged-kv-cache-inference-engine,sift-optical-flow-lunar-descent" | split: "," %}

<div class="projects">
  <div class="row row-cols-1 row-cols-md-2">
    {% for slug in selected %}
      {% assign project = site.projects | where_exp: "p", "p.slug == slug" | first %}
      {% if project %}
        {% include projects.liquid %}
      {% endif %}
    {% endfor %}
  </div>
</div>

[See all projects &rarr;](/projects/)
