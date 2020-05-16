# star-rating

This is simple star rating component

Requires icon of start, here using Fontawesome

**Usage**

```html
<StarRating  :rating=7 :maxstars=10 :step=1 update-msg="Rating destroyed"/>
```

`rating` current rating

`maxstars` number of stars in rating

`step` what is the step, that user can use for rating

`update-msg` message on rating update