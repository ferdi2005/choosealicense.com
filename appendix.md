---
layout: default
permalink: /appendix/
title: Appendice
class: license-types
---

Per referenza, tutte le licenze descritte in questo sito riassunte in una tabella.

Se sei qui per scegliere una licenza, **[inizia dalla homepage](/)** per vedere le licenze che funzionano nella maggior parte dei casi.

<table border style="font-size: xx-small">
{% assign types = "permissions|conditions|limitations" | split: "|" %}
<tr>
  <th scope="col" style="text-align: center">License</th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% if seen_tags contains rule_obj.tag or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:rule_obj.tag }}{% endcapture %}
      <th scope="col" style="text-align: center; width:7%"><a href="#{{ rule_obj.tag }}">{{ rule_obj.label }}</a></th>
    {% endfor %}
  {% endfor %}
</tr>
{% assign licenses = site.licenses sort: "path" %}
{% for license in licenses %}
  <tr style="height: 3em"><th scope="row"><a href="{{ license.id }}">{{ license.title }}</a></th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% assign req = rule_obj.tag %}
      {% if seen_tags contains req  or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:req }}{% endcapture %}
      {% assign seen_req = false %}
      {% for t in types %}
        {% for r in license[t] %}
          {% if r contains req %}
            <td class="license-{{ t }}" style="text-align:center">
              <span class="{{ r }}">
                <span class="license-sprite {{ r }}"></span>
              </span>
            </td>
            {% assign seen_req = true %}
          {% endif %}
        {% endfor %}
      {% endfor %}
      {% unless seen_req %}
        <td></td>
      {% endunless %}
    {% endfor %}
  {% endfor %}
  </tr>
{% endfor %}
</table>

## Legenda

<p>Le licenze open source garantiscono al pubblico <span class="license-permissions"><span class="license-sprite"></span></span> <b>permessi</b> di fare con l'opera licenziata cose che le normali leggi sul copyright o sulla "proprietà intellettuale" non permettono.</p>

<p>La maggior parte di queste licenze garantiscono permessi soggetti a <span class="license-conditions"><span class="license-sprite"></span></span> <b>condizioni</b>.</p>

<p>Molte licenze hanno anche <span class="license-limitations"><span class="license-sprite"></span></span> <b>limitazioni</b> che solitamente scaricano garanzie e responsabilità, e qualche volte escludono diritti su brevetti e trademark.</p>

<dl>
{% assign seen_tags = '' %}
{% for type in types %}
  {% assign rules = site.data.rules[type] | sort: "label" %}
  {% for rule_obj in rules %}
    {% assign req = rule_obj.tag %}
    {% if seen_tags contains req %}
      {% continue %}
    {% endif %}
    <dt id="{{ req }}">{{ rule_obj.label }}</dt>
    {% capture seen_tags %}{{ seen_tags | append:req }}{% endcapture %}
    {% for t in types %}
      {% assign rs = site.data.rules[t] | sort: "label" %}
      {% for r in rs %}
        {% if r.tag == req %}
          <dd class="license-{{t}}"><span class="license-sprite"></span> {{ r.description }}</dd>
        {% endif %}
      {% endfor %}
    {% endfor %}
  {% endfor %}
{% endfor %}
</dl>
