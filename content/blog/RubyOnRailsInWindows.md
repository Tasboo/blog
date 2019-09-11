---
title: Getting started with Ruby on Rails in Windows
date: 2019-09-09T1904-07:00
---

A few days ago, I came across a blog post by [Scott Hanselman](https://twitter.com/shanselman) on  [how to get Ruby On Rails working in Windows](https://www.hanselman.com/blog/RubyOnRailsOnWindowsIsNotJustPossibleItsFabulousUsingWSL2AndVSCode.aspx). In the post, Scott goes through several steps in how to get a Ruby on Rails environment setup on Windows, using the latest features that Windows has to offer. I decided to give it a try, but I ran into some issues getting everything setup. I could see these issues causing some people a lot of frustration. [Cassidy Williams](https://twitter.com/cassidoo) has been pretty inspiring in her posts especially when it comes to getting the motivation to write about your experiences, so I decided to give that a try as well.

<div>
        <ul>
            <li><a href="#Getting-started-with-Ruby-on-Rails-in-Windows">Top</a></li>
            <li><a href="#assumptions">Assumptions, pre-reqs, and personal background</a></li>
            <li><a href="#windowsinsider">Windows Insider Program</a></li>
            <li><a href="#wsl2">Windows Subsystem for Linux v2 (WSL2)</a></li>
            <li><a href="#windowsterminal">Windows Terminal</a></li>
            <li><a href="#installror">Installing Ruby on Rails</a></li>
            <li><a href="#installpostgres">Installing Postgresql</a></li>
            <li><a href="#createrailsapp">Creating a New Rails App</a></li>
            <li><a href="#railsvscode">Getting to the Rails App in Linux from Visual Studio Code on Windows</a></li>
            <li><a href="#explorer">Explorer.exe and the WSL</a></li>
            <li><a href="#dbconfig">Final Database Configuration and Setup</a></li>
            <li><a href="#runningrails">Running the Rails App</a></li>
            <li><a href="#wrapup">Wrapping Up</a></li>
            <li><a href="#next">Next steps</a></li>
        </ul>
        <h3 id="assumptions">Assumptions, pre-reqs, and personal background</h3>
        <p>As with any how-to in the dev community, there are some pre-reqs and some assumptions in mine. They are:
            <ul>
                <li>Have access to a computer.</li>
                <li>Have a valid copy of Windows 10 installed on said computer.</li>
                <li>The ability to install Preview versions of Windows on that machine.</li>
                <li>Administrator priveleges on that Windows install.</li>
                <li>Have an internet connnection.</li>
                <li>Some knowledge on how to use the command line in Windows/Linux.</li>
                <li>Install <a href="https://code.visualstudio.com/" target="_blank">Visual Studio Code</a></li>
                <li>Have a <a href="https://github.com/" target="_blank">GitHub</a> account already setup.</li>
                <li>Know that having to google things and being confused is normal when learning something new 😊.</li>
            </ul>
        </p>
        <p>Some personal info about my background going into this:
            <ul>
                <li>I've been coding for over a decade.</li>
                <li>I code mostly .Net stack, along with front end development.</li>
                <li>I have used other programming languages in limited capacities in the past, including PHP and Go.
                </li>
                <li>I have attempted to start coding in Ruby on Rails in the past a couple times, all failures.</a>
                <li>I have some vague familiarity with what Ruby on Rails is due to Asp.net MVC being heavily
                    influenced by Ruby on Rails and being curious about Ruby on Rails in general.</a>
                <li>I have a small understanding of what the Windows Subsystem for Linux is, but I have never
                    attempted to use it or understand it outside of reading a few blog posts.</a>
                <li>I have virtually no understanding of postgresql, though I have a lot of experience using MS SQL
                    Server and some knowledge of MySql and Sqlite.</a>
            </ul>
        </p>
        <h3 id="windowsinsider">Windows Insiders</h3>
        <p>The first thing Scott mentions after his opening paragraph is to get a recent version of Windows. This
            does not mean make sure you have Windows updated. As of the time of this writing, it means get a preview
            version of Windows. The preview version of Windows has version 2 of the Windows Subsystem for Linux
            (detailed more below). If I was going to follow what Scott was doing, I needed to start with getting
            that preview version of Windows. That meant becoming a Windows Insider.
        </p>
        <p>Becoming a Windows Insider is as simple as filling out a simple form. You'll need a Microsoft account of
            some sort, but that is a pretty low barrier to entry. The sign up process is rather easy.
        </p>
        <p>The tricky part, for me at least, was actually getting the preview editions of Windows installed. Once I
            was signed up, I needed to sign in to my computer with my Microsoft account, which I had never done
            before. Easy enough.
        </p>
        <img src="/images/AccountSettings.png" />
        <p>Once you do that <a href=" https://insider.windows.com/en-us/welcome-back/" target="_blank">you are
                supposed to</a> be able to go to the Windows Insider Program Settings in Windows and
            download a preview version of Windows. In the Windows Insider Program Settings, I see a button that says
            'Get Started'.
        </p>
        <img src="/images/WindowsInsidersInstructions.png" />
        <p>I could not get the 'Get Started' button to work. Everytime I clicked on it, I got one of two errors.
            Googling these errors, you get a wide variety of solutions and suggestions of things to try. These error
            messages seem to be related to Windows Updates in general, so I go to the Windows Update settings in
            Windows to see if that works. Sure enough, Windows Update works fine. My computer is completely up to
            date.
        </p>
        <img src="/images/Something went wrong.png" />
        <img src="/images/somethingwentwrong2.png" />
        <p>After many attempts to get this button working, I threw in the towel. Turns out, you don't need to have
            that working to get the preview edition of Windows installed. The ISOs are available to download in the
            Windows Insiders site. So that's what I ended up doing and it was super simple.
        </p>
        <p>So my advice is, if you see those error messages when clicking that 'Get Started' button, don't even
            bother trying to fix it unless you really really want to. Just go to the website and download the ISO
            manually. You'll save yourself a lot of time and frustration.
        </p>
        <p>Once the ISO is downloaded, you just need to right click it from an explorer window, select Mount, and
            then click the setup.exe file. Follow the simple setup instructions and after a while the Windows
            preview edition will have finished installing.
        </p>
        <img src="/images/Windows10preivew.png" />
        <h3 id="wsl2">Windows Subsystem for Linux v2 (WSL2)</h3>
        <p>Now that the newest preview edition of Windows was installed, I was able to get the Windows Subsystem
            for Linux up and running with Ubuntu 18.04. That was pretty painless. As Scott pointed to in his
            blog, follow these instructions for <a href="https://docs.microsoft.com/en-us/windows/wsl/install-win10"
                target="_blank">setting up WSL2</a> and you'll
            be golden.
        </p>
        <h3 id="windowsterminal">Windows Terminal</h3>
        <p>Scott suggests installing Windows Terminal, but after installing it and trying to use, it still feels
            a bit raw to me, particularly the settings configuration. That's not a knock on JSON or JSON based
            configuration, but having your settings all defined via JSON with virtually no guidance on what to
            do in that JSON means me investing time into something that I really don't need to do.
        </p>
        <p>For my terminal, launching the Ubuntu LTS install on Windows brings up a fine enough terminal window.
            No fuss, just works. Scott does have a guide on how to get the Windows Terminal all customized, but
            that seemed like a tangent that I did not want to follow. Maybe another day.
        </p>
        <h3 id="installror">Installing Ruby on Rails</h3>
        <p>At this point, I feel like I'm ready to get cooking: I can finally start installing Ruby on Rails
            😁😁😁. Scott's article links to this page on <a href="https://gorails.com/setup/ubuntu/18.04"
                target="_blank">how
                to install Ruby on Rails on Ubuntu 18.04</a>. I have to say, the instruction provided on that
            site for installing Ruby on Rails are pretty spot on. I only had a couple issues with their
            instructions, but that was only with the Postgresql portion of the instructions (those issues are
            detailed below). Getting Ruby on Rails itself installed went super well. One small note is that at
            one point, one command (I can't remember which) takes a particularly long time to finish running.
            That's not really an issue, but the thing that made me nervous was that it didn't output anything to
            the terminal window for a long time. It made me think something had gone wrong. I eventually just
            let it be and walked away for a while. After an hour or so I came back to my computer and it had
            finished doing whatever it was doing without an issue.
        </p>
        <h3 id="installpostgres">Installing Postgresql</h3>
        <p>The issue I ran into with the Postgresql was that I didn't have the necessary repositories installed
            in the Ubunutu instance to get the packages for installing Postgresql. The instructions on the
            install page for Ruby on Rails page did not detail how to do that, <a
                href="https://tecadmin.net/install-postgresql-server-on-ubuntu/" target="_blank">but another page I
                found
                did.</a>
        </p>
        <p>In case that page disappears, the commands for installing the repositories are:<br />
            sudo apt-get install wget ca-certificates<br />
            wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -<br />
            sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >>
            /etc/apt/sources.list.d/pgdg.list'<br />
        </p>
        <p>I only followed Step 1 on that page. After completing Step 1, I went back to the Ruby on Rails
            install steps. I was still getting issues trying to get the 'sudo apt install postgres-11
            libpq-dev' command to work. Turns out I had to run 'sudo apt-get update' in order for the newly
            installed repository to be registered or something like that.
        </p>
        <p>After I ran that, I ran into another issue with the Postgres install steps. The 'createuser'
            command came back at me with the error message 'createuser could not connect to database
            postgres no such file or directory'. ???
        </p>
        <p>I tried 2 commands in an attempt to get around this error. First I tried this commandI tried is
            specified in the output when you run 'sudo apt install postgres-11 libpq-dev': pg_ctcluster 11
            main start.
        </p>
        <p>Running the 'pg_ctcluster 11 main start' command returned the error 'Error: You must runt his
            program as the cluster owner (postgres) or root'. I had no idea what that meant, so I did some
            googling on the original '.. no such file or directory' error message. I eventually came across
            the command 'sudo service postgresql start'. After running that command, the createuser command
            worked fine.
        </p>
        <img src="/images/PostgresTerminalLog.png" />
        <h3 id="createrailsapp">Creating a New Rails App</h3>
        <p>I was now at the point in the instructions that said to create a new rails app. The instructions
            have 3 commands: one for sqlite, one for mysql, and one for postgresql. I used the postgresql
            command. This command took a few minutes to finish. Eventually it finished. The instructions at
            this point say to update the database config file to include the user/password info that you
            provided when setting up postgresql for the postgresql db connection. I figured this would be a
            good time to hop out of the Ubuntu terminal and test the WSL2 functionality from VS Code.
        </p>
        <h3 id="railsvscode">Getting to the Rails App in Linux from Visual Studio Code on Windows</h3>
        <p>After opening Visual Studio Code on my Windows install, I installed the <a
                href="https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl"
                target="_blank">Remote-
                WSL</a> extension. After installing that extension I clicked on this little green icon at
            the bottom-left of the VSCode window. Navigating through the provided options that popped up, I
            was able to select the Ubuntu-18.04 distro.
        </p>
        <img src="/images/WSLRemoteIcon.png" />
        <img src="/images/ExtensionRemoteOptions.png" />
        <img src="/images/ExtensionRemoteDistro.png" />
        <p>Clicking the Ubuntu distro caused VSCode to open a new VSCode window. This new window seemed to
            be attempting to connect to the Ubuntu instance I had selected. Sure enough, after a few
            seconds, I was able to select the directory of the myapp that I had created, just as if I was
            running that VSCode window on a Linux instance. That is pretty crazy.
        </p>
        <img src="/images/successmyapp.png" />
        <h3 id="explorer">Explorer.exe and the WSL</h3>
        <p>Following all this I go back to Scotts post. He says, from the terminal window, I can run
            "explorer.exe ." to open an explorer window. Pay special attention to the space and period at
            the end of that command. Without that space and period, it will just open an explorer window
            with its default starting location for your Windows install. it took me a minute to notice that
            period. After entering the full command, I was able to see my Ubuntu directory just fine.
        </p>
        <h3 id="dbconfig">Final Database Configuration and Setup</h3>
        <p>At this point, I'm almost ready to start running the Rails app, but there is still a couple
            things left to do. The config/databases.yml file in the Rails app needed to be updated with the
            credentials that I had entered when setting up the postgresql DB. I update the dev, test, and
            production databases in the yml file to have my user/password creds.
        </p>
        <p>The last thing that needs to be done before I run the app is to run the 'rake db:create' in the
            myapp directory in the Ubuntu terminal. I do that and get the following output. Seems like the
            dev and test DBs got created. Got a couple of warnings, but they don't seem like big deals to me
            at this point.
        </p>
        <img src="/images/postgresdbcreate.png" />
        <h3 id="runningrails">Running the Rails App</h3>
        <p>Per Scott's suggestion I run the rails app using 'rails server -b=0.0.0.0' which will allow me to
            navigate to the site using localhost:3000. I did this from the Ubuntu terminal. After running
            the command, everything worked. I could browse to http://localhost:3000 on my windows machine
            and see the website running!
        </p>
        <img src="/images/railsrun.png" />
        <h3 id="wrapup">Wrapping Up</h3>
        <p>At this point, I feel like I can really start my Ruby on Rails learning. I want to extend a huge
            thanks to the following:
            <ul>
                <li>To Scott Hansleman for his post on how to get this setup.</li>
                <li>To the GoRails team for their easy to follow instructions.</li>
                <li>To the people working on WSL and VSCode for making 'magic' happen.</li>
                <li>To Cassidy Williams for motivating me to write about things.</li>
            </ul>
        </p>
        <h3 id="wrapup">Next Steps</h3>
        <p>My next steps are not well thought out at the moment. Once I decide on what to do next, I'll
            document what I do as I do it and post about it here.
        </p>
    </div>