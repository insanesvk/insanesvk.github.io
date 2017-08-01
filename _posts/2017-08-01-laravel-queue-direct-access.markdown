---
layout: post
title:  "Direct access to Laravel's Queue"
date:   2017-08-01 09:12:25
categories: laravel
tags: laravel queue json serialize
---
Imagine that you're not using Laravel's queue exclusively in Laravel, but you have workers deployed for certain tasks in another programming language.

We have specific workers written in Node.js, that listen on a queue, and handle tasks. We also have a queue worker in Laravel handling Pusher events, and other miscellaneous stuff.

## The problem

The actual issue is the Job class. If you dispatch your jobs like this:

{% highlight php startinline=true %}
$job = (new ExampleJob([
    'foo' => 'bar'
]))->onQueue('nodeQ');

$this->dispatch($job);
{% endhighlight %}

... your queue will fill up with **php serialized** stuff (information about the invoking Job class, etc.), which you're most likely just going to use for Laravel anyway:

{% highlight json %}
{
    "job":"Illuminate\\\\Queue\\\\CallQueuedHandler@call",
    "data":{
        "command":"O:29:\\"Acme\\Jobs\\ExampleJob\\":4:{s:11:\\"foo\\";s:7:\\bar\\";s:5:\\"queue\\";N;s:5:\\"delay\\";N;s:6:\\"\\u0000*\\u0000job\\";N;}"
    }
}
{% endhighlight %}

But having another worker-set in a different language entirely, you're posed with another issue - you'll have to **deserialize** PHP's strings and/or objects. And that's not fun, nor is it practical.

## The solution

The solution is actually quite easy, we ended up using `QueueManager` and the `push()` method. This is how the `push` method [looks](https://laravel.com/api/5.4/Illuminate/Contracts/Queue/Queue.html#method_push) (at the point of Laravel 5.4):

{% highlight php startinline=true %}
mixed push(string $job, mixed $data = '', string $queue = null)
{% endhighlight %}

For the sake of the example, we will supply all the arguments, since we're using a dedicated queue for Node anyway. We **don't need a true Job** for this approach, so we're supplying it with a string name to identify it later if needed.

{% highlight php startinline=true %}
public function store(QueueManager $queue)
{
    $queue->push('exampleThing', [
        'foo' => 'bar'
    ], 'nodeQ');
}
{% endhighlight %}

And that's pretty much it! You'll be left with a lovely, non-serialized JSON which can be processed e.g. in Node.js with ease:

{% highlight json %}
{
  "displayName":"exampleThing",
  "job":"exampleThing",
  "maxTries":null,
  "timeout":null,
  "data":{"foo":"bar"}
}
{% endhighlight %}

I hope this article was useful. If you have any questions or comments, please post them down below. Thanks!

Thank you [HR](https://twitter.com/hrenael) for your peer-review!
