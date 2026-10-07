---
layout: archive
title: "CV"
permalink: /cv-json/
author_profile: false
redirect_from:
  - /resume-json
---

{% include base_path %}

<div class="cv-download-links">
  <a href="{{ base_path }}/files/CV.pdf" class="btn btn--primary">
    Download CV
  </a>

  <span id="cv-last-updated"
        style="margin-left: 12px; font-size: 0.9em; color: #666; font-style: italic;">
    Last updated: loading...
  </span>
</div>

<script>
document.addEventListener("DOMContentLoaded", function () {
  const repo = "{{ site.github.repository_nwo }}";
  const cvPath = "files/CV.pdf";
  const updatedElement = document.getElementById("cv-last-updated");

  fetch(`https://api.github.com/repos/${repo}/commits?path=${cvPath}&per_page=1`)
    .then(response => {
      if (!response.ok) {
        throw new Error("Unable to retrieve commit information.");
      }
      return response.json();
    })
    .then(data => {
      if (Array.isArray(data) && data.length > 0) {
        const date = new Date(data[0].commit.committer.date);

        const formattedDate = date.toLocaleDateString("en-US", {
          year: "numeric",
          month: "long",
          day: "numeric"
        });

        updatedElement.textContent = `Last updated: ${formattedDate}`;
      } else {
        updatedElement.style.display = "none";
      }
    })
    .catch(error => {
      console.error("Could not retrieve CV update date:", error);
      updatedElement.style.display = "none";
    });
});
</script>
