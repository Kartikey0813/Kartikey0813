# Technical Analyst GitHub README

## 🚀 About Me

I am a Technical Support / Technical Analyst focused on bridging customer needs with reliable technical solutions. I specialize in troubleshooting, API debugging, database diagnostics (MySQL/MongoDB), observability, and incident resolution. My GitHub serves as both a technical portfolio and a living record of continuous learning and contribution.

## 📊 GitHub Contribution Insights

This section visualizes contribution activity over time to reflect consistency, growth, and focus areas.

### Contribution Graphs

* **Weekly, Monthly, and Yearly Graphs**: These display commit frequency, issue activity, pull requests, and repository contributions aggregated over time.
* **Languages & Project Distribution**: Breakdown of activity by language, repository, and type (code vs. docs vs. automation).

### How the Graphs are Generated

You can generate the graphs locally or in CI using the included script (`scripts/contrib_stats.py`). It pulls your GitHub activity via the REST API, aggregates it, and renders:

* Commits per week (last 52 weeks)
* Commits per month (last 12 months)
* Commits per year (multi-year if applicable)
* Issue and PR activity over the same intervals

#### Example Python Usage (skeleton):

```python
import os
import requests
import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime

GITHUB_TOKEN = os.getenv('GITHUB_TOKEN')  # set this in environment for authenticated rate limits
USERNAME = 'your-github-username'
HEADERS = {'Authorization': f'token {GITHUB_TOKEN}'}

# Fetch events (paginated) and extract dates
def fetch_events(page=1):
    url = f'https://api.github.com/users/{USERNAME}/events/public?page={page}&per_page=100'
    resp = requests.get(url, headers=HEADERS)
    resp.raise_for_status()
    return resp.json()

# Aggregate event dates
all_dates = []
for page in range(1, 6):  # adjust upper bound if necessary
    events = fetch_events(page)
    if not events:
        break
    for e in events:
        dt = datetime.fromisoformat(e['created_at'].replace('Z', '+00:00'))
        all_dates.append(dt)

# Build DataFrame
df = pd.DataFrame({'created_at': all_dates})
df.set_index('created_at', inplace=True)

# Weekly aggregation
weekly = df.resample('W').size()
monthly = df.resample('M').size()
yearly = df.resample('Y').size()

# Plotting (example for weekly)
plt.figure()
weekly.plot(title='Weekly GitHub Activity')
plt.xlabel('Week')
plt.ylabel('Event Count')
plt.tight_layout()
plt.savefig('graphs/weekly_activity.png')
```

You can extend this to include:

* Filtering by event type (PushEvent, PullRequestEvent, IssuesEvent)
* Language detection per repository via the GitHub Languages API
* Normalizing by working days vs. weekend spikes

## 🧰 Repository & Project Portfolio Website

Showcase your projects dynamically via a portfolio site. Recommended setup:

### Features

* Auto-populate GitHub projects using GitHub API
* Display key metrics per project: stars, forks, last updated, primary language
* Highlight featured repositories (e.g., support tools, dashboards, automation scripts)
* Embed contribution graphs
* Responsive layout, deployed via **GitHub Pages**

### Example `index.html` snippet (static starter):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Technical Analyst Portfolio</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/tailwindcss@latest/dist/tailwind.min.css" />
</head>
<body class="bg-gray-50 font-sans">
  <div class="max-w-5xl mx-auto p-6">
    <h1 class="text-4xl font-bold mb-2">Technical Analyst Portfolio</h1>
    <p class="mb-6">Live GitHub activity and featured projects.</p>
    <section id="contributions">
      <h2 class="text-2xl font-semibold">Contribution Overview</h2>
      <img src="graphs/weekly_activity.png" alt="Weekly Activity" class="my-4 rounded shadow" />
      <!-- Add other graphs similarly -->
    </section>
    <section id="projects">
      <h2 class="text-2xl font-semibold mt-8">Featured Projects</h2>
      <div id="repo-list" class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-4">
        <!-- JS will populate this -->
      </div>
    </section>
  </div>
  <script>
    const username = 'your-github-username';
    fetch(`https://api.github.com/users/${username}/repos?per_page=100&sort=updated`)
      .then(res => res.json())
      .then(repos => {
        const container = document.getElementById('repo-list');
        repos.slice(0, 6).forEach(r => { // top 6
          const card = document.createElement('div');
          card.className = 'border rounded p-4 bg-white shadow';
          card.innerHTML = `
            <h3 class='text-xl font-medium'><a href='${r.html_url}' target='_blank'>${r.name}</a></h3>
            <p class='text-sm text-gray-600'>${r.description || ''}</p>
            <div class='mt-2 flex gap-3 text-xs'>
              <span>⭐ ${r.stargazers_count}</span>
              <span>🍴 ${r.forks_count}</span>
              <span>🛠 ${r.language || '—'}</span>
            </div>
          `;
          container.appendChild(card);
        });
      });
  </script>
</body>
</html>
```

### Deployment

1. Push the repository to GitHub with the portfolio in `index.html`.
2. Enable **GitHub Pages** from repository settings (use `main` branch or `/docs` folder).
3. Ensure `graphs/` and any generated images are committed or generated at build time (you can automate updates with GitHub Actions).

## 📈 Automation Ideas

* GitHub Action to refresh contribution graphs daily and commit updated images.
* CI step that fetches latest repo metadata and regenerates a summary JSON consumed by the portfolio page.
* Slack or email notification when a new significant contribution happens (e.g., high-star project update).

## 📝 Interview & Branding Tips

* Embed this README link in your interview portfolio or resume.
* Talk through one or two projects and show your contribution graphs as evidence of consistency.
* Describe how you built the portfolio site, the data pipeline for graphs, and how you use it to monitor your technical growth.

## 📫 Contact

* GitHub: `https://github.com/your-github-username`
* Email: [your.email@example.com](mailto:your.email@example.com)
* Portfolio: `https://your-github-username.github.io/`

---

*Replace placeholder usernames, tokens, and paths with your real values. Keep your GitHub token scoped minimally (read-only) and stored securely (e.g., in environment variables).*
