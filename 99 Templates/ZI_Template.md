---
title: "{{title}}"
{% if date %}year: {{date | format("YYYY")}}{% endif %}
authors: {{authors}}{{directors}}
tags: 
  - zotero 
---

{{ bibliography | replace("\n", " ") }}

- zotero: [{{desktopURI}}]({{desktopURI}})
- url: {{url}}
{% if pdfLink -%}
- pdf: {{pdfLink}}
{%- endif -%}


{% if abstractNote %}

# Abstract
{{ abstractNote }}
{% endif %}

# Highlights

{% for annotation in annotations %}
{% if annotation.annotatedText %}
> [!cite]
> {{annotation.annotatedText}}
> [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}})
{%- if annotation.comment %} {# comment along with highlighted text #}
> > [!note]
> > {{annotation.comment}}
{% endif %}
{% else %}
{% if annotation.comment %} {# standalone comment, no text! #}
> [!note]
> {{annotation.comment}}
> [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}})
{% endif -%}
{% endif -%}
{% if annotation.imageRelativePath %}
![[{{annotation.imageRelativePath}}]]
{% endif -%}
{% endfor %}

# Notes
{% persist "notes" %}
{% endpersist %}
