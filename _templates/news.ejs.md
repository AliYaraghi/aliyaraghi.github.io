::: {.list .news-list}
<% for (const item of items) { %>
::: {.news-item <%= metadataAttrs(item) %>}
[<%= item.date %>]{.news-date}

::: {.news-text}
<%= item.description %>
:::
:::
<% } %>
:::
