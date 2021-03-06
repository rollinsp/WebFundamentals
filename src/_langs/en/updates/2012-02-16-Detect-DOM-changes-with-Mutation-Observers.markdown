---

layout: update
published: true

collection: updates
category: chrome
product: chrome
type: news
date: 2012-02-16

title: "Detect DOM changes with Mutation Observers"
description: "An introduction to Mutation Observers."
article:
  written_on: 2012-02-16
  updated_on: 2012-02-16
authors:
  - paulkinlan
tags:
  - mutationobserver
  - dom
permalink: /updates/2012/02/Detect-DOM-changes-with-Mutation-Observers.html
---
Back in 2000, the [Mutation Events API was specified](http://www.w3.org/TR/DOM-Level-2-Events/events.html#Events-eventgroupings-mutationevents) to make it easy for developers to react to changes in a DOM (e.g. DOMNodeRemoved, DOMAttrModified, etc).

This feature wasn’t widely used by web developers but it presented a very convenient and popular use case for Chrome Extensions if they wanted to perform some action when something in the page changed.

Mutation Events are useful, but at the same time they create some performance issues. The Events are slow and they are fired too frequently in a synchronous way, which causes some undesired browser bugs.

Introduced in the [DOM4 specification](http://www.w3.org/TR/dom/), [DOM Mutation Observers](http://www.w3.org/TR/dom/#mutation-observers) will replace Mutation Events. Whereas Mutation Events fired slow events for every single change, Mutation Observers are faster using callback functions that can be delivered after multiple changes in the DOM.

You can [manually handle the list of changes](http://www.w3.org/TR/dom/#mutationrecord) the API offers, or use a library such as [Mutation Summary](https://code.google.com/p/mutation-summary/) which makes this task easier and adds a layer of reliability about the changes that took place in the DOM.

You can start using Mutation Observers in Chrome Beta to detect changes in the DOM and be ready to use it when it comes to stable (Chrome 18). If you are currently using  the deprecated Mutation Events, just migrate to Mutation Observers.

Here’s an example of listing inserted nodes with Mutation Events:

{% highlight javascript %}
var insertedNodes = [];
document.addEventListener("DOMNodeInserted", function(e) {
  insertedNodes.push(e.target);
}, false);
console.log(insertedNodes);
{% endhighlight %}

And here’s how it looks with Mutation Observers:

{% highlight javascript %}
var insertedNodes = [];
var observer = new MutationObserver(function(mutations) {
 mutations.forEach(function(mutation) {
   for (var i = 0; i < mutation.addedNodes.length; i++)
     insertedNodes.push(mutation.addedNodes[i]);
 })
});
observer.observe(document, { childList: true });
console.log(insertedNodes);
{% endhighlight %}
