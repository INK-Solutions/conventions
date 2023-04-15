# REST endpoints conventions

### Why do we need conventions?

We need conventions because we don't want to think more than necessary. 
We want to use our mental energy on solving project-specific problems, rather than problems that are common for all projects.

Besides we would like to open the project written by any of our colleagues and 'feel like home'. Read it: "as little surprises as possible". We don't want to familiarise ourselves with 'yet another project specifics', we want to agree on the best conventions one time and use them over and over.   

### Abstract

- There are resources, that represent business-relevant entities
- There are verbs that correspond to HTTP methods 
  - GET for getting the resource (should be idempotent)
  - PUT for updating the resource
  - POST foa adding the resource
  - DELETE for removing the resource
- There are actions that are done on the resources
  - Actions can be expressed as verbs (preferred), like: `PUT .../loans/{id}` 
  - Or as verbs and additional information in url, like: `PUT .../loans/{id}/internal-eligibility`
- All api related requests should be hidden under `<application-name>/api/<version>`
- Resource names are plurals

### Example 

Let's say we are building a loan application in system named Loan Origination System (los for short):


1. Create loan would look like this: 

`POST localhost:8080/los/api/v1/loans`

`{ "clientId:" 123, "amount": 1500, "currency": "USD" }`


As a result we would return full loan information: 

`{  "id:" 1, "clientId": 123, "amount": 1500, "currency": "USD" }`.

It's the exact same result that the system would return on the call:

`GET localhost:8080/loans/api/v1/loans/1`. 

The reason why we return the results of `GET` response for `POST` (and `PUT`) calls is because when the application
calling our API (be it front end or other backend system) to update or create resources, they need up-to-date information 
about the state of the resource they are creating/updating. 

You can imagine a simple front-end application that renders a todo-list and allows to add new tasks and edit existing ones. If user adds a new task
to the list, front end does a POST call to backend and after the call is completed front end would need to render a new item and allow user to edit it.
For that functionality front-end needs to know the information about the created resource from backend (because how can you edit resource if there's no id)?

2. Updating the loan would look like this:  


`PUT localhost:8080/los/api/v1/loans/1`

`{ "amount": 2000, "currency": "USD" }`

3. Querying all loans of the client:

`GET localhost:8080/los/api/v1/clients/1/loans` (if backend supports the functionality of quering all loans for specified user - like an admin tool)

`GET localhost:8080/los/api/v1/loans` (if client is querying the loans that belong to him and then authorisation information is taken from headers)

The result of this call will usually be a paginated and look like this: 


```{
"loans": {
    "content": [
        { "id": 1, "amount": 2000, "currency": "USD" },
        { "id": 2, "amount": 3000, "currency": "USD" },
    ],
    "pageable": {
        "sort": {
        "sorted": true,
        "unsorted": false,
        "empty": false
    },
    "offset": 0,
    "pageNumber": 0,
    "pageSize": 20,
    "unpaged": false,
    "paged": true
    },
    "totalElements": 0,
    "totalPages": 0,
    "last": true,
    "sort": {
        "sorted": true,
        "unsorted": false,
        "empty": false
    },
    "numberOfElements": 0,
    "size": 20,
    "number": 0,
    "first": true,
    "empty": true
  }
}
```

This is a default response that is coming from spring when using pagination (`org.springframework.data.domain.PageImpl`). 
For some applications it can be too huge and might need to be shortened to the essential information only.


4. Searching for loans with filtration:

`GET localhost:8080/los/api/v1/loans?curreny=USD&minAmount=1000`


5. Initiate eligibility check on loan:

`PUT localhost:8080/los/api/v1/loans/1/eligibility`

This operation is done on loan, that's why as resource we used loan. 
As a verb we chose `PUT` because we triggered the operation that would modify the internal state of the loan.
And we added `eligibility` to URL for specifying an action that we want to execute 
(`PUT localhost:8080/los/api/v1/loans/1` is already busy because it updates the loan).
