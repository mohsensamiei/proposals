# API Specification Proposal

## URL Path
Path should be start with plural resource name, like: 
```
/users
/products
/orders
```
and continue with dynamic data or reserved words, like:
```
/users/me
/products/123
/orders/6ccf5e1f-4be4-4c32-ad16-2179267bb7e6
```
repeat this convention for nested reources, like:
```
/users/me/orders
/products/123/varient/456
/orders/6ccf5e1f-4be4-4c32-ad16-2179267bb7e6/items
```

## Reserved words:
* me
* details

## Request Content Types:
* `application/json`
* `application/xml`

## Error Raised
### Response:
* Statuses: 
    ```
    4 Family
    5 Family
    ```
* Body: `[serialized object]`
* Vendor:
    ```json
    {
        "error": "message"
    }
    ```
    ```xmls
    <?xml version="1.0" encoding="UTF-8"?>
    <root>
        <error>message</error>
    </root>
    ```

## Create an entity
### Request:
* Method: `POST`
* URL: Should end with plural resource name: 
    ```
    /users
    /products
    /users/me/orders
    ```
### Response:
* Status: `201 Created`
* Body: `[serialized object]`

## Update an entity (Idempotent)
### Request:
* Method: `PUT`
* URL: Should end with dynamic data or reserved words, like: 
    ```
    /users/me
    /products/123/varient/456
    /orders/6ccf5e1f-4be4-4c32-ad16-2179267bb7e6
    ```
### Response:
* Status: `200 OK`
* Body: `[serialized object]`

## Delete an entity
### Request:
* Method: `DELETE`
* URL: Should end with dynamic data or reserved words, like: 
    ```
    /users/me
    /products/123/varient/456
    /orders/6ccf5e1f-4be4-4c32-ad16-2179267bb7e6
    ```
### Response:
* Status: `204 No Content`

## Retrieve Once entity
### Request:
* Method: `GET`
* URL: Should end with dynamic data or reserved words, like: 
    ```
    /users/me
    /products/123/varient/456
    /orders/6ccf5e1f-4be4-4c32-ad16-2179267bb7e6
    ```
### Response:
* Status: `200 OK`
* Body: `[serialized object]`

## Retrieve many entities
### Request:
* Method: `GET`
* URL: Should be end with plural resource name: 
    ```
    /users
    /products
    /users/me/orders
    ```
### Response:
* Status: `200 OK`
* Body: `[serialized object]`
* Vendor:
    ```json
    {
        "elements": [
            {
                ...
            },
            ...
        ],
        "count": 100
    }
    ```
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <root>
        <elements>
            <element>
                ...
            </element>
            ...
        </elements>
        <count>100</count>
    </root>
    ```

## Querying list of entities
### Filtering:
* Key: `filter`
* Value: `[func](data)`
* Functions:
    * `eq`: equal
    * `neq`: not equal
    * `gt`: Greater than
    * `gte`: Greater than or equal
    * `lt`: Less than
    * `lte`: Less than or equal
    * `in`: Exists in list
    * `nin`: Not exists in list
    * `cnt`: Contains value
    * `ncnt`: Not contains value
* Examples:
    ```
    /items?filter=user_id:eq(1)
    /items?filter=user_id:neq("text")
    /items?filter=user_id:gt(2)
    /items?filter=user_id:lte(5)
    /items?filter=user_id:in(1,2,3,4)
    /items?filter=user_id:nin(5,6,7,8)
    /items?filter=user_id:cnt(1)
    /items?filter=user_id:ncnt("text")
    /items?filter=user_id:gte(1),user_id:lt(3)
    ```
### Pagination:
* Keys:
    * `take`: Count of returns records
    * `skip`: Count of skip records from top
* Value: `[int]`
* Examples: 
    ```
    /items?take=10
    /items?skip=20
    /items?take=10&skip=20
    ```
### Sorting:
* Key: `sort`
* Value: `[func](field)`
* Functions:
    * `asc`: Ascending sort
    * `desc`: Descending sort
* Examples: 
    ```
    /items?sort=asc(user_id)
    /items?sort=desc(group_id)
    /items?sort=asc(user_id),desc(group_id)
    ```

### Selecting fields:
* Key: `select`
* Value: `[fields]`
* Examples: 
    ```
    /items?select=user_id,username
    ```

### Expanding object:
* Key: `expand`
* Value: `[entities]`
* Examples: 
    ```
    /items?expand=user,category
    ```

### Full text search:
* Key: `query`
* Value: `[text]`
* Examples: 
    ```
    /items?query="search text"
    ```
