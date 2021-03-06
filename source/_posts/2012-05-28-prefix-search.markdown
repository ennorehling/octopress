---
layout: post
title: "Prefix search, Part 3: more fun with tries"
date: 2012-05-28 14:49:49
comments: true
categories: [kata, code, algorithms]
---
In my last post, I demonstrated that crit-bit trees are really fast. This time,
I want to talk a little bit about my specific implementation of these tries, and
some interesting applications you can use them for.

<!-- more -->
I also want to acknowledge some of the prior art that I looked at before
deciding to roll my own implementation. Professor Bernstein himself has written
an implementation himself for his portable qhasm assembler. But since it's part
of a larger software product, I didn't want to disentangle it. Adam Langley has
an implementation on [his github](https://github.com/agl/critbit) with
some outstanding documentation that was very useful, but it only stores
zero-terminated strings, not arbitrary data. While I didn't read his code too
closely to keep my own implementation challenging, I did steal the clever
bit-mask trick from it.

## More Things To Do With Tries

While they are really good at string lookup, what crit-bit trees are best at is
finding strings with a common prefix. I implemented two separate functions for
this:
{% codeblock lang:c %}
const void * cb_find(critbit_tree * cb, const void * key, size_t keylen);
int cb_foreach(critbit_tree * cb, const void * key, size_t keylen,
    int (*match_cb)(const void *, const void *, size_t, void*), void *data);
{% endcodeblock %}
The first function is straightforward, it looks for an exact match of the first
`keylen` bytes of `key` in the given tree. The second function
calls the `match_cb` callback on every match, which is a much easier
pattern to implement than trying to return all matches. Where would you store
them? Dynamic memory allocation is right out, of course. I compromised a bit by
writing the `b_find_prefix` function, which takes a buffer supplied by
the caller, but beare that paging through the results by calling it repeatedly
is quadratic in complexity, so you shouldn't do that. It's really just a hack.

## Use As A Fast Key-Value Store

One really nifty thing that I have been using this code for is as a fast
key-value store. You could insert a couple of strings like "config_value=42" in
the tree and look for the first match with the prefix "config_value=". There are
two wrapper macros in critbit.h to simplify this, and being able to store data
other than zero-terminated strings means I am not limited to storing string
values. Here's some example code that shows their use:
{% codeblock lang:c %}
int i = 42;
char buffer[20];
const char * key = "herpderp";
void * match;
size_t len;

len = cb_new_kv(key, strlen(key), &i, sizeof(int), buffer);
cb_insert(&cb, buffer, len);
if (cb_find_prefix(&cb, key, strlen(key)+1, &match, 1, 0)) {
    cb_get_kv(match, &i, sizeof(int));
}
{% endcodeblock %}
What these macros essentially do is this:

1. `cb_new_kv` creates a (key,0,value) string inside buffer.
2. Once you insert it, you can search for it by prefix-search for the key plus its zero-terminator.
3. Finally, use `cb_get_kv` to extract the value from the first match.

## Testing

I approached this project with a test-first attitude, writing tests before
writing the actual implementation, and fixing bugs until the tests passed. I
wrote almost as many lines of tests for this as I wrote actual code, and it
still wasn't enough - one or two small issues slipped through. Lots of pointers
and memory-pokery is always tricky, and I think this is one of those cases where
it should be undisputed that unit tests are just incredibly useful.
