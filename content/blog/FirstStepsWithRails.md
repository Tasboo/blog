---
title: "First Steps with Rails Development"
date: "2019-09-20T07:02:00+01:00"
draft: false
---

After I got my [first rails app](/blog/RubyOnRailsInWindows) created, I wanted to do some learning on my own first. My first question was, "what in the world is this page and how do I edit it?":

![My first rails app](/images/successmyapp.png)

Turns out, you're not supposed to edit this page. It seems to be some sort of built in render within Rails that you can't edit yourself. It probably only gets displayed if you don't have a default route setup for your root. I tried editing views/applicaiton.html.erb, but nothing I changed there seemed to have an impact.

![Modified application view](/images/ModifiedApplicationView.png)
![Failed results](/images/ModifiedApplicationViewResults.png)

If you want to edit what gets displayed when you run your site, you need to add a controller and then set that controller to the root of your site. Editing the default application contoller or view doesn't have an impact on what is displayed until you have created a controller/view and setup your routes page.

To add a new controller, I ran the following:

```
rails generate controller Home index
```

Some explanation of what each of this command seems to be doing:

- rails = rails command
- generate = tell rails to generate something
- contoller = tell rails that I want to generate a controller
- Home = the name of the new controller
- index = the name of the view that will get generated for the controller

After running that command a few different things happen, the output tells me that it did a few things. Let's see what it did.

1. The controller file 'controllers/home_controller.rb' gets created
   1. By default, the Home controller is setup to inherit from the ApplicationController
   2. The home controller has index defined in it.
2. The view file 'views/home/index.html.erb' gets created
   1. Some default html is added to that file automatically telling how to find this file.
3. The helper file 'helpers/home_helper.rb' gets added
   1. I have no idea what this does right now. The helper by default has no code other than declaring the module
4. The 'config/routes.rb' file gets modified to include an entry to do a GET on home/index
5. The test file 'test/controllers/home_controller_test.rb' gets created.
   1. I have no idea how to run tests in Rails yet, but I am glad this gets added automatically.
   2. Something to note, while there is a section for helpers within the test directory, a test class for the home_helper was not added.
6. The scss file 'assets/stylesheets/home.scss' was created for styling the home controller view. 
   1. Nothing is in this file asside from a comment.

So that command did a lot it seems, but I want to make sure that my app defaults to this new controller when I navigate to the root of the site. To do that, I add an entry in the routes.rb file so that it looks like this:

```
Rails.application.routes.draw do
  get 'home/index'
  root 'home#index'
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

This tells rails that if the root of the site is requested, execute the home controller with the index route/view. Without changing anything else, I run my site uisng 'rails server -b=0.0.0.0' and now see this:

![Successful results](/images/NewHomeController.png)

Awesome! Not only do I see the new index view for my home displayed, I also see my modifications that I did earlier to the application.html.erb view. Looking at these results, it's very easy to compare ASP.NET MVC to Rails. Seeing the routes, controllers, and views feels very familiar to me.

I think this will be it for this post, but there are a few things I would like to look at next.

1. How do I run tests? That new test file has me intrigued.
2. How do I pass variables from my controller module down to the view I am trying to render?
3. What is with all the files in the config/initializers directory? I am curious to see what I can do with each one of those files.
4. How do I start using my postgresql database that I created when I set all this up originally?

   
Speaking of postgresql, I have a bit of tangent that I had to go down when I first started this post. When I started this post, it had been a while since I ran the app. When I first went to run the app, I got an exception with the message 'Is the server running locally and accepting connections on Unix domain socket "/var/run/postgresql/.s.PGSQL.5432'. To fix this, I found this stackoverflow post which lead me to use this command 'sudo pg_ctlcluster 11 main start'

1. not sure exactly what pg_ctlcluster does, but I assume it allows me to controll postgresql clusters?
2. 11 corresponds to the version of postgres I am using
3. main corresponds to the name of the cluster
4. start corresponds to the action I want to do on the cluster. In this case, start it.

To verify the version and cluster name, navigate to the etc/postgresql directory on your Linux instance. The first directry you see should be the version number and within the version number directory, you should see the cluster name directory.

```
/etc/postgresql/11/main$ sudo pg_ctlcluster 11 main start
```

After running that command, I got the output 'Removed stale pid file'. Running the site after that worked just fine. It seems postgresql shut down or something and had a stale file? Not sure exactly what that was all about, but it seems to be fixed for now.




