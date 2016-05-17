# Intersection Observer API


Right now to observe position and visibility of DOM elements we need to query DOM state and add listen to scroll event which causes recalculation of style and layout and causes performance issues.


Intersection Observer API provides an interface to get the position and visibility of DOM elements  relative to a container element or to browser top level viewport.

## Using IntersectionObserver API

Creating a IntersectionObserver

```js
var observer = new IntersectionObserver(callback, options);
```

*callback* function fires whenever a given element crosses the threshold.

*options* is an object which can have following properties

- *root* - it is the element relative to which element's intersection is calculated. By default it is browser top level viewport

- *rootMargin* - rootMargin is the offsets applied to element's root that is used to calculate 
intersections. By default it is ```"0px 0px 0px 0px"```

-  *threshold* - threshold is a sorted array in increasing order. Each element of array is a ratio of intersection area to bounding box area of an observed target. Callback is fired when thresholds are crossed for a target. By default it is [0].

## Demo

```js
    var elementsToObserve = [], observer;

    fetch('http://jsonplaceholder.typicode.com/posts').then(res => res.json())
	.then(posts => render(posts))
	.then(() => {
      observer = new IntersectionObserver(onChange, {
        threshold: [1]
      });

      elementsToObserve.forEach(el => observer.observe(el));
    });

    function onChange(changes) {
      changes.map(change => {
        /*
        you can send a XHR that increases the view count of this particular post for now i'm
        just logging the data-id attribute that i send while creating the div of each post
        */
        console.log(change.target.getAttribute('data-id'));
        observer.unobserve(change.target); // unobserve the element after it's view is counted
      });
    }

    function render(posts) {

      posts.forEach(post => {
        var el = document.createElement('div');
        el.setAttribute("data-id", post.id);
        var titleP = document.createElement('p');
        titleP.textContent = "Title: " + post.title.repeat(5);
        var bodyP = document.createElement('p');
        bodyP.textContent = "Body: " + post.body.repeat(15);
        el.appendChild(titleP);
        el.appendChild(bodyP);

        document.body.appendChild(el);
        elementsToObserve.push(el);
      });

    }
```

This is a demo for counting views of a particular post in a list of posts.

First i ```fetch``` random posts from an API and render them into the DOM. After that i create a new InteractionObserver object and set the threshold to ```[0.5]``` so whenever any post crosses browser's top viewport by 50% callback is fired.

Inside the ```callback``` function i logged each target element's id (you can send a XHR to server and increment the count of that post) and then unobserve the element so that callback is not fired again for that particular element.


This is just a simple demo of what you can do with InteractionObserver API

Some other examples:

- [Lazy Loading Images](http://wilsonpage.github.io/in-sixty/intersection-observer/)

- [Delay Loading](https://github.com/WICG/IntersectionObserver/blob/gh-pages/explainer.md#delay-loading)

- [Element visibility for more than 1 second](https://github.com/WICG/IntersectionObserver/blob/gh-pages/explainer.md#element-visibility)


## Further Reading

- [Intersection Observer API draft](https://wicg.github.io/IntersectionObserver/)

> Note: IntersectionObserver API currently is only supported in Chrome Canary




