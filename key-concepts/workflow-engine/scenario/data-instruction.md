# Data instruction

Data instructions allow you to dynamically set properties of a state or actions when they are instantiated using data from the projected process.

* `<ref>` - Resolve a reference to another part of the document using a dot key path
* `<ifset>` - Checks if a reference is null. If so, replace the object by null.
* `<switch>` - Choose one of the child properties based on a property in the document
* `<merge>` - Merge a set of objects
* `<enrich>` - Enrich an object with extra data by matching properties
* `<tpl>` - Parse text as [Mustache](https://mustache.github.io/) template
* `<apply>` - Project an object using the [JMESPath](http://jmespath.org/) query language
* `<dateformat>` - Takes a date and a format \(defaults to `Y-m-d`\) and formats the accordingly. Optionally you can set the timezone if you wish to output timezone information.

## Example

```javascript
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
            "projection": "RelatedTopics[].{url: FirstURL, description: Text}",
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
                "apples": 100,
                "pears": 220
            }
        ]
    }
}
```

