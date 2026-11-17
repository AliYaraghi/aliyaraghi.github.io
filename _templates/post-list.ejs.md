<%
const tagBase = (templateParams && templateParams['tag-base']) || '';
const thumbnails = !!(templateParams && templateParams['thumbnails']);
%>
<% if (items.length > 0) { %>
::: {.list .post-list <%= thumbnails ? '.with-thumbs' : '' %>}
<% for (const item of items) { %>
::: {.post-item <%= metadataAttrs(item) %>}

<% if (thumbnails) { %>
```{=html}
<a class="post-thumb<%= item.image ? '' : ' is-empty' %>" href="<%- item.path %>" aria-hidden="true" tabindex="-1">
<% if (item.image) { %><img src="<%- item.image %>" alt="" loading="lazy"><% } else { %><i class="bi bi-journal-text"></i><% } %>
</a>
```
<% } %>

::: {.post-body}
<a class="post-title" href="<%- item.path %>"><%= item.title %></a>

<% if (item.description) { %>
::: {.post-desc}
<%= item.description %>
:::
<% } %>

```{=html}
<div class="post-meta">
<span class="listing-date"><%= item.date %></span>
<% if (item['reading-time']) { %><span aria-hidden="true">·</span><span><%= item['reading-time'] %></span><% } %>
<% for (const category of (item.categories || [])) { %><a class="post-tag" href="<%- tagBase %>#category=<%- encodeURIComponent(category) %>"><%= category %></a><% } %>
</div>
```
:::
:::
<% } %>
:::
<% } %>

<% if (items.length === 0) { %>
::: {.listing-empty}
No posts yet.
:::
<% } %>
