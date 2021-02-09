---
layout: post
title: "Automate API testing in Postman"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/postman.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/postman.png)

You can automate testing of API requests and responses in Postman.
Use the login API provided by Reqres.in.

# Request

Log in to Postman and open a new Request tab.
Request (Send) by POST Method as follows:

> Parameter email must be 'eve.holt@reqres.in'.

- Method: `POST`
- Request URL: `https://reqres.in/api/login`
- Content-Type: `application/json`
- Parameter: `{ "email": "eve.holt@reqres.in", "password": "1234" }`

![image](https://heropy.blog/images/screenshot/postman-api-testing/postman1.jpg)

Returns the user token with the response code `200` as shown above.

```undefined
{
"token": "QpwL5tke4Pnpja7X4"
}

```

Let`s test this request this time.
Select the `Tests` tab and select from the `SNIPPETS` list on the right as follows:

- `Status code: Code is 200`
- `Response body: JSON value check`

You can use the ChaiJSBDD syntax and `pm.expected` with JavaScript.
Modify the code generated after selection as follows:

![image](https://heropy.blog/images/screenshot/postman-api-testing/postman2.jpg)

Verify that the response code is `200` and that the returned value contains the `token` attribute.

```js
pm.test("Status code is 200", function () {
pm.response.to.have.status(200);
});
// pm.test("Your test name", function () {
pm.test("Response must have the token property", function () {
var jsonData = pm.response.json();
// pm.expect(jsonData.value).to.eql(100);
pm.expect(jsonData).to.have.keys('token');
});

```

After Send, check the `Test Results` tab at the bottom.
You can see that two tests have passed as follows:

![image](https://heropy.blog/images/screenshot/postman-api-testing/postman3.jpg)

You can test dozens or hundreds of situations in this way, and you can automate these situations with the Collection Runner.

# Collection

Let`s create a new Collection!

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection1.jpg)

In the modal, specify the name and description of the collection and create it.

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection2.jpg)

Let`s save the request we made above!

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection3.jpg)

Under Modal, select and save Collection at the bottom.

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection4.jpg)

After a while, you will see the Collection (`Resres.in test`) and Request list generated from the left menu.

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection5.jpg)

Let`s test all the requests that Collection has.
Select in order, as shown in the following image:

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection6.jpg)

Select the desired request and start the test.

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection7.jpg)

You can see the full test results!

![image](https://heropy.blog/images/screenshot/postman-api-testing/collection8.jpg)

# References

https://learning.postman.com/docs/writing-scripts/test-scripts/