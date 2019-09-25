---
title: "Doing CRUD in Rails"
date: "2019-09-25T04:00:00+01:00"
draft: false
---

Following up on my last post on [creating a controller, action, and routes](/blog/NewControllerActionAndRoutes), I decided to expand my animal picker concept into a full fledged CRUD feature. The goal was to allow for adding, updating, deleting, and viewing animals using postgresql and rails.

What I've learned while making this CRUD app is that the official documentation from the Rails team is pretty awesome. To get the CRUD application going, I leaned heavily on a few different guides from them:

- [Getting Started with Rails](https://edgeguides.rubyonrails.org/getting_started.html) for creating the core CRUD functionality.
- [Active Record Basics](https://guides.rubyonrails.org/active_record_basics.html) for generating the database table and model.
- [Active Record Migrations](https://guides.rubyonrails.org/active_record_migrations.html) for modifying the database table and model.
- [Active Storage Overview](https://edgeguides.rubyonrails.org/active_storage_overview.html) for storing image files with my records.

The thing is there are plenty of great blog posts in the Rails community on how to create CRUD applications, but none of them seemed to get the simplicity and throughness of these official guides. So I guess my point is, <strong>if you are starting out with Rails development, the official guides are your best place to start</strong>.

While the official guides explain how to do things very well, there are still a few edge cases that I had to google to figure out. This mainly had to do with me changing requirements and straying from the official guides, but I thought I'd share those experiences in case anyone else decides to stray from the path a bit like I did (I deliberately avoided the "off the rails" pun opportunity here 😛).

<h3>Syntax Issues</h3>
First issue I ran into was my own idiocy. I got to the point where we [start to save data](https://edgeguides.rubyonrails.org/getting_started.html#saving-data-in-the-controller) and I kept on getting an error that wasn't detailed in the guide. The error was "wrong number of arguments (given 0, expected 1)". Googleing the error didn't seem to produce any solutions that helped. It wasn't until after a while that I realized I had used square brackets instead of parens when calling the .require method.

Bad way 
```
params.require[:animal].permit(:name, :credit, :image)
```

Good way 
```
params.require(:animal).permit(:name, :credit, :image)
```

I think I ran into this issue because I had just been accessing :animal from the params collection like params[:animal] before.

<h3>Editing an Existing Database Table</h3>
The next issue I ran into was editing an existing table that I had created using the "rails generate model" command. The Getting Started guide tells you [how to create a migration and model](https://edgeguides.rubyonrails.org/getting_started.html#creating-the-article-model), but doesn't really dive into how to modify it. Initially, when I setup my Animal model/table, I added a column called 'image' that was intended to store an image file's contents for the animal. Not the most graceful way to storage an image file, I know, but I wanted something that I thought would be quick and dirty. 

After stumbling upon [how to do file uploads using the Active Storage feature](https://edgeguides.rubyonrails.org/active_storage_overview.html#attaching-files-to-records) (which was super easy to do), I realized that my Animal model didn't need an image column anymore. Active Storage would handle the storage of the image for me. All I had to do was modify the Animal model in the animal.rb file to associate the model with the image in the Active Storage. The thing is, the Getting Started guide doesn't tell you how to remove a column from the table that gets created when you generate a model. [This guide](https://guides.rubyonrails.org/active_record_migrations.html#creating-a-standalone-migration) however does tell you how to do that using a standalone migration. I used the command "rails generate migration RemoveImageFromAnimals image:binary". That command created the correct migration. After I ran "db migrate", the column was successfully removed from the Animals table.

<h3>Displaying an Image Stored in Active Storage</h3>
I had reached the point in the Getting Started guide where we [display the newly created record](https://edgeguides.rubyonrails.org/getting_started.html#showing-articles) in the show.html.erb file. I was straying outside the lines on these guides, so I don't fault them for not having this information explicitly layed out exactly the way I expected it to be in their guides, but it wasn't super obvious how to display the image I had just saved to Active Storage. I found the solution I needed pretty quickly on [Stack Overflow](https://stackoverflow.com/a/50779147/2754718). Here is the code I added to the show.html.erb file to display the image from the @animal variable (the key part being the url_for() helper):

```
<p>
  <span>Image:</span>
  <%= image_tag url_for(@animal.image) %>
</p>
```

[The url_for helper is listed in the Active Storage guide](https://edgeguides.rubyonrails.org/active_storage_overview.html#linking-to-files), but connecting that together in the show.html.erb file wasn't something I got right away for whatever reason.