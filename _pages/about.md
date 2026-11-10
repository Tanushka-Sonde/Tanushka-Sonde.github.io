---
layout: about
title: about
permalink: /
subtitle: B.Tech Mechanical Engineering (Minor in AI) &middot; IIT Gandhinagar

profile:
  align: right
  # TODO: add your photo at assets/img/prof_pic.jpg, then change the line below to `image: prof_pic.jpg`
  image: prof_pic_placeholder.svg
  image_circular: false
  more_info: >
    <p>Indian Institute of Technology Gandhinagar</p>
    <p>Gujarat, India</p>

selected_papers: false # no publications yet
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true
  scrollable: false
  limit: 5

latest_posts:
  enabled: false
---

I'm a third-year B.Tech student in Mechanical Engineering at IIT Gandhinagar, minoring in Artificial Intelligence. I work on applied machine learning, mostly computer vision and LLM systems, and I like building things from first principles to understand how they work.

I'm currently doing research at the Computer Vision Lab, IIT Gandhinagar, with Prof. Shanmuganathan Raman on camera-controlled scene generation from sketches. In summer 2026 I interned at EY GDS, building an agentic multimodal RAG knowledge assistant, and at Deloitte India, building a proof-of-concept local RAG policy assistant.

Outside of that, I've built a vLLM-style paged KV cache inference engine from scratch, a quant research and backtesting framework, and several computer vision pipelines. Before moving into ML, I worked on robot manipulation with ROS 2 and MuJoCo.

I'm open to research and engineering internship opportunities. The best way to reach me is [email](mailto:tanushka.sonde@iitgn.ac.in).

## selected projects

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
