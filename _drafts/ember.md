---
layout: post
title: Splitting hairs in Ember
date: 2019-08-30 08:21 +0100

---
Working for a while with EmberJS applications I have noticed a very common pattern that is used in adapter methods. And it goes like this: 
```javascript
export default ApplicationAdapter.extend({
    urlForQuery(query) {
        const { id } = query;
        delete query.id;
        return `/user/${id}/settings`;
    }
}
```
That always made sense to me. If we don't delete the `id` form the `query` object, `id` ends up being added to the URL's query params when requestiong the data.

```javascript
	//somewhere in the model() hook
	this.get('store').query('user', { id: 100 })
```

In this case, if we don't `delete query.id` it gets appened as a query param: 

```
	GET http://example.com/user/100/settings?id=100
```
Extra query param is not a big deal in most of the cases. If the server is correctly implemented the additional query param will get ignored. But it creates cofusion and sometimes doesn't play well with backend ( for various, sometimes completly valid resons, which we won't disccuss here). 

In those cases we want to delete redundant query paramters and above method is often used. It's even recommended in some [books](https://github.com/skaterdav85/ember-data-in-the-wild/blob/master/chapter5/app/adapters/contact.js) ( not affiliated link, and I am not even criticising the book as I didn't read it, it's probalby fine ). 

But, what I do whan't to criticise is the approach. But before I do, let's look at the example: 

```javascript
export default ApplicationAdapter.extend({
    urlForQuery(query) {
        query.timestamp = new Date(); 
        return `/user/settings`;
    }
}
```

In this case we may start wondering, why is the timesamp there, where is it used? We clearly see the code is modifying the input paramter, and possibly ( most liketly ) treating is as a return value. That is breaking the first of two ( yes, it's breaking one more, we will get there ) 