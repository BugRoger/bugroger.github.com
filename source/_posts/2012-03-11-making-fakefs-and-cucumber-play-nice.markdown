---
layout: post
title: "Making FakeFS and Cucumber Play Nice"
date: 2012-03-11 11:52
comments: true
categories: ruby bdd cucumber
---

As it turns out just requiring the fakefs gem doesn't play nice with Cucumber. The fakefs confuses Cucumber pretty well and it can't find its own steps anymore.

There's a simple solution using cucumber hooks. In `features/support/env.rb` add the following tag specific hooks:

``` 
require 'fakefs/safe'

Before('fakefs') do
  FakeFS.activate!
end

After('fakefs') do
  FakeFS::FileSystem.clear
  FakeFS.deactivate!
end
```

Afterwards it's possible to activate FakeFS selectively on a feature or scenario basis:

```
  @fakefs
  Scenario: Create Symlinks
```
