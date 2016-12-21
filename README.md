# REST API Design Guidelines

## Introduction
What you will find in this repository is a list of guidelines we follow at [Inaka](http://inaka.net) when designing our REST APIs.
These guidelines are not taxative, some exceptions can be found here or there, but we do base our tools (like [Jayme](http://github.com/inaka/Jayme), [Dayron](http://github.com/inaka/Dayron) and [SumoREST](http://github.com/inaka/sumo_rest)) on them. That's why, if you're going to use one of those tools, it would be much simpler to have a server that complies to the following guides.

## Contact Us

For **questions** or **general comments** regarding the use of this repository, please use our public
[hipchat room](http://inaka.net/hipchat).

And you can check all of our open-source projects at [inaka.github.io](http://inaka.github.io)

---

## Conventions & Rules

#### Maintain existing style
> When editing a system written by someone else, stick to the style in which it was designed. If a project has an overall style, stick to that when adding, removing or updating API endpoints as well.

##### Examples
###### Good

- **POST** `/set_table`
- **GET** `/get_table/x`
- **POST** `/set_chair` ⃪ _added by you_

###### Bad

- **POST** `/set_table`
- **GET** `/get_table/x`
- **POST** `/chairs` ⃪ _added by you_

###### Ugly
- **POST** `/set_table`
- **GET** `/tables/x`
- **POST** `/api/v2.3/set_chair` ⃪ _added by you_

##### Reasoning
It's better to maintain a system that just looks ugly to you than to have a system that looks half ugly to you, half ugly to somebody else.

***


