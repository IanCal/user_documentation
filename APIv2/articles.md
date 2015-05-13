# Article public endpoints


##List public articles

    GET /v2/articles


**Input**

|Name             |Type|Description|
|-----------------|----|-----------|
|`page`             | `int`|Show specified page only. Default is 1. Max page is 100. To see more then 100 pages, the search endpoint must be used to narrow down the results|
|`per_page`         | `int`|How many entries per page to show. Default is 10.|


**Success Response (list of articles)**
```
Status: 200 OK
```
```json
[{object},{object}...{object}]
```
**Error Response (Max page number reached)**

```
Status: 400 Bad request
```
```json
{"message": "Max page reached. Please narrow down your search"}
```


##Search public articles

    POST /v2/articles/search


**Input**

|Name               |Type   |Description|
|-------------------|-------|-----------|
|`page`             |`int`  |(offset)Show specified page only. Default is 1. Max is 100|
|`per_page`         |`int`  |(limit)How many entries per page to show. Default is 10. Max is 100|
|`search_for`       |`str`  |(query)String to perform search for. Minimum of 4 characters|
|`published_since`  |`date` |(filter)Narrow search  to articles published since the specified date|
|`modified_since`   |`date` |(filter)Narrow search  to articles modified since the specified date|
|`institution`      |`str`  |(filter)Filter results for this instritution only|
|`group`            |`str`  |(filter)Filter results for this institution group only|
|`order_by`         |`str`  |(sort)Perform a sort using the `order_by`. Valid values are: `published_date`, `modified_date`, `views`, `shares`|
|`order_method`     |`str`  |(sort)How to sort. Descending or ascending. Valid values are: `desc`, 'asc'|


**Success Response (list of articles)**
```
Status: 200 OK
```
```json
[{object},{object}...{object}]
```

**Error Response (invalid input)**
```
Status: 400 Bad request
```
```json
{"message": "Invalid value received for order_by"}
```
**Error Response (Max page number reached)**
```
Status: 400 Bad request
```
```json
{"message": "Max page reached. Please narrow down your search"}
```


##Read public article

    GET /v2/articles/{id}


**Success Response (article object)**
```
Status: 200 OK
```
```json
{object}
```
**Error Response (article not found)**
```
Status: 404 Not Found
```
```json
{"message": "Article with specified ID does not exist"}
```


##Read public article version

Read public article that has {id} and {v_number}

    GET /v2/articles/{id}/versions/{v_number}


**Success Response (article object for specified version)**
```
Status: 200 OK
```
```json
{object}
```
**Error Response (Version not found)**
```
Status: 404 Not Found
```
```json
{"message": "Version does not exist"}
```


#Article private endpoints (OAuth required)

##Get own articles

    GET /v2/account/articles

**Input**

|Name               |Type   |Description|
|-------------------|-------|-----------|
|`page`             |`int`  |Show specified page only. Default is 1. Max page is 100. After 100 an error is raise and user is instructed to use the search endpoint|
|`per_page`         |`int`  |How many entries per page to show. Default is 10. Max is 100|


**Success Response**
```
Status: 200 OK
```
```json
[{object},{object}...{object}]
```

**Error Response (Max page number reached)**
```
Status: 400 Bad request
```
```json
{"message": "Max page reached. Please narrow down your search"}
```

##Search own articles

    POST /v2/account/articles/search


**Input**

|Name               |Type   |Description|
|-------------------|-------|-----------|
|`page`             |`int`  |(offset)Show specified page only. Default is 1. Max is 100|
|`per_page`         |`int`  |(limit)How many entries per page to show. Default is 10. Max is 100|
|`search_for`       |`str`  |(query)String to perform search for. Minimum of 4 characters|
|`published_since`  |`date` |(filter)Narrow search  to articles published since the specified date|
|`modified_since`   |`date` |(filter)Narrow search  to articles modified since the specified date|
|`institution`      |`str`  |(filter)Filter results for this instritution only|
|`group`            |`str`  |(filter)Filter results for this institution group only|
|`order_by`         |`str`  |(sort)Perform a sort using the `order_by`. Valid values are: `published_date`, `modified_date`, `views`, `shares`|
|`order_method`     |`str`  |(sort)How to sort. Descending or ascending. Valid values are: `desc`, 'asc'|


**Success Response (list of articles)**
```
Status: 200 OK
```
```json
[{object},{object}...{object}]
```

**Error Response (invalid input)**
```
Status: 400 Bad request
```
```json
{"message": "Invalid value received for order_by"}
```
**Error Response (Max page number reached)**
```
Status: 400 Bad request
```
```json
{"message": "Max page reached. Please narrow down your search"}
```


##Create a new article

    POST /v2/account/articles


**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`title`            |`str`                  |The title for this article - `mandatory`|
|`description`      |`str`                  |The article description. In a publisher case, usually this is the remote article description|
|`tags`             |`array of str`         |List of tags to be associated with the article (e.g ['tag1', 'tag2', 'tagn'])|
|`links`            |`array of str`         |List of links to be associated with the article (e.g ['http://link1', 'http://link2', 'http://link3'])|
|`categories`       |`array of int`         |List of category ids to be associated with the article(e.g [1, 23, 33, 66])|
|`authors`          |`array of int|str`     |List of authors to be assosciated with the article. The list can contain author ids ([12121, 34345, 233323]) or author names([1212, 'John Doe']). No more then 10 authors. For adding more authors use the specific authors endpoint|
|`custom_metadata`  |`dict`                 |List of key, values pairs to be associated with the article|


**Success Response**
```
Status: 201 Created
Location: https://api.figshare.com/v2/account/articles/123
```
```json
{object}
```
**Error Response (Missing mandatory field)**
```
Status: 400 Bad request
```
```json
{"message": "Missing mandatory field - title"}
```


##Read own article

    GET /v2/account/articles/{id}


**Success Response**
```
Status: 200 OK
```
```json
{object}
```
**Error Response (Id not found)**
```
Status: 404 Not found
```
```json
{"message": "Article not found"}
```

##Update article

    PUT /v2/account/articles/{id}


**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`title`            |`str`                  |The title for this article |
|`description`      |`str`                  |The article description. In a publisher case, usually this is the remote article description|
|`tags`             |`array of str`         |List of tags to be associated with the article (e.g ['tag1', 'tag2', 'tagn'])|
|`links`            |`array of str`         |List of links to be associated with the article (e.g ['http://link1', 'http://link2', 'http://link3'])|
|`categories`       |`array of int`         |List of category ids to be associated with the article(e.g [1, 23, 33, 66])|
|`authors`          |`array of int|str`     |List of authors to be assosciated with the article. The list can contain author ids ([12121, 34345, 233323]) or author names([1212, 'John Doe']). No more then 10 authors. For more authors use the authors specific endpoint|
|`custom_metadata`  |`dict`                 |List of key, values pairs to be associated with the article. Similar to custom article fields|


**Success Response**
```
Status: 200 OK
```
```json
    {object}
```
**Error Response (Id not found)**
```
Status: 404 Not found
```
```json
    {"message": "Article not found"}
```


##Delete own private article

    DELETE /v2/account/articles/{id}

**Success Response**
```
Status: 204 No content
```

**Error Response (Id not found)**
```
Status: 404 Not found
```
```json
    {"message": "article not found"}
```


##Reserve DOI for article**

    POST /v2/account/articles/{id}/reserve_doi

**Success Response**
```
Status: 200 OK
```
```json
{"doi": "doi link"}
```
**Error Response (Id not found)**
```
Status: 404 Not found
```
```json
{"message": "article not found"}
```

##Publish article

    POST /v2/account/articles/{id}/publish

**Success Response**
```
Status: 201 Created
Location: https://api.figshare.com/v2/articles/123
```
```json
{object}
```
**Error Response (Id not found)**
```
Status: 404 Not found
```
```json
{"message": "article not found"}
```

**Error Response (Missing mandatory fields for publish (e.g authors))**
```
Status: 400 Bad request
```
```json
{"message": "Missing mandatory field for publication - authors"}
```


##article authors subsection

###List authors

    GET /v2/account/articles/{id}/authors

**Success Response**
```
Status: 200 OK
```
```json
{object}
```
**Error Response (article Id not found)**
```
Status: 404 Not found
```
```json
{"message": "article not found"}
```

###Associate new authors with the article

    POST /v2/account/articles/{id}/authors


**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`authors`          |`array of int|str`     |List of new authors to be assosciated with the article. The list can contain author ids ([12121, 34345, 233323]) or author names([1212, 'John Doe'])|


**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Author(s) id not found)**
```
Status: 400 Bad request
```
```json
{"message": "Author with ID not found"}
```


###Associate and replace existing authors of this article

    PUT /v2/account/articles/{id}/authors

**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`authors`          |`array of int|str`     |List of authors to be assosciated with the article. The list can contain author ids ([12121, 34345, 233323]) or author names([1212, 'John Doe']). Existing authors will be replaced|


**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Author(s) id not found)**
```
Status: 400 Bad request
```
```json
{"message": "Author with ID  not found"}
```


###De-associate author from article

    DELETE /v2/account/articles/{id}/authors/{author_id}

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Author(s) id not found)**
```
Status: 400 Bad request
```
```json
{"message": "Author with ID  not found"}
```


##article categories subsection

###List categories

    GET /v2/account/articles/{id}/categories

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

###Associate new categories with the article

    POST /v2/account/articles/{id}/categories


**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`categories`       |`array of int`         |List of new categories to be assosciated with the article|


**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Category ID not found)**
```
Status: 400 Bad request
```
```json
{"message": "Category with ID not found"}
```


###Associate and replace existing categories of this article

    PUT /v2/account/articles/{id}/categories

**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`categories`       |`array of int`         |List of categories to be assosciated with the article. Existing categories will be replaced|


**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Catgeory ID not found)**
```
Status: 400 Bad request
```
```json
{"message": "Category with ID  not found"}
```


###De-associate category from article

    DELETE /v2/account/articles/{id}/categories/{category_id}

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Category id not found)**
```
Status: 404 Bad request
```
```json
{"message": "Category with ID  not found"}
```


##article files subsection

###List files

    GET /v2/account/articles/{id}/files

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

###Initiate new file upload within the article

    POST /v2/account/articles/{id}/files


**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`md5`              |`str`                  |MD5 sum pre computed on the client side|


**Success Response**
```
Status: 201 OK
Location: https://uploads.figshare.com/hash
```
```json
{object}
```


###Complete file upload

    POST /v2/account/articles/{id}/files/{file_id}/complete

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (File_id not found)**
```
Status: 404 Not found
```
```json
{"message": "File not found"}
```


###View file information

    GET /v2/account/articles/{id}/files/{file_id}

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (File_id not found)**
```
Status: 404 Not found
```
```json
{"message": "File not found"}
```




###Delete file  from article

    DELETE /v2/account/articles/{id}/articles/{file_id}

**Success Response**
```
Status: 200 OK
```
```json
{object}
```
**Error Response (File_id not found)**
```
Status: 404 Not found
```
```json
{"message": "File not found"}
```


##article private links subsection

###List private links

    GET /v2/account/articles/{id}/private_links

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

###Create new private link for this article

    POST /v2/account/articles/{id}/private_links


**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`valid_until`      |`date`                 |Date when this private link should expire - option. By default private links do not expire|
|`scope`            |`str`                  |Private links can be accessed by public users or logged in users. Scope `public` implies that anyone can access the link. Scope `private` implies that only logged in users can access the link|
|`user`             |`int`                  |Private links with `private` scope can be generated for specific users|



**Success Response**
```
Status: 201 OK
```
```json
{object}
```


###Update existing private link for this  article

    PUT /v2/account/articles/{id}/private_links

**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`valid_until`      |`date`                 |Date when this private link should expire - option. By default private links do not expire|
|`scope`            |`str`                  |Private links can be accessed by public users or logged in users. Scope `public` implies that anyone can access the link. Scope `private` implies that only logged in users can access the link|
|`user`             |`int`                  |Private links with `private` scope can be generated for specific users|



**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Private Link ID not found)**
```
Status: 404 Not found
```
```json
{"message": "Private Link with ID  not found"}
```


###Disable/delete private link for article

    DELETE /v2/account/articles/{id}/private_links/{private_link_id}

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (Private link ID not found)**
```
Status: 404 Not found
```
```json
{"message": "Private link with ID not in article"}
```


##Article embargo subsection

###View embargo settings

    GET /v2/account/articles/{id}/embargo

**Success Response**
```
Status: 200 OK
```
```json
{object}
```
###Update embargo settings

    PUT /v2/account/articles/{id}/embargo

**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`active`           |`bool`                 |Confidentiality status. True, False|
|`scope`            |`str`                  |Embargo can be enabled at the `article` or the `file` level. Possible values: `article`, `file`|
|`expires`          |`date`                 |Date when the embargo expires and the article gets published|


**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Input error**
```
Status: 400 Bad request
```
```json
{"message": "Invalid expires date"}
```


###Delete embargo settings

    DELETE /v2/account/articles/{id}/embargo

**Success Response**
```
Status: 204 No content
```
```json
{object}
```



##Article confidentiality subsection

###View confidentiality settings

    GET /v2/account/articles/{id}/confidentiality

**Success Response**
```
Status: 200 OK
```
```json
{object}
```
###Update confidentiality settings

    PUT /v2/account/articles/{id}/confidentiality


**Input**

|Name               |Type                   |Description                                |
|-------------------|-----------------------|-------------------------------------------|
|`active`           |`bool`                 |Confidentiality status. True, False|


**Success Response**
```
Status: 200 OK
```
```json
{object}
```

###Delete confidentiality settings

    DELETE /v2/account/articles/{id}/confidentiality

**Success Response**
```
Status: 204 No content
```
```json
{object}
```




##Article versions subsection

###List versions

    GET /v2/account/articles/{id}/versions

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (article ID not found)**
```
Status: 404 Not found
```
```json
{"message": "article with ID not found"}
```


###View article version

    GET /v2/account/articles/{id}/versions/{v_nr}

**Success Response**
```
Status: 200 OK
```
```json
{object}
```

**Error Response (article version not found)**
```
Status: 404 Not found
```
```json
{"message": "Version {nr}  not found"}
```
