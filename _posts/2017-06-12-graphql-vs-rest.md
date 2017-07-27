---
layout: post
title:  "GraphQL vs REST basic comparison"
date: 2017-06-12 22:40:56
categories: graphql rest coding
published: true
---

Moving away from REST architecture might be a nightmare or a enlightment depending on
how easy (and well documented) is the new model.
I have faced a problem with my previous projects with nested relationships
between multiple objects caused tons of REST calls back and forth which is nothing
new. But when I've found out about GraphQL I wanted to try it out if it's maybe easier
to handle such cases.
On the example below I want to show some of the differences in approaches between
REST and graphQL approaches.

Let say we have an application for dogs daycare with some dogs' records
```
dog: {
 id: 1
 name: 'Bingo',
 breed: 'Mix',
 owner: {
   id: 1,
   name: 'Alex Smith',
   phone: '555-555-5555',
   dogs: [
       { id: 1, 'Bingo', breed: 'Mix', ... },
       { id: 2, 'Sally', breed: 'Mix', ... }
   ]
 },
 dogFriends: [
   {
     id: 2,
     name: 'Jack',
     breed: 'Husky',
     owner: { id: 2, name: 'Max Johnson', ...}
   },
   {
     id: 3,
     name: 'Sally',
     breed: 'Mix',
     owner: { id: 1, name: 'Alex Smith', ... }
   }
 ],
 dogAvoid: [
   {
     id: 4,
     name: 'Pinky',
     breed: 'York',
     owner: { name: 'Lindsay Mill', ... }
   }
 ]
}
```
For each dog we track who the owner is and what are the other dogs are their
bff's and which they don't get along with.

#Major differences:
##Numerous call to get nested information:
If we have situation that one dogs was found sick and we want to call all the owners of
the dogs which might have encounter with the sick one we will have different approaches
in REST vs GraphQL models:


###Calls to be made in REST:
1. Get ids of all dogFriends
dog.filter(id="1")
results
{...
dogFriends:[2, 3]
...}

2. From each dog get owners id
dog.filter(id="2")
results:
{...
owner: 2
...}

3. From each owner id get owner contact details (and remove duplicates
if two dogs have the same owner)
owner.filter(id="2")
results:
{...
name: 'Max Johnson',
phone: 555-333-1111
...}

GRAPHQL
Single call with specified path:
(
 dog (id: "1"){
   dogFriends {
     owner {
       name,
       phone
       }
   }
 }
)


##Server side REST, client side GraphQL queries:
The other major difference is that in REST calls we define static endpoints on the server
side. So often we receive more information than is actually needed at the time. In case
of out sick dog example we get all additional information about the breed that is
not needed while contact number of the owner is nested deeper in the object and requires
additional calls to retrieve.

GraphQL we can have one endpoint which will handle various queries defined on the client side.
This approach allows to retrieve only needed information including those nested inside
other linked objects.

However, by exposing meta information about object structure to the client security
measures and levels of authorizations must be placed between graphQL and database level.


##Scalability
When it comes to scalability REST architecture might have some advantage over
the graphQL. In case of larger data sets, REST calls will load information in parts
and improve user experience. In contrast, graphQL might need more time to build
the whole data tree and present it at once. This is the reason why while working with
graphQL on large datasets (or the ones that will scale) to set up pagination on the reasonable
level and avoid extracting too much data at the single call.


#Summary
GraphQL gives a good alternative to REST calls and although it might require
the change in approach in the way it works (single endpoint on the server side, issues
with scalability and additional security setup on data layer) it is definitely
worth exploring.



