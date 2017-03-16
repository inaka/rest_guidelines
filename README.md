# REST API Design Guidelines

## Introduction
Here, you will find the guidelines that we follow at [Inaka](http://inaka.net) when designing our REST APIs. These guidelines are not taxative, some exceptions can be found here or there, but we do base our tools (like [Jayme](http://github.com/inaka/Jayme), [Dayron](http://github.com/inaka/Dayron) and [SumoREST](http://github.com/inaka/sumo_rest)) on them. That's why, if you're going to use one of those tools, it would be much simpler to have a server that complies to the following guides.

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

#### Use snake_case in URLs and JSON keys

> Use *snake_case* for separating words when naming URLs and JSON keys.


##### Examples
###### Good

**GET** `/feed_items/123`

```
{
	"caption": "string",
	"comment_count": 0,
	"created_at": "2017-03-07T18:57:44.622Z",
	"id": "123",
	"like_count": 0,
	"media_type": "image",
	"owner": "string"
}
```

###### Bad

**GET** `/feedItems/123`

```
{
	"caption": "string",
	"commentCount": 0,
	"createdAt": "2017-03-07T18:57:44.622Z",
	"id": "123",
	"likeCount": 0,
	"mediaType": "image",
	"owner": "string"  
}
```

##### Reasoning
Studies have found that *snake_case* is 20% easier to read than camelCase. Also, although JavaScript convention is *camelCase*, most of popular JSON APIs use *snake_case*.

---

#### Don't use response envelopes by default
> When an endpoint needs to return several entities in one response, place those entities in a plain array at the root level of the JSON object. Don't wrap that array within another object in the JSON.

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
Having an array with the results at the root level is simpler than anything else. Using an envelope, on the other hand, adds complexity to the response, and the possibility of introducing more information to it, that may not be related to the resources themselves, such as pagination information, which should be somewhere else.

---

#### Use proper HTTP verbs

> Any endpoint must use an HTTP verb that reflects what the endpoint is meant to do. Typically, these are the scenarios:
> * Use the `POST` verb for endpoints that create entities.
> * Use the `GET` verb for endpoints that return entities.
> * Use the `PUT` verb for endpoints that fully update entities.
> * Use the `PATCH` verb for endpoints that partially update entities.
> * Use the `DELETE` verb for endpoints that delete entities.

##### Examples
###### Good

- **POST** `/media/:media_id/comments` *Creates a comment related to the media with id :media_id.*

- **GET** `/media/:media_id/comments` *Reads all the comments related to the media with id :media_id.*

- **PUT** `/media/:media_id/comments/:comment_id` *Updates all the fields of the comment with id :comment_id.*

- **PATCH** `/media/:media_id/comments/:comment_id` *Updates specific fields of the comment with id :comment_id.*

- **DELETE** `/media/:media_id/comments/:comment_id` *Deletes the comment with id :comment_id.*

###### Bad

- **GET** `/media/:media_id/create_comment?text=comment%20text`

- **GET**  `/media/:media_id/update_comment?text=comment%20new%20text`

- **GET** `/media/:media_id/delete_comment?id=:id`


##### Reasoning
Taking advantage of the HTTP verbs by using them properly leads to clear APIs that can be known and understood by the server and the clients with no ambiguity.

---

#### Use Nouns for endpoints names (URLs)
> Your endpoint names should be always nouns, not verbs.

##### Examples

###### Good

> Getting a user

- **GET** `/users/:id`
  The endpoint retrieves the user with the id you pass along in the url.

> Creating a user

- **POST** `/users`
  This creates a user with the info you pass along in the request body.

> Deleting a user

- **DELETE** `/users/:id`
  This deletes the user with the id you pass along in the url.

###### Bad

> Getting a post

- **GET** `/get/posts?id=:id`
  This is wrong because you already established that it is a GET. Using /get on the endpoint name is redundant

> Creating a post

- **POST** `/create/post`
  Same as above, you already estabished that you're going to create something with the **POST** so, again, the create is redundant.

> Deleting a post

- **POST** `/delete_post/:id`

  In this case you're using **POST** (a verb that's intended to be used for creation) to delete an object. That's semantically wrong. You should use **DELETE** for that, instead. If you do so, you'll notice that you no longer need to add delete to your url and that then your url will clearly identify the object being deleted.
##### Reasoning

Your endpoints should reflect what they do based on the HTTP Verb plus the endpoint name (noun). Therefore it makes sense that doing a **GET** to `/entities` retrieves a list of entities. On the contrary, doing a **GET** to `/get/entity` looks redundant and it's not self descriptive. 
Let's try with another example: **POST** `/update_entity/:id`. That, semantically speaking, should be interpreted as creating/adding an entity to the `update_entity` set. If your intention was to update the entity with id `:id`, you should've used **PUT** or **PATCH** `/entities/:id` instead.

---

#### Plural nouns or singular nouns?

> If your endpoint manages a list or a group of entities instead of just one then it should be named with a plural noun. If it always manages a single entity then it should be named with a singular noun.

##### Examples

###### Good

> Getting comments from a post

- **GET** `/posts/:id/comments`
  The endpoint retrieves a list of comments for the post with the id you pass along in the url.

> Getting the likes count of a post

- **GET** `/posts/:id/like_count`
  The endpoint retrieves the likes count of a post with the id you pass along in the url.

> Deleting all the comments from a post

- **DELETE** `/posts/:id/comments`
  The endpoint deletes all the comments from the post with the id you pass along in the url.

###### Bad

> Getting a post

- **GET** `/post/:id`
  You're getting a post from a list/group of posts, so it should be plural `/posts/:id`.

> Updating a post

- **PUT** `/posts`
  You're trying to update a post but you're not passing the id on the url. Passing it along in the request body is wrong.

> Deleting a comment from a post

- **DELETE** `/posts/:post_id/comment/:id`
  Here you have a list/group of comments so it should be plural `/posts/:post_id/comments/:id`. Notice how I changed `comment` for `comments`.

##### Reasoning

Your endpoint name should be clear about the fact that it affects a single object or a group/list of objects. If you do a **POST** to `/posts` , you'll find the object you created by doing a **GET** to `/posts/:id`. 

---

#### Query String VS. Request Body. Where to send the object?

> There are four places where you can add paremeters in your HTTP request: `query string`, `path`, `body` and `headers`. Query Strings are only valid on **GET** requests and every parameter they include is or should be used as a filter on the data returned. The only scenario where a parameter goes in the path is to identify a resource. Every other paremeter (in **POST**, **PUT**, **PATCH**, etc) should go in the request body. Any parameter that doesn't match any of the previous rules should go in headers (authentication, API versioning, etc).

##### Examples

###### Good

> Getting posts for an specific user

- **GET** `/posts?user_id=:id`
  The endpoint retrieves a list of posts whose owner is the user with the id in the url

> Rating a post

- **PATCH** `/posts/:id`
  `{"rate": 4.5}`
  The endpoint gives the post a rating value. 

> Geting places nearby the user

- **GET** `/places?latitde=35.002&longitude=55.032`
  The endpoint retrieves all the places nearby the user's location. Radius is a fixed value on the server.

###### Bad

> Creating a place

- **POST** `/places?latitde=35.002&longitude=55.032&name=awesome%20place&type=restaurant`
  Sending the parameters as a query string on a POST request is wrong. They should be in the request body. Something like 
```js
{
	"latitude": 35.002
	"longitude": 55.032
	"name": "awesome place"
	"type": "restaurant"
}
```

> Updating a place

- **PATCH** `/places?id=123&name=incredible%20place`
  Don't send the parameters on the query string unless it is a GET. You should be sending them in the request body like
```js
{
	"id": "123"
	"name": "incredible place"
}
```

> Geting places from Buenos Aires

- **GET** `/places`
```js
{
	"city": "Buenos Aires"
}
```
  Don't send the search parameters on the request body if you're using a **GET**. Your url should be `/places?city=Buenos%20Aires`

##### Reasoning

Is a good semantic practice to use query strings only when you're using **GET** and use the request body to send all the data needed by the endpoint when using **POST**, **PUT**, **PATCH**, etc.

***
