# Data instructions

Data instructions allow you to dynamically set properties of a state or actions when they are instantiated using data
from the projected process.

### Schemas

[JSON Schema](schema.json) | [changelog](changelog.md)

_Please checkout the [JSON Schema](schema.json) for more information. Documentation will be added._

### Example

```json
{
    "foo": {
        "bar": {
            "qux": 12345
        },
        "term": "data enrichment",
        "city": "Amsterdam",
        "country": "Netherlands"
    },
    "amount": {
        "<ref>": "foo.bar.qux"
    },
    "message": {
        "<tpl>": "I want to go to {{ foo.city }}, {{ foo.country }}"
    },
    "shipping": {
        "<switch>": {
            "on": { "<ref>": "foo.country" },
            "options": {
                "USA": "UPS",
                "Netherlands": "PostNL"
            },
            "default": "DHL"
        }
    },
    "user": {
        "<src>": "https://api.example.com/users/9870"
    },
    "search_results": {
        "<apply>": {
            "query": "RelatedTopics[].{url: FirstURL, description: Text}",
            "input": {
                "<src>": {
                    "<tpl>": "http://api.duckduckgo.com/?q={{ foo.term }}&format=json"
                }
            }
        }
    },
    "profile": {
        "<merge>": [
            {
                "<ref>": "foo.bar"
            },
            {
                "<hash>": {
                    "algo": "md5",
                    "input": "foo"
                }
            },
            {
                "<src>": "https://api.example.com/zoo/99"
            },
            {
                "apples": 100,
                "pears": 220
            }
        ]
    }
}
```

