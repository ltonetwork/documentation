---
description: REST API of the Event Chain service
---

# Event Chain service

{% swagger baseUrl="https://lto.example.com" path="/event-chains/" method="get" summary="Get a list of your event chains" %}
{% swagger-description %}
The service uses the public key of the HTTP signature to find all event chains that have an identity with a matching public key.
{% endswagger-description %}

{% swagger-response status="200" description="" %}
```javascript
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://lto.example.com" path="/event-chains/" method="post" summary="Create an event chain or append events" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://lto.example.com" path="/event-chains/{id}" method="get" summary="Get an event chain" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="string" %}
Event chain identifier
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
    
```
{% endswagger-response %}
{% endswagger %}

