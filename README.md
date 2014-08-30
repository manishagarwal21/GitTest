STATUS
======
- can retrieve FB friend list of logged in user
- retrieving birthdays is a different deal

>Where do we go from here?


RANTS
=====

This is why we can't get all friends: https://developers.facebook.com/docs/apps/faq
look for this question:

* _In v2.0 of the API I'm unable to get the full friend list of someone who has logged into my app - is there a way to get the full friend list?_

also relevant:
* _This will only return any friends who have used (via Facebook Login) the app making the request_ from [here](https://developers.facebook.com/docs/graph-api/reference/v2.1/user/friends).

another obstacle:
* _https://developers.facebook.com/docs/facebook-login/permissions/v2.1_

this is the [code](http://vikasrao.wordpress.com/2011/06/02/sorting-birthdays-received-from-facebooks-graph-api/) which would have solved our problem before FB got wise:
* code:
```
FB.api("/me/friends?fields=id,name,birthday,picture, link", function(response) {
    var birthdays = response.data; //list of friend objects from facebook
    var currentMonth = new Date().getMonth() + 1;
    var upcoming = [];
    for (var i = birthdays.length - 1; i >= 0; i--) {
        item = birthdays[i];
        if (item.birthday) {
            var bday = item.birthday;
            //if the birthday is after today
            if (currentMonth <= bday.substr(0, 2) * 1 && new Date().getDate() <= new Date(bday).getDate()) {
                upcoming.push(item);
            }
        }
    };
    //set the year to current year because of birth years being different.
    var year = new Date().getFullYear();
    upcoming = upcoming.sort(function(a, b) {
        return new Date(a.birthday).setYear(year) - new Date(b.birthday).setYear(year);
    });
 
    console.log(upcoming);//console log here, but do whatever you want with the sorted friends
});
```

KNOWN BUG LIST
=================
- page does not automatically display birthdays on successful user login. But works on refresh
