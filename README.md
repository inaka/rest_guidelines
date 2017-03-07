# REST API Design Guidelines

## Introduction
What you will find in this repository is a list of guidelines we follow at [Inaka](http://inaka.net) when designing our REST APIs.
These guidelines are not taxative, some exceptions can be found here or there, but we do base our tools (like [Jayme](http://github.com/inaka/Jayme), [Dayron](http://github.com/inaka/Dayron) and [SumoREST](http://github.com/inaka/sumo_rest)) on them. That's why, if you're going to use one of those tools, it would be much simpler to have a server that complies to the following guides.

## Contact Us

For **questions** or **general comments** regarding the use of this repository, please use our public
[hipchat room](http://inaka.net/hipchat).

And you can check all of our open-source projects at [inaka.github.io](http://inaka.github.io)



## Conventions & Rules

#### Use snake_case with JSON
> When the endpoint returns a JSON, the keys should always be in snake_case, unless you have a super edge case.


##### Examples
###### Good

      {
      "caption": "string",
      "comment_count": 0,
      "created_at": "2017-03-07T18:57:44.622Z",
      "id": "string",
      "like_count": 0,
      "media_type": "image",
      "owner": "string"
    }

###### Bad

      {
      "caption": "string",
      "commentCount": 0,
      "createdAt": "2017-03-07T18:57:44.622Z",
      "id": "string",
      "likeCount": 0,
      "mediaType": "image",
      "owner": "string"
    }

##### Reasoning
Even though in some programming languages is common to name variables or object instances with camelCase names, we consider the snake_case is the most readable and understandable way to named the key fields on a JSON response

---

#### Don't use response envelopes by default
> When the endpoint returns, for example, an array, it should be returned as a plain array, not a dictionary with the array as a value.


##### Examples
###### Good

    [
      {
      "like_count": 0,
      "id": "string",
      "media_type": "comment",
      "owner": "string"
      },
      {
        "like_count": 0,
        "id": "string",
        "media_type": "comment",
        "owner": "string"
      },
      {
        "like_count": 0,
        "id": "string",
        "media_type": "comment",
        "owner": "string"
      }
    ]

###### Bad

    { "items" : [
        {
          "like_count": 0,
          "id": "string",
          "media_type": "comment",
          "owner": "string"
        },
        {
          "like_count": 0,
          "id": "string",
          "media_type": "comment",
          "owner": "string"
        },
        {
          "like_count": 0,
          "id": "string",
          "media_type": "comment",
          "owner": "string"
        }
      ]
    }

##### Reasoning
If the client expects an array it could ( and would ) be confusing to receive a dictionary with one key containing the array as a value. 

***

~~~

~~~

~~~

~~~