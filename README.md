# REST API Design Guidelines

## Introduction
What you will find in this repository is a list of guidelines we follow at [Inaka](http://inaka.net) when designing our REST APIs.
These guidelines are not taxative, some exceptions can be found here or there, but we do base our tools (like [Jayme](http://github.com/inaka/Jayme), [Dayron](http://github.com/inaka/Dayron) and [SumoREST](http://github.com/inaka/sumo_rest)) on them. That's why, if you're going to use one of those tools, it would be much simpler to have a server that complies to the following guides.

## Contact Us

For **questions** or **general comments** regarding the use of this repository, please use our public
[hipchat room](http://inaka.net/hipchat).

And you can check all of our open-source projects at [inaka.github.io](http://inaka.github.io)



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

------

#### 

#### Use snake_case with JSON

> When the endpoint returns or receive a JSON, the keys should always be in snake_case.


##### Examples
###### Good
```
{
	"caption": "string",
	"comment_count": 0,
	"created_at": "2017-03-07T18:57:44.622Z",
	"id": "string",
	"like_count": 0,
	"media_type": "image",
	"owner": "string"
}
```

###### Bad

```
{
	"caption": "string",
	"commentCount": 0,
	"createdAt": "2017-03-07T18:57:44.622Z",
	"id": "string",
	"likeCount": 0,
	"mediaType": "image",
	"owner": "string"  
}
```

##### Reasoning
Even though in some programming languages is common to name variables or object instances with camelCase names, we consider the snake_case is the most readable and understandable way to name the key fields on a JSON object.

---

#### Don't use response envelopes by default
> When the endpoint returns, for example, an array, it should be returned as a plain array, not a dictionary with the array as a value.


##### Examples
###### Good

```
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
```
###### Bad
```
{ "items" :
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
}
```

##### Reasoning
If you're calling a **GET** on `/entities` , you're asking for a list of entities, not an object with the entities inside one of its properties. So don't envelope things that should be retrieved directly.

---

#### Use all HTTP verbs (Not everything is a GET)
> Use all the HTTP verbs. If you have to Create something use POST. If you want to Read something use GET. If you want to Update something use PATCH or PUT. If you want to Delete something use DELETE. Always remember C.R.U.D. (Create, Read, Update, Delete) when setting the verb for your resource.


##### Examples
###### Good

>Creating a comment on a media item: 

- **POST** `/media/:media_id/comments`
   This Creates a comment related to the media id you pass along the URL.
> Reading comments on a media item:

- **GET** `/media/:media_id/comments`
   This Reads all the comments related to the media id you pass along the URL.
> Updating ( editing ) the comment content on a media item:

- **PUT**  `/media/:media_id/comments/:id`
   This Updates all the fields of the comment with the id you pass along the URL
> Deleting a comment on a media item:

- **DELETE** `/media/:media_id/comments/:id`
   This Deletes the comment with the id you pass along the URL

###### Bad
> Creating a comment on a media item: 

- **GET** `/media/:media_id/create_comment?text=comment%20text`

> Updating the comment content on a media item:

- **GET**  `/media/:media_id/update_comment?text=comment%20new%20text`
> Deleting a comment on a media item:

- **GET** `/media/:media_id/delete_comment?id=comment1`


##### Reasoning
Using the right HTTP Verb on each resource will make clearer what the resource is made for and what we should expect it to do, receive and return. 

---

#### Use Nouns for endpoints names ( URLs )

> Your resources should be always nouns and no verbs.

##### Examples

###### Good

> Getting an entity

- **GET** `/entities/:id`
  This retrieve the entity with the Id you pass along the URL.

> Creating an entity:

- **POST** `/entities`
  This creates an entity with the info you pass along the request body ( We'll get to this later ).

> Deleting an entity

- **DELETE** `/entities/:id`
  This Deletes the entity with the id you pass along the URL

###### Bad

> Getting an entity

- **GET** `/get/entity?id=:id`

> Creating an entity

- **POST** `/create/entity`

> Deleting an entity

- **DELETE** `/delete_entity/:id`

##### Reasoning

Your resources should reflect what they do based on the HTTP Verb plus the resource name ( Noun ). So it make sense that doing a **GET** to `/entities` retrieves a list of entities. In exchange, doing a **GET** to `/get/entity` looks redundant and is not self descriptive.

---

#### Plural nouns or singular nouns?

> If your resource contains a list or a group of entities insted of just one then it should be named with a plural noun. If it always contains on single entity then it should be named with a singular noun.

##### Examples

###### Good

> Getting comments from an entity

- **GET** `/entities/:id/comments`
  This retrieve a list of comments for the entity with the Id you pass along the URL.

> Getting the likes count of an entity

- **GET** `/entities/:id/like_count`
  This gets the likes count of  an entity with the Id you pass along the URL.

> Deleting all the comments from an entity

- **DELETE** `/entities/:id/comments`
  This Deletes all the comments fomr the entity with the id you pass along the URL

###### Bad

> Getting an entity

- **GET** `/entity/:id`

> Creating an entity:

- **POST** `/entities`

> Deleting a comment from an entity

- **DELETE** `/entities/:id/comment/:id`

##### Reasoning

Your resource should be clear about the fact that it is a single object or a group/list of objects

***
