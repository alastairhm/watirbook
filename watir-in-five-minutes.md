# Watir in Five Minutes

In this chapter I assume you have watir-webdriver gem and Firefox browser installed, since that is the combination of the only Watir gem that is available on all platforms, and a popular browser, also available on all platforms.

_An example would be handy right about now_ Brian Marick would say. Five minutes from now, you will be crazy about Watir. If not, maybe it is not the right tool for you. Even then, if you continue reading the book, I hope you will grow to like it the more you know about it.

In the rest of the book, I will explain all Watir functionality in great (maybe even painful) detail, but for now, I just want to show off a few cool features to get you excited about it.

I still remember how pleasantly surprised I was the first time I saw Watir at work. I installed it, and in a few hours I was able to create a script that would log me into the web application I was testing. Since you have this book you will be able to log into your web site in minutes, not hours.

If you are familiar with Ruby, I am sure you already think IRB is one of the greatest tools for learning a new Ruby library.

If you are new to Ruby, you are probably thinking: _What is this IRB thing?_

IRB (in this case) does not stand for _International Rugby Board_ or _Immigration or Refugee Board_ (of Canada). It stands for _Interactive Ruby Shell_. Think of it as a shell that knows Ruby (as the name says).

To start IRB, just type `irb` in command line. You will see something like this (on Mac):

    $ irb
    >> 

It should look similar on Windows or Linux.

Now you can enter any Ruby command and you will immediately get a result. We will start with telling Ruby that we want to use RubyGems and watir-webdriver gem:

    require "rubygems"
    require "watir-webdriver"

You should see something like this:

    require "rubygems"
    => false
    require "watir-webdriver"
    => true

Every Ruby command returns something. After `require "rubygems"` you will get either `true` or `false`, depending how your Ruby installation is set up. You do not have to care about that right now. You should get `=> true` after `require "watir-webdriver"`. There are two parts in the returned line. The first one is `=>`. It looks like an arrow. It means `Ruby returned this`. The second part is `true`, the thing that is actually returned. When `true` is returned, it usually means that everything is fine.

Unless I say differently, just ignore what is returned, for now.

And now the magic starts. With just one command you will open Firefox.

    browser = Watir::Browser.new :ff

![watir-webdriver driving Firefox 4 on Mac OS 10.6](images/mac-10-6-webdriver-firefox-watir5.png)\

*watir-webdriver driving Firefox 4 on Mac OS 10.6*

When I saw the browser magically appearing for the first time, I started singing (with apologies to [Foreigner]): _I've been waiting for a tool like you to come into my life..._

Please open just one browser. It will be enough. You can play with other browsers later.

Output should be similar to this:

    browser = Watir::Browser.new :ff
    => #<Watir::Browser:0x2ed1f1cd5b186306 url="about:blank" title="">

As I said earlier, you can ignore `#<Watir::Browser...>`. Opening Firefox returned the browser as an object, and this is textual representation of the object.

Just opening a browser is cool, but not so useful. Watir can do much more. For example, it can navigate the browser to any site. I will use google.com in this example. I suggest that you literally follow the example, and then try a few sites yourself.

So, go to google.com:

    browser.goto "http://www.google.com/"
    => "http://www.google.hr/"

And google.com opens. Magic, isn't it?

Controlling the browser is really useful but, as I am sure you already know, there is more to testing than just performing the actions. You have to check what happened after the action. What happens when I enter a URL in browser address bar, when I click a link or a button, when I enter some text in a text field or select something from select box...?

This is the first time we will perform a check. It is also the first time we will take a look what Ruby returns after the command. Let's check if the browser really opened google.com.

    browser.url
    => "http://www.google.hr/"

It really works! Ruby returned a string (the thing in double quotes) that contains the text from the browser address bar. Since I am in Croatia, google.hr opened. If you are not in the US, some other Google site could open.

Time to click on a link. It is easy to explicitly say which link to click on. Right now I want to click on a link with the text `Google.com in English`. It gets just a bit more complicated if there are two links with the same text on the page, but we will deal with that later. If your browser already opened google.com, ignore this step. 

    browser.link(:text => "Google.com in English").click
    => []

And google.com opens.

Before we click another link, I want to show off one of Watir's killer features. It is called `flash`. Real world web applications are complex, and sometimes when you are developing a new test or debugging an existing one, you want to make sure you are interacting with the correct element. Try this (and look at `Images` link in the top-left corner of google.com):

    browser.link(:text => "Images").flash
    => 10

![`Images` link with normal background](images/flash-1.png)\
![`Images` link with red background](images/flash-2.png)\

*`Images` link changes background from white to red a few times.*

Did you see the link flashing? It's background color changes to red a few times. Isn't that cool? I use it all the time when I present Watir at conferences. I think Watir is just great for presentations. It is very visual.

If you did not see it (it flashes just for a short time), execute the command a few more times. To execute the same command in command prompt again, just click up arrow key on your keyboard and the last command that you have typed will appear.

It is time to click the link:

    browser.link(:text => "Images").click
    => []

This time, let's check the page title.

    browser.title
    => "Google Images"

We got back the string with the page title.

Let's search for something. This will enter `book` in search text field:

    browser.text_field(:name => "q").set "book"
    => ["book"]

Maybe you are wondering how I knew the text field had the value of `name` attribute set to `q` (I am talking about `:name => "q"`). If you do not know how to inspect pages, read on. I will explain it later.

Now, click `Search Images` button:

    browser.button(:value => "Search Images").click
    => []

Page with search results will open. Let's check how many images are on the page.

    browser.images.size
    => 242

And finally, let's close the browser.

    browser.close
    => true

Well, that was a lot of fun. But you do not want to type into IRB all the time. You want to run the tests and do something else while it runs. As for almost everything else in Ruby and Watir, there is a simple solution. Paste all code you have entered in IRB in a text file, and save it with `rb` extension. IRB is used only for development or debugging, so do not paste `irb` as the first line of the file. The file should look like this:

    require "rubygems"
    require "watir-webdriver"
    browser = Watir::Browser.new :ff
    browser.goto "http://www.google.com/"
    browser.url
    browser.link(:text => "Google.com in English").click
    browser.link(:text => "Images").click
    browser.title
    browser.text_field(:name => "q").set "book"
    browser.button(:value => "Search Images").click
    browser.images.size
    browser.close

You can use any text editor to edit the file. I use [NetBeans].

Save the file as `watir5.rb`. To run it, navigate in command prompt to the folder where you have saved it and type `ruby watir5.rb`. If IRB is still running in your command prompt, enter `exit` to return to normal command prompt, or open a new command prompt.

You can remove clicking `Google.com in English` link if Firefox opens google.com automatically for you.

Execute `watir5.rb`. Smile while the browser clicks around.

What is the output in the command prompt? Nothing? Yes, nothing. IRB displays values that Ruby returns, but when you execute Ruby file from the command line, it does not display the values Ruby returns. You have to explicitly say to Ruby that you want them displayed. It is as easy as adding `puts` in front of the command. Modify the script to look like this (you can add puts in front of every command, but you really do not care about what some commands return):

    require "rubygems"
    require "watir-webdriver"
    browser = Watir::Browser.new :ff
    browser.goto "http://www.google.com/"
    puts browser.url
    browser.link(:text => "Google.com in English").click
    browser.link(:text => "Images").click
    puts browser.title
    browser.text_field(:name => "q").set "book"
    browser.button(:value => "Search Images").click
    puts browser.images.size
    browser.close

Run the script. This time the output should look like this:

    $ ruby watir5.rb
    http://www.google.hr/
    Google Images
    246

Later I will show you how to make cool looking reports.

If you are not impressed by now, you probably never will. If you liked what you saw so far, it is time to bring on heavy artillery. Let's go deeper.

[Foreigner]: http://www.youtube.com/watch?v=BrzzR-3PPqw
[NetBeans]: http://netbeans.org/

\newpage

