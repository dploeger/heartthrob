# Heartthrob - A webservice description language for Rockstars

![Draft](https://img.shields.io/badge/State-Draft-yellow.svg)

## Introduction

Heartthrob is a definition language for webservices and also resembles the spoken lyrics of power ballads from the 80s. It is a hommage to the [Rockstar programming language](https://github.com/dylanbeattie/rockstar) and follows the same idea.

## Why?

Because it can be done and now, after certifying as a Rockstar developer, one can also say, that they can define webservices like a Heartthrob.

## The Heartthrob specification

Like in Rockstar, services in Heartthrob are defined using phrases in some sort of poetic english.

### File Format

Heartthrob uses UTF-8 plaintext files with the extension .throb.

### Comments

Sometimes it's necessary to add some context to an endpoint when defining it. Comments start with the keyword "But". Everything else is ignored until the next empty line.

### Ignored words

Some words like articles are ignored by Heartthrob. So that "a monster" is the same as "monster". These ignored words are:

* the, a, an
* my
* his,hers, its

### Endpoints

Endpoints are a crucial part of every web service definition. They are mostly resembled as slash-separated and optionally variable paths to a ressource like /users/:uid/signatures/:sid. Often, there are multiple endpoints at one part of the path (like /users, /users/:uid). These things don't look very poetic, so Heartthrob uses concatenation, references and aliases for most things. 

Every endpoint starts with "Calling". Aliases are "Reaching out to", "Screaming at", "Talking to". Paths are concatenated using "and" and "while", commas or without anything at all. All components of the path are lowercase a-z. If the last component is introduced with the "while" alias, everything after it is ignored.

    Talking to common and users while auth is ignored

    /common/users/auth

If the endpoint definition is started with "and", it will be added to the previous endpoint:

    Talking to common.

    /common

    And screaming at users.

    /common/users

### Item Aliases

Usually, object names (like path components) in web services aren't the most poetic words out there, so to stay in character, Heartthrob allows us to create item aliases for them. These item alias definitions follow the form of "Understand, that <something> is/are/(are) like <something>", so one could use "Understand, that users are unicorns" and later use the word "unicorns" in the endpoint definition:

    Understand, that common is shining, users are unicorns and auth is love!
    Screaming at shining unicorns while love is around!

    /common/users/auth

Aliases for "Understand, that" are:

* (Please/Try to) understand, that/a...
* (Please/Try to) Believe me, that/a...
* (Why) Can't you see, that...
* (Please/And always) Remember, that/a...

Aliases can be concatenated using commans, "and" and full stops.

### Resources

Resources are objects transported to or retrieved from the webservice. They usually come in the form of JSON or XML documents and have some structure. This structure can also be described in Heartthrob using a resource title and telling about the structure in this form:

    Believe me, a token is a monster and the timestamp is hell.

    I know about my ticket to nowhere.
    It's made of a monster with strings and the hell counts the days.

    Ticket:
    {
        "token": "<string>",
        "timestamp": "<date>"
    }

The object name is introduced with "I know about". The next line directly follows and tells about the structure using "It's made of". The names for the keys can use item aliases and the data types use the following aliases:

* String: strings, texts, words
* Integer: numbers, "can be counted"
* Date: "count(s) the days", "calendar", "time is fleeting"
* Boolean: "truth", "lies"

Additionally, these words are ignored in between:

* "are made of", "with", "are filled up with"

Array are denoted like "a multitude of <key name or alias>", "thousands of <key name or alias>".

Objects need to be linked. So you need to create another resource for the sub-object and just reference the name of the other resource. The resource name is then used as the key. If that shouldn't be the case, use "called" or "which I know as".

    Believe me, a token is a monster and the timestamp is hell.

    I know about my ticket.
    It's made of a monster with strings and the hell counts the days.

    I know about a keychain.
    It's filled up with a user of strings and a multitude of tickets which I know as keys.

    {
        "user": "<string>",
        "keys": [
            {
                "token": "<string>",
                "timestamp": "<date>"
            }
        ]
    }

Aliases for "I know about" are:

* "I heard about"
* "There once was"

### Document types

Usually, web services accept and return documents in JSON or XML format. This can be set globally or per resource. The two document formats have aliases called "Jason" (JSON) and "Grampa" (XML). To define, which documents are accepted type use the form "I heard it from <document format>", to define which formats are sent, use the form "I spoke with <document format>". Multiple formats can be added using "and" and commas.

If you accept and send the same formats you can use the form "I heard it from <document format> and told them/him my story!".

### HTTP Methods

One aspect of webservices are different HTTP methods, that can be used for every endpoint. As GET, POST, DELETE and so on aren't really poetic, Heartthrob define some aliases for them:

* GET: get(ting), fetch(ing), hungry for
* POST: push(ing), forcing, force
* PUT: changing, change
* PATCH: rearranging, rearrange
* DELETE: kill(ing), rip(ping) apart, destroy(ing)
* OPTIONS: I want to know, Please tell me

    Believe me, a token is a monster and the timestamp is hell.

    I know about my ticket to nowhere.
    It's made of a monster with strings and the hell counts the days.

    And always remember, that common is shining, users are unicorns and auth is love!

    Screaming at shining unicorns while love is around.

    Hungry for my ticket to nowhere.

    GET /common/users/auth
    {
        "token": "<string>",
        "timestamp": "<date>"
    }

### Status codes

[HTTP Status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) tell the client about failures and successes for their calls and web service definitions usually include the possible return codes. They are three digit numbers, so Heartthrob also tries to be poetic about most of them. These aliases are defined:

* 200: "I'm okay", "It's allright", "I'm fine"
* 201: "It's been done", "I've made that for you"
* 301: "She's/He's gone for good"
* 302: "She's/He's gone"
* 400: "My/Your mistake", "You don't understand me!", "He/she doesn't understand me!"
* 401: "I don't know, who you are", "Who are you", "I don't know who she/he was"
* 403: "You/She/he can't do that (to me)", "Don't do that!"
* 404: "You're/She's/He's wrong", "What are you doing here?", "What is she/he doing here?"
* 500: "The gods were against me/you/her/him", "It wasn't my/your/her/his fault"

The possible status codes are added after the resource type reference, are introduced with "telling me that" and can be concatenated using commas, full stops or "or"

    Hungry for my ticket to nowhere, telling me that it's allright. Who are you?

## Example

The following sample Heartthrob document defines the service available at [JsonPlaceHolder](https://jsonplaceholder.typicode.com/):

```
I heard it from Jason and told him my story!

Believe me, that posts are like angels, comments like beasts. Albums are like horses, photos like ecstasy, todos are pointless and users are enemies.

Why can't you see, that userID is a friend and id is truth.

I heard about an angel.
It's made out of a friend with numbers and the truth can be counted, the title is text and the body is made out of words.

Screaming at angels.
Fetching an angel. I'm okay.
```

## Todo

This currently is a draft, so suggestions are welcome. Also, these points aren't currently handled yet:

* Authentication
* Optimizing the specification and adding more aliases