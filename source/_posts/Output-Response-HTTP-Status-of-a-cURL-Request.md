title: Output Response HTTP Status of a cURL Request
date: 2014-10-10 21:38:28
tags: [cURL API]
---

Debugging your API can be a subtle pain in your backside. Though there are some handy tools available to do that, [Apigee](http://apigee.com) and [Postman](http://www.getpostman.com/) are pretty sweet, but there’d be a lot of folks out there who love to just go native and do it. cURL ftw! 

Below is a handy trick on how you can print the response status of your cURL request.

```
-w "\nSTATUS: %{http_code}\n"
```

`-w` helps you to write an output after completion.
You can also add `-s` as an option if you want to silence the output of your cURL request. 

**Below is an example cURL request:**

```
curl -X PUT \
-H "Accept: application/json" \
-H http://yourapp.com/api/endpoint \
-w "\n\nSTATUS: %{http_code}\n\n"

STATUS: 200
```

That helped at a lot of folks at my office, SupportBee. Hope it helps you guys too!
