## Tools
| Tool                  | URL                                                                                |
| --------------------- | ---------------------------------------------------------------------------------- |
| Time String generator | https://www.strfti.me/                                                             |
| Sandbox               | https://jumpseller.com/support/liquid-sandbox/                                     |
| Documentation Braze   | https://www.braze.com/docs/user_guide/messaging/design_and_edit/personalize/liquid |
| Documentation Shopify | https://shopify.github.io/liquid/                                                  |

## Variable Assignments
`{% assign variable_name = value %}`
`{% capture variable_name %}value{% endcapture %}`

## Control Flow
### Operators
| Operator | Meaning                                                                      |
| -------- | ---------------------------------------------------------------------------- |
| `==`     | equals                                                                       |
| `!=`     | does not equal                                                               |
| `>`      | greater than                                                                 |
| `<`      | less than                                                                    |
| `>=`     | greater than or equal to                                                     |
| `<=`     | less than or equal to                                                        |
| `or`     | logical or                                                                   |
| `and`    | logical and                                                                  |
| contains | returns true when a substring inside a string is found (works on arrays too) |
### IF/Else Block
```liquid
{% if ${first_name} != blank %}
	Hello {{${first_name}}}
{% else %}
	Hello there
{% endif %}
```
### Elsif / Else Block
```liquid
{% if ${language} == 'es' %}
	¡Gracias por descargar nuestra aplicación!
{% elsif  ${language} == 'fr' %}
	Merci d'avoir téléchargé notre application!
{% else %}
	Thanks for downloading our app!
{% endif %}
```
### Unless Block
```liquid
{% unless ${first_name} == blank %}
	Hello {{${first_name}}}, thanks for downloading our app!
{% endunless %}
```
### Case/When Block
```liquid
{% case {{custom_attribute.${fav_color}}} %}
{% when "aqua" %}
	{% assign color = "#00FFFF" %}
{% when "green", "Cadmium_Green", "Grass" %}
	{% assign color = "#097969" %}
{% else %}
	{% assign color = "#8B4000" %}
{% endcase %}
<div style="background-color: {{color}}">
	<p>the lazy dog jumped over the quick brown fox</p>
</div>
```
## Iteration
### Basic Loop
```liquid
{% for flavor in {{custom_attribute.${flavors}}} %}
	<p> The flavor of the week is: {{flavor}}</p>
{% endfor %}
```
### Loop with array of objects
```liquid
{% assign orders = {{custom_attribute.${orders}}} %}
{% assign total = 0 %}
{% for order in orders %}
	{% assign total = total | plus: order.cost %}
	<h3>Order # {{order.id}}</h3>
	<ul>
		<li>{{ order.flavor }}</li>
		<li>{{ order.size }}</li>
		<li><b>{{ order.sause}}</li>
		<li>{% if order.side == true %}Yes!{% else %}No!{% endif %}</li>
		<li>${{order.cost}}</li>
	</ul>
	<hr>
{% endfor %}
<h3>Total: ${{total}}</h3>
```
### Using Else
```liquid
{% assign orders = {{custom_attribute.${orders}}} %}
{% assign total = 0 %}
{% for order in orders %}
	{% assign total = total | plus: order.cost %}
	<h3>{{order.id}}</h3>
	<ul>
		<li>{{ order.flavor }}</li>
		<li>{{ order.size }}</li>
		<li>{{ order.sause}}</li>
		<li>{% if order.side == true %}Yes!{% else %}No!{% endif %}</b></li>
		<li>${{order.cost}}</li>
	</ul>
{% else %}
	<p> Something went wrong and your order was not completed. Please call us at number-here.
<hr>
{% endfor %}
<h3>Total: ${{total}}</h3>
```
### Using Break
```liquid
{% for flavor in {{custom_attribute.${flavors}}} %}
{% if flavor == 'cheddar' %}
    {% break%}
{% else %}
    <p> The flavor of the week is: {{flavor}}</p>
{% endif %}
{% else %}
    <p>Flavor of the week promotion is not available at the moment </p>
{% endfor %}
```
### Using Continue
```liquid
{% for flavor in {{custom_attribute.${flavors}}} %}
{% if flavor == 'cheddar' or flavor == 'buffalo' %}
    {% continue %}
{% else %}
    <p> The flavor of the week is: {{flavor}}</p>
{% endif %}
{% else %}
    <p>Flavor of the week promotion is not available at the moment </p>
{% endfor %}
```
### Using parameters
```liquid
{% for flavor in {{custom_attribute.${flavors}}} limit: 5 %}
    <p> The flavor of the week is: {{flavor}}</p>
{% else %}
    <p>Flavor of the week promotion is not available at the moment </p>
{% endfor %}

{% for flavor in {{custom_attribute.${flavors}}} offset: 4 %}
    <p> The flavor of the week is: {{flavor}}</p>
{% else %}
    <p>Flavor of the week promotion is not available at the moment </p>
{% endfor %}

{% for i in (3..5) %}
  {{ i }}
{% endfor %}

{% for flavor in {{custom_attribute.${flavors}}} %}
  {% if forloop.length > 0 %}
    {{ flavor }}{% unless forloop.last %},{% endunless -%}
  {%- endif -%}
{% endfor %}
```

### Reversing & Sorting an array
```liquid
{% assign flavors_sorted = {{custom_attribute.${flavors} | sort }} %}

<h1>Unsorted Array:</h1>

{% for flavor in {{custom_attribute.${flavors}}} %}
	<ul>
		<li>Flavor: {{flavor}} </li>
	</ul>
{% endfor %}
<hr>

<h1>Sorted array:</h1>
{% for flavor in flavors_sorted %}
	<ul>
		<li>Flavor: {{flavor}} </li>
	</ul>
{% endfor %}
<hr>

<h1>Sorted Reversed array:</h1>
{% for flavor in flavors_sorted reversed %}
	<ul>
		<li>Flavor: {{flavor}} </li>
	</ul>
{% endfor %}
```

## Abort tag
```liquid
{% if ${language} == 'en' %}
	Send this message in English!
{% else %}
	{% abort_message('lang was not en') %}
{% endif %}
```
## Dates
```liquid
{% assign iso_date = 'now' | date: '%Y-%m-%d %H:%M:%S.%6N %Z'%}
```

## Easy subject line
```liquid
{% liquid
echo TEST campaign.${name} | default: 'Braze Support Testing' | append: ' '
echo 'now' | date: '%Y-%m-%d %H:%M:%S.%6N %Z'
%}
```
## Whitespace control
```liquid
{{content_blocks.${subjectLine}}}
{{- subject_line | strip | strip_newlines -}}
```
## Context, API, Events
```liquid
{{context.${example_variable_name}}}
{{event_properties.${your_custom_event_property}}}
{{api_trigger_properties.${your_api_trigger_property}}}
```
## Connected content
```json
{
  "movies": [
    {
      "name": "Inception",
      "description": "A skilled thief is given a chance at redemption if he can successfully perform inception.",
      "dvd_image_url": "https://m.media-amazon.com/images/I/71uKM+LdgFL.jpg"
    },
    {
      "name": "The Matrix",
      "description": "A computer hacker learns about the true nature of his reality and his role in the war against its controllers.",
      "dvd_image_url": "https://m.media-amazon.com/images/I/71PfZFFz9yL._AC_UF894,1000_QL80_.jpg"
    },
    {
      "name": "Interstellar",
      "description": "A team of explorers travel through a wormhole in space in an attempt to ensure humanity's survival.",
      "dvd_image_url": "https://m.media-amazon.com/images/I/514zBLkyJcL._AC_UF894,1000_QL80_.jpg"
    }
  ]
}
```
### CC Example Object
```liquid
{% connected_content https://webhook.site/64a710c8-8fe8-4a40-ab9d-eab23388ad41 :save movies_payload %}

{% assign movies = movies_payload.movies %}

<table>
  <thead>
    <tr>
      <th>Movie Name</th>
      <th>Description</th>
      <th>DVD Image</th>
    </tr>
  </thead>
  <tbody>
    {% for movie in movies %}
    <tr>
      <td>{{ movie.name }}</td>
      <td width="400">{{ movie.description }}</td>
      <td><img width="70" height="100" src="{{ movie.dvd_image_url }}" 
      alt="{{ movie.name }} DVD Cover" /></td>
    </tr>
    {% endfor %}
  </tbody>
</table>
```
## Catalogs Example
```liquid
{% catalog_items Games 1234 %}
{% if items[0].on_sale == true %}
  {{ items[0].title }} is on sale! Get it for {{ items[0].price }}.
{% else %}
  Check out {{ items[0].title }} at full price.
{% endif %}
```
## Catalogs Selection
```liquid
{% catalog_selection_items item-list selections %} 
{% if items[0].venue_name.size > 10 %}
Message if the venue name's size is more than 10 characters. 
{% elsif items[0].venue_name.size <= 10 %}
Message if the venue name's size is 10 characters or fewer. 
{% else %} 
{% abort_message('no venue_name') %} 
{% endif %}
```

