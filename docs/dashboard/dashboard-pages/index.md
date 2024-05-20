---
title: Dashboard Pages
date: 2024-05-20
updatedDate: 2024-05-20
menuWeight: 1
pageFullWidth: true
hideToc: true
pages:
  - title: Get Started
    slug: /docs/dashboard/dashboard-pages/get-started
    icon: list-check
  - title: Dashboard
    slug: /docs/dashboard/dashboard-pages/dashboard
    icon: qrcode
  - title: Endpoints
    slug: /docs/dashboard/dashboard-pages/endpoints
    icon: swap
  - title: Outbound Integrations
    slug: /docs/dashboard/dashboard-pages/outbound-integrations
    icon: arrows-turn-right
  - title: Changes & Errors
    slug: /docs/dashboard/dashboard-pages/changes-errors
    icon: bug
  - title: API Log Explorer
    slug: /docs/dashboard/dashboard-pages/api-log-explorer
    icon: list-tree
  - title: Documentation
    slug: /docs/dashboard/dashboard-pages/documentation
    icon: brackets-curly
  - title: Reports
    slug: /docs/dashboard/dashboard-pages/reports
    icon: chart-simple
---

# Dashboard Pages

In these guides, we will show you how to effectively navigate through the APItoolkit dashboard and maximize all the powerful features accessible on different pages of the dashboard. Kindly click on any of the cards below to get started.

```=html
<div class="mt-8 grid grid-cols-1 md:grid-cols-4 lg:grid-cols-4 gap-8 text-center">
{% for page in this.frontmatter.pages %}
  <a href="{{ page.slug }}" class="docs-card-2 rounded-md">
    <i class="fa-regular fa-{{ page.icon }} h-5 w-5 mr-2"></i>
    <p class="text-sm font-bold text-black dark:text-white">{{ page.title }}</p>
  </a>
{% endfor %}
</div>
```

