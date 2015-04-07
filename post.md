I'd been working on the Rails framework for a while now but I hadn't yet made a contribution back to Rails until very recently. I wanted to do something about that. This post is mostly about the journey that helped me get there and also some pointers based on my experience of how one can get there too.

I've also given a lightning talk on this at [RubyConfIndia 2015](http://rubyconfindia.org/). You can view the slides [here](http://goo.gl/yr87oM) if you'd like to see a shorter version of this blogpost.

I was working on a Rails 4.1.x project and for one of the requirements I had to run a custom rake task and pass command line arguments to it. I didn't know how to do it and yeah, you guessed it right, I hit Google.

I found a couple of links(Blogs, Stackoverflow etc.,) that helped, but [this blog](http://davidlesches.com/blog/passing-arguments-to-a-rails-rake-task) in particular was closest to what I was looking for. I wanted to have the ability to run my rake task for specific environments and the `:environment` option allowed me to do just that.

Over the years as a programmer, I've come to realise that the Offical Documentation is one of the most authentic resources one might want to have a look at, if that person is searching for specific details. In my case, I did have a look at what the [Rails guides](http://guides.rubyonrails.org/) had to say about [custom rake tasks](http://guides.rubyonrails.org/command_line.html#custom-rake-tasks). The image below sums up the information the Rails guides provided in the context of my requirement.


![What Rails guides said about custom rake tasks before the Pull request](https://monosnap.com/file/uCXfQKxGbMTU9lqgLRXI1xtxcWF7Xi.png)


At first glance, I had observed few things. As described in the above picture, I didn't clearly understand what they meant by things like `pre_1`, and `t` to start with. When comparing this with the [blog](http://davidlesches.com/blog/passing-arguments-to-a-rails-rake-task), I came to realise these short forms stand for `prerequisite` and `task` respectively. This is exactly where **I found a gap** which triggered in me the thought that this had the potential as to something which could be added to the [Rails guides](http://guides.rubyonrails.org/). The guidelines on [contributing to Rails docs](http://guides.rubyonrails.org/contributing_to_ruby_on_rails.html#contributing-to-the-rails-documentation) confirmed this for me. Below is what exactly they have gotta say on this -

```
You can help improve the Rails guides by making them more coherent, consistent or readable, adding missing information, correcting factual errors, fixing typos, or bringing it up to date with the latest edge Rails. To get involved in the translation of Rails guides, please see Translating Rails Guides.
```

What they've mentioned above was along the lines of my observation and I also found(as indicated in the above photo) that the guides didn't clearly point out on how to access the command line arguments within a rake task, **another detail that could be added**. This led to my [pull request](https://github.com/rails/rails/pull/19357) to the [Rails repository](https://github.com/rails/rails). Below is a snapshot which summarises the changes that were made as part of my pull request.

![Changes made as part of the pull request](https://monosnap.com/file/F3Nh0PEZ8hNhxDs1kvHWN2vTGbr0CZ.png)

I would like to pause for a bit and mention some things I really appreciated about the core team members from what I observed as part of the interim timespan between the time I submitted my PR and until my changes were merged into Rails master. My interaction with a fellow Rails core team member is summarised below -

* Very Friendly
* Approachable
* My commit was merged in almost no time(I think this will vary on a case to case basis based on the nature of the commit, but nevertheless my commit being merged that fast was still in itself a pleasant suprise for me :))

In case by now, if you've decided that you'd like to commit to the Rails repo but if wondering about what's the process of going about the same, the below pointers should help you get started in the right direction. There are basically two scenarios overall via which one can contribute to the Rails repository. These two scenarios are followed by a couple of common steps applicable to both the scenarions. Please find below the enlisted steps that one could follow in order to contribute to Rails - 

* Scenario 1 - Forking Rails for the first time?
  * Fork the Rails repo from this link
* Scenario 2 - Already forked Rails repo but not updated it?
  * Do a fetch, checkout master branch
  * Rebase local master with remote master
  * You can find the exact git commands here
* Common steps applicable to Scenario 1 & 2 mentioned above
 * Create a meaningful branch name
 * Make your changes
 * Use a meaningful commit message
 * Create a Pull Request(PR)
 * Add a summary of changes to explain your PR

Below is a screenshot of an example [Pull request](https://github.com/rails/rails/pull/19357) in the context of the above mentioned details. 

![Use meaningful commit messages](https://monosnap.com/file/oiUZQ2QcOOUuMzzM4YyUC6htofOAl9.png)


Oh and btw, here's how the code in my rake task looked after adding changes as mentioned in the [blog](http://davidlesches.com/blog/passing-arguments-to-a-rails-rake-task) -

```ruby
desc 'Displays user details'
task :display_details_for, [:user_first_name] => [:environment] do |task, args|
  user = User.find_by_first_name(args.user_first_name)

  raise "user doesn't exist in the system currently" unless user
  
  show_details_for(user)
end
```


**A few resources that that motivated me to get here are mentioned below. I am thankful to them.**

1. [Thoughtbot's](https://www.youtube.com/watch?v=DndoNihAzv8) five-day email course on landing the Rails job of
your dreams. Quoting what they say on Day 2 of this course -
```
If you'd like to have some open-source contributions to share, adding and
improving documentation for popular libraries is a great way to get started.
```

2. [RailsConf 2014 - Get More Hands on Your Keyboard by Manik Juneja](https://www.youtube.com/watch?v=DndoNihAzv8)
3. [How I submitted my first patch to Rails (and you can too!)](http://nithinbekal.com/posts/first-rails-patch/)

**My two cents on how you can get started with contributing back to Rails**

* I very recently made [Aaron Patterson]() at [RubyConfIndia 2015](http://rubyconfindia.org/) and I asked him how could we contribute more to open source. He said "**ask more questions**". I think this applies especially when we're not sure of somethings that we'd like to commit or work on, in such a situation it's best to ask.
* Find missing gaps(I just did that and it helped) in the docs. 
* Meet fellow Rubyists in meetups and conferences and talk to them on what you'd like to contribute. Some of them might even want to help you out on this or atleast guide you on stuff. I met  [Prathamesh](https://github.com/prathamesh-sonpatki), **a top 100 Rails contributor** at [GCRC 2015](http://www.gardencityruby.org/) and he told me that folks can even commit things as a pair. I didn't know about this earlier.
* The Rails guides give you great hints on what you can contribute on, do read them in more detail. [Here](http://monosnap.com/image/k5CTXqfitz1f9mUQI95JqG6jjUVxIn) is where you can find those guides.
* Be clear about your branch name, commit message on what exactly you're contributing. Also, I think adding a summary of what your pull request(PR) is about, also helps. If you're not sure, just have a look around wrt other commits, PR to get a feel of what other committers write.


**My Key Takeaways from this experience**

* Start small, but get started
* Once you start contributing, you get confidence to contribute more
* May be it's only when you give back to open source for what you've used from open source, you'll come to realise how it feels like doing so. I personally found the feeling totally worth it :). This small PR simply made me feel more connected with the community
* Take the leap of faith, inspired by others, I've started to ask myself 'Can one be more?'

Thank you.
