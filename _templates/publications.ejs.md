<%
const params = templateParams || {};
const selectedOnly = !!params['selected-only'];

const pubs = (selectedOnly ? items.filter((p) => p.selected) : items.slice())
  .sort((a, b) => Number(b.year || 0) - Number(a.year || 0));

const esc = (s) => String(s ?? '')
  .replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;');

const me = /(Ali Yaraghi|A\. Yaraghi|Yaraghi, A\.?|Yaraghi, Ali)/g;
const authors = (s) => esc(s).replace(me, '<span class="me">$1</span>');

const link = (href, label) => href ? `<a href="${esc(href)}">${label}</a>` : '';
let lastYear = null;
%>

<% if (pubs.length > 0 && selectedOnly) { %>
<h2 class="anchored" id="selected-publications-heading">selected publications</h2>
<% } %>

<% if (pubs.length > 0) { %>
::: {.list .pub-list}
<% for (const [i, item] of pubs.entries()) { %>
<% if (!selectedOnly && item.year && item.year !== lastYear) { lastYear = item.year; %>
<h2 class="pub-year anchored"><%= esc(item.year) %></h2>
<% } %>

```{=html}
<div class="pub-item">
<div class="pub-badge-col"><% if (item.abbr) { %><span class="pub-badge"><%= esc(item.abbr) %></span><% } %></div>
<div class="pub-body">
<div class="pub-title"><%= item.url ? link(item.url, esc(item.title)) : esc(item.title) %></div>
<% if (item.authors) { %><div class="pub-authors"><%= authors(item.authors) %></div><% } %>
<% if (item.venue) { %><div class="pub-venue"><%= esc(item.venue) %><%= item.year ? `, ${esc(item.year)}` : '' %></div><% } %>
<div class="pub-links">
<% if (item.abstract) { %><button type="button" aria-expanded="false" aria-controls="pub-abs-<%= i %>" onclick="const t=document.getElementById(this.getAttribute('aria-controls'));t.hidden=!t.hidden;this.setAttribute('aria-expanded',String(!t.hidden));">Abstract</button><% } %>
<%= link(item.pdf, 'PDF') %>
<%= link(item.code, 'Code') %>
<%= link(item.slides, 'Slides') %>
<%= link(item.poster, 'Poster') %>
<% if (item.bibtex) { %><button type="button" aria-expanded="false" aria-controls="pub-bib-<%= i %>" onclick="const t=document.getElementById(this.getAttribute('aria-controls'));t.hidden=!t.hidden;this.setAttribute('aria-expanded',String(!t.hidden));">BibTeX</button><% } %>
</div>
<% if (item.abstract) { %><div class="pub-abstract" id="pub-abs-<%= i %>" hidden><%= esc(item.abstract) %></div><% } %>
<% if (item.bibtex) { %><div class="pub-bib" id="pub-bib-<%= i %>" hidden><pre><%= esc(item.bibtex) %></pre></div><% } %>
</div>
</div>
```

<% } %>
:::
<% } %>

<% if (pubs.length === 0 && !selectedOnly) { %>
::: {.pub-empty}
No publications yet.
:::
<% } %>
