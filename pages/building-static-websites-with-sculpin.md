---
title: Building static websites with Sculpin
timestamp: 2026-11-26T08:30:01
author:
published: true
description:
tags:
---

Static websites are quick to build, inexpensive to host, and easy to maintain.

The pages are generated in advance and served as static files.

[Sculpin](http://sculpin.io/) is a PHP static site generator that turns Markdown and [Twig](https://twig.symfony.com/) templates into static HTML.

This talk will show how to build a static website with Sculpin, from creating content and templates to generating and deploying the finished site.

The examples focus on Sculpin, but the principles apply to static site generators more broadly.

## Bio

[Oliver Davies](https://www.oliverdavies.uk/) is a Software Developer and Drupal expert with 19 years experience. He specialises in code quality, automated testing and test-driven development.

## Notes and links

* [Source code](https://code.oliverdavies.uk/sculpin-demo/commit/?h=20261125-php-maven) created during the presentation.
* [No AI in DuckDuckGo](https://noai.duckduckgo.com/) skips the AI stuff. Mentioned during the chat.
* [Speaking about Sculpin at PHP Maven](https://www.oliverdavies.uk/blog/speaking-about-sculpin-php-maven) the announcement Oliver made about this event.
* [Packagist](https://packagist.org/) The PHP Package Repository
* [Install composer](https://getcomposer.org/)
* [nix](https://nix.dev/)
* [PHPSW](https://phpsw.uk/) PHP South West UK. Bristol.


{% youtube id="-UJef7EXOiY" file="2026-11-25-Building-static-websites-with-Sculpin-Oliver-Davies.mp4" %}

## Timestamps

* 00:00 Gabor Szabo: introduction
* 01:40 Oliver Davies starts the presentation
* 06:40 sculpin.io
* 07:20 diagrams
* 08:22 Static site generators
* 09:55 Installing Sculpin `composer require sculpin/sculpin`
* 11:03 Q: What is composer?
* 12:27 Installing Sculpin with Nix
* 13:47 Run `sculpin` command to show the version number.
* 14:22 Sculpin 4
* 15:11 Q: How long does it take to build your site?
* 17:30 Start of the coding demo
* 18:50 PHPSW web site.
* 19:27 `sculpin init`
* 23:30 `sculpin generate`


## Transcript

1
00:00:02.140 --> 00:00:06.660
Gabor Szabo: So, hi and welcome to this session, this Code Maven session.

2
00:00:06.970 --> 00:00:26.910
Gabor Szabo: I would like to thank, first of all, Oliver, who agreed to give this presentation, and to the audience who are present, who arrived to this meeting, and obviously to you who are watching this video on YouTube. Below the video, you will find links to our future events, and

3
00:00:26.990 --> 00:00:40.729
Gabor Szabo: notes and links about this presentation, and obviously, it would be nice if you liked the video and followed the channel as well, but we definitely are interested also if you join us

4
00:00:40.740 --> 00:00:52.489
Gabor Szabo: at upcoming events. My name is Gabor. I organize these events, and you will see a lot of these events in this link.

5
00:00:54.180 --> 00:01:08.330
Gabor Szabo: People who are here, feel free to ask questions, and after the video recording is stopped, then we can continue having a free conversation, and that's why it's good for… to join these presentations.

6
00:01:10.130 --> 00:01:21.749
Gabor Szabo: Anyway, oliver, I see that you have some… you put yourself there. We are going to have a little of you, yourself, on the top right corner as well. So, if you

7
00:01:21.880 --> 00:01:32.840
Gabor Szabo: It's fine, I don't know how to turn that off right now, so anyway, we… that's fine. So the stage is yours, that's what I usually try to say.

8
00:01:32.960 --> 00:01:37.680
Gabor Szabo: Go ahead, just unmute yourself, please.

9
00:01:38.320 --> 00:01:56.340
Oliver Davies (opdavies): Okay, awesome. Yeah, I didn't want to interfere whilst you were doing your, your introduction, so… No problem. That's fine. Cool. So hopefully you can see my screen, and you can see the slides. So, I have a few… only a few slides, and then we'll jump into some live coding, and we'll see where we go from there.

10
00:01:57.180 --> 00:02:09.950
Oliver Davies (opdavies): So, this presentation is Building Static Websites with Scalpin. I'm Oliver Davies, and I'm OP Davis, mostly everywhere online, primarily GitHub, Drupal.org, Cobra, etc.

11
00:02:09.949 --> 00:02:20.430
Oliver Davies (opdavies): My website is oliverdavis.uk, which is powered by sculping, which we'll get onto, and you can find a link to this talk, at slash sculpin.

12
00:02:22.010 --> 00:02:35.929
Oliver Davies (opdavies): So, speaking about my website, it started on Drupal 6, and the first post I did was in 2010. I've been primarily a Drupal developer since 2007, or 8.

13
00:02:35.940 --> 00:02:47.070
Oliver Davies (opdavies): Really, and then my first full-time role was in 2010. So, interestingly, it's around about the same sort of time when I started doing blogging and everything else is when I moved into it full-time.

14
00:02:47.980 --> 00:02:51.990
Oliver Davies (opdavies): And then 2015, I moved on to sculpting, and…

15
00:02:52.000 --> 00:03:06.649
Oliver Davies (opdavies): Reasons for that was around, Drupal 8's release time, so I wanted to learn some of the new things that were coming in in Drupal 8 that I wasn't really aware of before this. So, in Drupal 7, the template is all done in PHP.

16
00:03:06.650 --> 00:03:12.309
Oliver Davies (opdavies): And the configuration was done in PHP, so there was no twig at that point.

17
00:03:12.330 --> 00:03:21.730
Oliver Davies (opdavies): So, I wanted to learn Twig and YAML, ready for Drupal 8, but in more focused ways, so I didn't necessarily

18
00:03:21.740 --> 00:03:36.870
Oliver Davies (opdavies): want to learn the whole thing of Drupal 8 straight away, because there was object-oriented decode and loads of other new things in there at the time. And by this point, I was also using Jekyll, which is another static site generator, and

19
00:03:37.120 --> 00:03:48.269
Oliver Davies (opdavies): I had one, I, being a PHP dev, wanted, would prefer to have something in PHP that I would… could know and contribute to and alter if I wanted to.

20
00:03:49.780 --> 00:03:58.050
Oliver Davies (opdavies): So, I was looking for something that would make it quick to build, and easy to deploy, and either cheap or ideally free to host.

21
00:03:58.500 --> 00:04:04.909
Oliver Davies (opdavies): I remember using shared hosting, with cPanel and things, and drag and drop files around.

22
00:04:04.960 --> 00:04:24.779
Oliver Davies (opdavies): as my first version. Then moved to a virtual server, and of course, with Drupal and WordPress and those, you need PHP, and you need, databases, etc. So you need to install and maintain all of that, infrastructure just to run, for me, a fairly simple site.

23
00:04:25.770 --> 00:04:38.719
Oliver Davies (opdavies): Now it's hosted on my home lab, which is on the shelf behind me, so I was looking for something sort of minimal, that I can host easily, and get the most performance, benefit out of.

24
00:04:38.770 --> 00:04:54.749
Oliver Davies (opdavies): And because we all know about the recent, PHP security, issues, so it's better the fact I've got less stuff running on my server, on my overlap. There's a smaller, security, vulnerability.

25
00:04:54.950 --> 00:04:57.989
Oliver Davies (opdavies): potential there, because I'm running less stuff.

26
00:04:58.990 --> 00:05:18.980
Oliver Davies (opdavies): And then, on my site, it's currently showing 1,228 published posts, and 1764 index.html files. So, it's not really a small site. I feel like I'm pushing, the limits of things in some ways, sometimes, but…

27
00:05:19.020 --> 00:05:19.910
Oliver Davies (opdavies): There we go.

28
00:05:21.290 --> 00:05:36.569
Oliver Davies (opdavies): These are a selection of some of the files on my server, so everything is HTML. Interestingly, we were just talking about GitHub pages and static site hosting a minute ago before we started recording.

29
00:05:36.570 --> 00:05:46.039
Oliver Davies (opdavies): So everything on the server is a .html file, or there's some CSS, but there's no PHP.

30
00:05:46.040 --> 00:06:03.289
Oliver Davies (opdavies): There's no database, there's just, static files when you're on the server. So, everything… every page you go to on the website is a directory, or a folder, and inside that folder is an index.html file.

31
00:06:03.980 --> 00:06:15.620
Oliver Davies (opdavies): And when you go to that page, the web server will look either for a blog.html or an index.html inside a blog folder. So…

32
00:06:15.790 --> 00:06:32.999
Oliver Davies (opdavies): instead of having one sort of index.php that everything goes through, it's set up in this way instead, which works on any web server. So, things like, GitHub pages, or, Cloudflare pages, or Netify, like, works on any of those things.

33
00:06:33.170 --> 00:06:38.040
Oliver Davies (opdavies): And the PHP is purely kept, local for local development.

34
00:06:40.180 --> 00:06:42.729
Oliver Davies (opdavies): So, sculpting. Sculpting…

35
00:06:42.930 --> 00:06:54.879
Oliver Davies (opdavies): helps you build websites, by combining Markdown content and Twig templates into static HTML files, like we just saw. They can be deployed to any web host, because it's just…

36
00:06:54.880 --> 00:07:05.920
Oliver Davies (opdavies): HTML at this point, right? You don't need, a big, fancy, hosting setup, and, you know, say, PHP, and a database, and Kubernetes, and,

37
00:07:06.070 --> 00:07:18.399
Oliver Davies (opdavies): load balance, everything, right? It's, mine is hosted on a simple, CADI server, on a home lab, but equally it works with, say, GitHub pages, or any of those sorts of things as well.

38
00:07:18.990 --> 00:07:20.500
Oliver Davies (opdavies): Which is one of the big benefits.

39
00:07:22.480 --> 00:07:32.870
Oliver Davies (opdavies): So, static site generators, typically work in this way. You have some source files, then yours, in this case, is probably going to be Markdown, or Twig.

40
00:07:33.280 --> 00:07:44.709
Oliver Davies (opdavies): also HTML. The static side joiner lives in the middle, so you pass the source files into the generator, the generator generates stuff, and then you get static HTML out at the end.

41
00:07:45.000 --> 00:07:47.390
Oliver Davies (opdavies): From a scoping perspective.

42
00:07:47.500 --> 00:07:59.799
Oliver Davies (opdavies): Let's say our source files are going to be, MD files, so markdown for pages, for blog posts, etc. They could be HTML files, twig templates.

43
00:08:00.030 --> 00:08:04.100
Oliver Davies (opdavies): And there's some YAML files in there as well, for things like configuration.

44
00:08:05.190 --> 00:08:12.809
Oliver Davies (opdavies): And you have your normals of CSS, JavaScript, image files, etc, all live within your source folder.

45
00:08:13.240 --> 00:08:22.120
Oliver Davies (opdavies): Sculping then sits in the middle as the site generator, sucks in all the source files, and outputs the .html files, and you're assets at the end.

46
00:08:24.810 --> 00:08:32.149
Oliver Davies (opdavies): So, there's a lot of static site generators, there's a whole website devoted to different static site generators.

47
00:08:32.250 --> 00:08:44.900
Oliver Davies (opdavies): The most popular ones at the moment, probably Hugo, which is written in Go. Jekyll is one of the original, or earlier ones that's written in Ruby, it's what GitHub Pages uses.

48
00:08:45.090 --> 00:08:51.190
Oliver Davies (opdavies): Things like Astro 11T, have been around for a while as well, they're JavaScript-based.

49
00:08:51.330 --> 00:08:53.320
Oliver Davies (opdavies): The song was written in Rust.

50
00:08:53.620 --> 00:09:05.140
Oliver Davies (opdavies): There is one for Drupal, so you can export your Drupal site to a static HTML called Tome. And yep, there's lots of these site challenges that do the same…

51
00:09:05.250 --> 00:09:07.519
Oliver Davies (opdavies): Same sort of thing, basically.

52
00:09:08.230 --> 00:09:15.379
Oliver Davies (opdavies): However, my sort of take on it is Sculpin is the static site generator for PHP developers.

53
00:09:15.530 --> 00:09:22.099
Oliver Davies (opdavies): Again, there are various other PHP CMSs, like Drupal or WordPress, that you could use.

54
00:09:22.130 --> 00:09:36.699
Oliver Davies (opdavies): We know about frameworks, like Symphony and Laravel, and there are other site generators as well. Jigsaw is one if you're coming from a Laravel-based approach, so if you're using Blade templates and things.

55
00:09:36.760 --> 00:09:52.309
Oliver Davies (opdavies): But if you're primarily a Symphony developer or a Drupal developer, a lot of the… it's built on Symphony components, so a lot of it is going to be familiar, and of course, if you learn Twig in one place, you know Twig, and all the other places that it's used.

56
00:09:52.420 --> 00:09:53.200
Oliver Davies (opdavies): As well.

57
00:09:55.130 --> 00:10:06.249
Oliver Davies (opdavies): So, how do we install it? It's a PHP package, so we can install it through Composer. It's just sculpting, sculpting on Packages, so we can… Compose requires sculpting, sculpting.

58
00:10:06.420 --> 00:10:15.960
Oliver Davies (opdavies): You can add it per project into a Compose a JSON file for that project, or you can sort it globally, I think, still.

59
00:10:16.290 --> 00:10:19.359
Oliver Davies (opdavies): Unless you've got conflicts and things, but I think you can still do it.

60
00:10:19.540 --> 00:10:29.709
Oliver Davies (opdavies): There are, some built-in, some blog skeletons that are available as official ones, like the sculpian slash blog skeleton.

61
00:10:30.100 --> 00:10:47.570
Oliver Davies (opdavies): That we can use, and I maintained one, on my side, but since I've moved it to, Seagit and off, GitHub, that may or may not still work, so we can keep that stew out of there for now, but we can try it, and if it doesn't work later on, I will…

62
00:10:47.810 --> 00:10:56.169
Oliver Davies (opdavies): update this slide. We'll see. It's just, but it's just a more minimal, sculpting, skeleton than the one I use.

63
00:10:57.470 --> 00:10:58.550
Oliver Davies (opdavies): So, that's your typical.

64
00:10:58.550 --> 00:10:59.220
Gabor Szabo: What are you gonna…

65
00:10:59.220 --> 00:11:00.350
Oliver Davies (opdavies): I have a quip.

66
00:11:00.350 --> 00:11:01.819
Gabor Szabo: I have a question, SR.

67
00:11:01.820 --> 00:11:02.470
Oliver Davies (opdavies): Yep, absolutely.

68
00:11:02.470 --> 00:11:11.999
Gabor Szabo: non-PHP developer. What is Composer? Is it like the tool to install packages that comes with PHP?

69
00:11:12.000 --> 00:11:15.239
Oliver Davies (opdavies): Yeah, exactly. So, you could go to Packages.

70
00:11:15.580 --> 00:11:26.590
Oliver Davies (opdavies): package.org, this is the PHP package repository, so you can type in sculpin, and it will find sculpin slash sculpin, which is the project we're talking about.

71
00:11:26.600 --> 00:11:43.700
Oliver Davies (opdavies): And it gives you the command, to run there, and gives you some stuff about the number of installs, etc. It's… if you're familiar with NPM for JavaScript, then… or Bundle for Ruby, it's the PHP equivalent of…

72
00:11:44.220 --> 00:11:44.620
Gabor Szabo: Okay.

73
00:11:44.620 --> 00:11:45.949
Oliver Davies (opdavies): It's how you pull in.

74
00:11:46.100 --> 00:11:51.600
Gabor Szabo: If you install PHP on your computer, then you'll have Composer, I guess, and then you can go. So that's the…

75
00:11:51.800 --> 00:11:53.020
Gabor Szabo: Beginning of this, right?

76
00:11:53.600 --> 00:12:05.620
Oliver Davies (opdavies): Yeah, get composer. Composer is a separate thing, so you can download it from here, and then that will give you, the Composer binary, and then you could just say, yeah, Compose require.

77
00:12:05.780 --> 00:12:07.399
Oliver Davies (opdavies): Whatever, but…

78
00:12:07.660 --> 00:12:19.250
Oliver Davies (opdavies): Most things are, I'd say, are probably Composer-based now. Drupal has been since Drupal 8, and of course, Symphony that are available based on, using Composer as is… as it's scoping.

79
00:12:19.740 --> 00:12:20.900
Gabor Szabo: Okay, thank you.

80
00:12:21.410 --> 00:12:22.670
Oliver Davies (opdavies): No problem.

81
00:12:24.000 --> 00:12:28.519
Oliver Davies (opdavies): So, yeah, Composer should be familiar for most PHP devs,

82
00:12:28.740 --> 00:12:42.489
Oliver Davies (opdavies): I could also install it with Nix. So Nix is a package manager that works across operating systems, so it works on macOS and Linux, and I maintain the scalping package on Nix packages.

83
00:12:42.690 --> 00:12:59.249
Oliver Davies (opdavies): So you could say next shell dash p for package sculpin, and it will make a shell with sculpting available. So, behind the scenes, it will download the right version of PHP, bundle it all up for you, and it'll just… just work, which is awesome.

84
00:13:00.240 --> 00:13:18.810
Oliver Davies (opdavies): Also got a couple of templates that I use. One is PHP-based, so it still uses Nix, but uses Nix to install PHP, and then PHP and Composer manage the site, or you can go full-on using Nix itself instead of Composer, so that also works.

85
00:13:19.650 --> 00:13:38.230
Oliver Davies (opdavies): And then something I've been doing for my own site recently, because my site is all wrapped in Nix as well, is building out a sculpting tools package with some helpers, like CreateSculpting site and create sculpting DevServer, which gives us a bit more structure and makes it easier to build, sculpting with Nix.

86
00:13:39.160 --> 00:13:48.489
Oliver Davies (opdavies): So, whichever way you want to install it, you'll end up with a sculpting binary on your project. Let's see… so…

87
00:13:48.750 --> 00:13:53.489
Oliver Davies (opdavies): I've got it here, right, sculpting. This is after you've installed it.

88
00:13:53.700 --> 00:14:11.000
Oliver Davies (opdavies): And then you'll get a version number. I've just fixed that in Nix packages, today, or yesterday. So now the actual version number shows up here, rather than dev, dev slash debug. And then these are all the options that you get from sculpting, so…

89
00:14:11.270 --> 00:14:17.939
Oliver Davies (opdavies): This is all based on, Symphony console component, so it should be pretty familiar to people.

90
00:14:20.960 --> 00:14:38.949
Oliver Davies (opdavies): And then, I think this is the last slide before we jump into some coding. So, scoping 4 is what we're currently working on. We're currently on version, 4.0.0 Alpha 4, so I'm gonna be doing some live coding on Alpha, so let's, we may find…

91
00:14:39.030 --> 00:14:56.900
Oliver Davies (opdavies): some things, but hopefully we'll be okay. I've been using it on my site for a little while, and then it's using PHP.5 as the minimum, and so we're going through a good modernization of sculpting internals whilst adding some new features.

92
00:14:57.090 --> 00:15:06.710
Oliver Davies (opdavies): better performance is something I'm looking at right now, how to use, like, all the cores on your machine to build faster, especially if I've got

93
00:15:06.880 --> 00:15:11.290
Oliver Davies (opdavies): 1,800 pages to build. I want to build that as quick as possible.

94
00:15:11.570 --> 00:15:15.009
Gabor Szabo: Really, how long does it take for you to build your site?

95
00:15:15.420 --> 00:15:34.569
Oliver Davies (opdavies): Mine, uncached takes about 30 seconds, because I have a lot of content, first of all, and I use, quite a lot of stuff in Twig as well, so you can use twig includes and things, so there's quite a lot for it to do, just based on the number of… on the amount of content.

96
00:15:34.750 --> 00:15:39.939
Oliver Davies (opdavies): With the patch I'm currently working on, it's down to about 12 seconds.

97
00:15:40.300 --> 00:15:48.889
Oliver Davies (opdavies): And… from a fresh build, so if you're watching for changes in the background, it's faster, but from nothing to build.

98
00:15:49.140 --> 00:16:05.730
Oliver Davies (opdavies): If it's cached, it's down to about 3 seconds, but of course, as soon as you change something, it invalidates the cache, so… I'm getting there with that at the moment, so… yeah. But most sites are very quick to build, it's just I've got so much…

99
00:16:05.880 --> 00:16:10.050
Oliver Davies (opdavies): content in there. It just takes a while to go through, go through everything.

100
00:16:11.170 --> 00:16:15.319
Gabor Szabo: Yeah, I'm thinking about, for example, GitHub pages. Sorry if the,

101
00:16:15.900 --> 00:16:24.019
Gabor Szabo: then probably the installation of Scalpin and all this stuff would take way more than this processing time, so…

102
00:16:25.130 --> 00:16:36.700
Oliver Davies (opdavies): Yeah, I mean, I… because I'm using Nix as a package manager anyway, I would just use that to make it available, and then the build time, I would do probably, like, a…

103
00:16:37.350 --> 00:16:56.929
Oliver Davies (opdavies): a pipeline, or what we do for the main sculpting docs is we build it into a docs folder, and then GitHub Pages looks at that docs folder. So, it doesn't really do any building, there, or you could do what I'm doing on my site and sort of deploy it as part of a pipeline, and then just the build site gets pushed up.

104
00:16:57.460 --> 00:16:58.010
Gabor Szabo: Beautiful.

105
00:17:00.080 --> 00:17:12.340
Oliver Davies (opdavies): Cool, and then, yeah, do some UX improvements to the CLI, and then doing various documentation updates, as we go, because documentation needs updating, and…

106
00:17:12.339 --> 00:17:23.270
Oliver Davies (opdavies): Found some recognices to sculpin.json, which is an old file we used to use, back when I started using Sculping, that doesn't get supported anymore, I don't think, but…

107
00:17:23.380 --> 00:17:26.719
Oliver Davies (opdavies): Yeah, I'm going soon documenting stuff.

108
00:17:28.130 --> 00:17:37.579
Oliver Davies (opdavies): So, let's see, so I've got Sculpin Demo set up, so this is just an empty directory. I have Sculping, installed.

109
00:17:37.580 --> 00:17:49.929
Oliver Davies (opdavies): And we've, we've seen, showed you that already. So, here we've got all the commands that we can run. So, we've got things like generate, that's the main one we're gonna want to run, generates the site from source.

110
00:17:49.990 --> 00:17:57.369
Oliver Davies (opdavies): We've got init that initializes a new site. I didn't know about that until quite recently, so I was doing it by hand.

111
00:17:57.490 --> 00:18:10.470
Oliver Davies (opdavies): Run is a new one, so that combines, serve, with a couple of other options. Run combines the options, sort of together. It opens the site up for you.

112
00:18:10.650 --> 00:18:20.580
Oliver Davies (opdavies): And… that's pretty much, about it. So, in this directory, we have… Let's see, we have…

113
00:18:20.970 --> 00:18:23.429
Oliver Davies (opdavies): Level, let's have a look. Level 2.

114
00:18:23.920 --> 00:18:29.339
Oliver Davies (opdavies): Yep, in this folder, all I've got is, some sample data, and…

115
00:18:29.910 --> 00:18:39.829
Oliver Davies (opdavies): We've got a Git directory, obviously. Some sample data that we'll come back to in a minute, but there's nothing else here in this directory tool. We are starting from scratch, other than

116
00:18:39.890 --> 00:18:52.930
Oliver Davies (opdavies): a gitignore file that's gonna ignore some things that are gonna run. So, we are starting from scratch, and the last time I gave this talk, I did an update of, or a version of.

117
00:18:53.210 --> 00:19:01.979
Oliver Davies (opdavies): This website, which is the PHP Southwest, website. So this is my local user group, PHP user group.

118
00:19:02.380 --> 00:19:03.430
Oliver Davies (opdavies): And…

119
00:19:04.010 --> 00:19:16.430
Oliver Davies (opdavies): Seeing as it's back online now, I think, again, we'll, I thought earlier on we'll, we'll see, yeah, again, what we can… what we can build, to, to, to, based on… on some of this, so…

120
00:19:16.560 --> 00:19:22.139
Oliver Davies (opdavies): Yeah, that's not sculptor-generated, as far as I know, but

121
00:19:22.280 --> 00:19:38.369
Oliver Davies (opdavies): We'll see. Maybe it is, I'll have to ask Dave. Cool. So, start with sculpting in it. So, first of all, we're gonna give it a title, so let's just stick with PHP Southwest, or let's type it out properly, for,

122
00:19:38.580 --> 00:19:40.179
Oliver Davies (opdavies): Faking PHP.

123
00:19:42.100 --> 00:19:56.390
Oliver Davies (opdavies): So then it says, will your site have blog posts? So it's not just for blogs, you can do it with, just static site, any sort of static, static content. Let's a no for now.

124
00:19:56.680 --> 00:20:05.329
Oliver Davies (opdavies): So it shows you what it's done. So this is the folder we're in. It's made two directories, so it's made app and source.

125
00:20:05.600 --> 00:20:07.570
Oliver Davies (opdavies): It's then said, success.

126
00:20:07.750 --> 00:20:15.439
Oliver Davies (opdavies): And there's the command that we want to run, to start it up. So, we'll do that in a second.

127
00:20:15.590 --> 00:20:25.530
Oliver Davies (opdavies): when GitHub comes back, I've got a PR that, is gonna add, run, scalping run, or scalper generating here, so that would be there as soon as…

128
00:20:25.670 --> 00:20:32.310
Oliver Davies (opdavies): GitHub comes back, because for some reason, I couldn't, push to it earlier on. So we'll,

129
00:20:32.510 --> 00:20:34.630
Oliver Davies (opdavies): Get onto that when we're ready.

130
00:20:36.390 --> 00:20:46.560
Oliver Davies (opdavies): This is now coming into I… let's see if I can remember how to tie it out of Linux. Let's ignore that, let's ignore that. Okay, cool. So this is what we're looking for. This is…

131
00:20:46.900 --> 00:20:54.229
Oliver Davies (opdavies): same directory we're just in, I've just hidden the sample data, hidden the Git directory, because we don't… don't care about those running.

132
00:20:54.480 --> 00:20:58.059
Oliver Davies (opdavies): So, Scalp and Generate has made this app folder.

133
00:20:58.760 --> 00:21:13.480
Oliver Davies (opdavies): And then made a config folder, and then let's give it 3, maybe. It's made some files inside here, got sculpting kernel and sculptinsite.yaml. It's made a placeholder, sculptingkernel.php.

134
00:21:13.630 --> 00:21:16.130
Oliver Davies (opdavies): Because I get ignored from earlier.

135
00:21:16.240 --> 00:21:27.470
Oliver Davies (opdavies): And then the magic stuff happens inside this source folder. So, when I go back to the slides, instead of where source files go in, that's where they'll go inside this source folder.

136
00:21:28.330 --> 00:21:31.870
Oliver Davies (opdavies): And what's made for us, if I just go ahead and open it…

137
00:21:32.360 --> 00:21:38.370
Oliver Davies (opdavies): is this index.md file, so it's a markdown file.

138
00:21:38.550 --> 00:21:48.539
Oliver Davies (opdavies): And in this section here, we specify, the layout to use, so this is called the Front Matter section. So this is YAML at the top of the page.

139
00:21:48.620 --> 00:22:02.579
Oliver Davies (opdavies): And then we've got Markdown here, and then this goes through a twig formatter as well. So, these double curly brackets here are a twig variable that we're gonna put here, so…

140
00:22:02.580 --> 00:22:09.560
Oliver Davies (opdavies): Site.title is going to look in site, sculptingSite.yaml.

141
00:22:09.700 --> 00:22:17.870
Oliver Davies (opdavies): And it's going to pull, this title from here, into this layout on the right-hand side, because…

142
00:22:18.010 --> 00:22:25.909
Oliver Davies (opdavies): Site-level variables are site.whatever, and then the key on the left-hand side matches the variable name over here.

143
00:22:26.850 --> 00:22:33.079
Oliver Davies (opdavies): So when we build it, I expect it to say, welcome to PHP Southwest, on the side there. So…

144
00:22:33.690 --> 00:22:37.889
Oliver Davies (opdavies): We've also got the views directory, so this is where our layouts live.

145
00:22:38.180 --> 00:22:52.369
Oliver Davies (opdavies): It also supports underscore layouts, so if you see that as well, it's the same thing. And again, this is just a twig file, so you've got double curly braces in here, so that's where the site title is going to go.

146
00:22:52.530 --> 00:22:59.489
Oliver Davies (opdavies): And we have some blocks, so this is where we're going to define where the content lives within the template.

147
00:23:00.080 --> 00:23:03.250
Oliver Davies (opdavies): So, again, people are familiar with Twig, they should look…

148
00:23:03.500 --> 00:23:08.300
Oliver Davies (opdavies): pretty familiar to, you know, a Symphony project or a Drupal project.

149
00:23:08.920 --> 00:23:15.760
Oliver Davies (opdavies): And then we've got some includes directory, where we can put some reusable components and snippets and things.

150
00:23:16.530 --> 00:23:21.010
Oliver Davies (opdavies): In there. So, let's go back into my terminal…

151
00:23:22.100 --> 00:23:27.959
Oliver Davies (opdavies): And then the command, if I go back up to the top level, you're going to run, is scoping…

152
00:23:28.740 --> 00:23:31.610
Oliver Davies (opdavies): So, after you do this first. Scope and generate.

153
00:23:32.240 --> 00:23:41.200
Oliver Davies (opdavies): So that's gonna take in, the files. I don't have it in this shell… For some reason…

154
00:23:41.340 --> 00:23:42.539
Oliver Davies (opdavies): Let's just ruin that.

155
00:23:44.200 --> 00:23:47.019
Oliver Davies (opdavies): Let's gonna open up a new tab. That's the problem.

156
00:23:49.150 --> 00:23:52.690
Oliver Davies (opdavies): And source.source doesn't exist, I'm in the wrong folder.

157
00:23:53.430 --> 00:24:01.139
Oliver Davies (opdavies): Okay, cool. So now I'm in the right folder. It's looked for the source directory, which I couldn't find, because I was inside the source directory.

158
00:24:02.010 --> 00:24:16.440
Oliver Davies (opdavies): But now it's found it, it's generated the site, it took 0.3… 0.03 seconds, and we have this new directory. Yeah, so we've got output dev directory. So we can just run tree on output dev.

159
00:24:16.820 --> 00:24:22.030
Oliver Davies (opdavies): And our index.md has now become index.html.

160
00:24:22.860 --> 00:24:25.120
Oliver Davies (opdavies): So, the file name…

161
00:24:25.420 --> 00:24:35.309
Oliver Davies (opdavies): But from here, it's just become, it's just replaced the extension. So, if this was, banana, it would become banana.html, or whatever you think is.

162
00:24:35.830 --> 00:24:37.970
Oliver Davies (opdavies): And these are generated.

163
00:24:38.090 --> 00:24:56.869
Oliver Davies (opdavies): HDR. So, it's taken that layout that we… that I showed it a minute ago, substituted the site title here, then substituted it within the H1 as well, and then built us a static page. So, I could take this whole directory.

164
00:24:57.440 --> 00:25:04.859
Oliver Davies (opdavies): put it on GitHub pages, put it on my web server, or whatever, and it would run. Easy peasy.

165
00:25:05.120 --> 00:25:08.319
Oliver Davies (opdavies): So… That's how we build the site.

166
00:25:08.730 --> 00:25:14.339
Oliver Davies (opdavies): Let's say you could also run dash dash server, dash dash watch.

167
00:25:15.250 --> 00:25:28.169
Oliver Davies (opdavies): I think that's probably the right combination of things. Let's go to localhost. So now it's running this site locally, for me, and you can see that it's running…

168
00:25:28.350 --> 00:25:32.999
Oliver Davies (opdavies): It's done the template substitution here, and everything.

169
00:25:33.380 --> 00:25:40.320
Oliver Davies (opdavies): So, the new one is… Sculpting… run.

170
00:25:40.830 --> 00:25:43.059
Oliver Davies (opdavies): So yeah, just learned about this as well.

171
00:25:43.220 --> 00:25:47.770
Oliver Davies (opdavies): So, that does the same thing. It does server generate…

172
00:25:47.950 --> 00:25:52.549
Oliver Davies (opdavies): and editor, I believe, I think I've got that right. And we can see that this new…

173
00:26:05.520 --> 00:26:06.640
Gabor Szabo: You got muted.

174
00:26:07.050 --> 00:26:08.390
Gabor Szabo: Oliver you are muted.

175
00:26:08.600 --> 00:26:14.289
Oliver Davies (opdavies): Sorry, okay, sorry, I was dragging the bar out of the way and must have clicked the button.

176
00:26:14.480 --> 00:26:28.160
Oliver Davies (opdavies): So, yeah, I was saying that, yeah, now you can click Edit, and this is a new thing in Sculpting 4, is this browser editor, so I can click edit the page, and we can edit it in here, save the changes.

177
00:26:28.530 --> 00:26:37.839
Oliver Davies (opdavies): And then it's updating that there, in real time, and also should be updating it in there as well, which I think is pretty cool.

178
00:26:39.770 --> 00:26:46.060
Oliver Davies (opdavies): Yeah, so now we can just keep making changes in here, so we can say hello, or whatever we want.

179
00:26:46.240 --> 00:26:53.240
Oliver Davies (opdavies): me here, and we can make lists, because this is an MD file, and go with a typical…

180
00:26:53.780 --> 00:26:56.509
Oliver Davies (opdavies): Fruit list, for whatever reason.

181
00:26:56.870 --> 00:26:58.700
Oliver Davies (opdavies): And you can see it's made.

182
00:26:58.990 --> 00:27:03.990
Oliver Davies (opdavies): an actual HTML list from… from that markdown, so…

183
00:27:04.150 --> 00:27:15.070
Oliver Davies (opdavies): Again, if you're just using GitHub pages, writing things in Markdown, you can do that, and then when it generates and builds the site, it will make proper HTML.

184
00:27:15.630 --> 00:27:21.100
Oliver Davies (opdavies): It's HTML, with Twig, with Markdown, which is a pretty… Powerful combination.

185
00:27:23.510 --> 00:27:31.960
Oliver Davies (opdavies): Cool. Any questions for anybody, or Kevin, anything I've missed? You want me to mention before we move on to the next… the next bit?

186
00:27:33.510 --> 00:27:36.489
Kevin Boyd: I can't think of anything in particular.

187
00:27:36.490 --> 00:27:37.670
Oliver Davies (opdavies): If not just carrying.

188
00:27:38.820 --> 00:27:39.390
Kevin Boyd: Yeah.

189
00:27:39.610 --> 00:27:42.520
Gabor Szabo: I assume there's no auto-load of the.

190
00:27:42.520 --> 00:27:43.540
Oliver Davies (opdavies): That's, that's a good point.

191
00:27:43.540 --> 00:27:44.240
Gabor Szabo: Nulu.

192
00:27:44.240 --> 00:27:57.600
Oliver Davies (opdavies): So, what we'll do, let's add another page, let's call it page 2. Let's just show, like, again, we've added a new, new Markdown file, page2.md. Try not to mute myself.

193
00:27:57.800 --> 00:28:09.420
Oliver Davies (opdavies): And you can see on this side, because we're running dash dash watch, it's noticed that there's a new file, or a file that's changed, and it's rerun. So hopefully, on this side now, we can go to page 2.

194
00:28:11.240 --> 00:28:16.710
Oliver Davies (opdavies): And this might change the content, it would be useful, so we could see the difference.

195
00:28:18.990 --> 00:28:22.539
Oliver Davies (opdavies): Okay, so there's page 2. So, we take out…

196
00:28:23.070 --> 00:28:25.469
Oliver Davies (opdavies): That, we go back to the homepage.

197
00:28:25.660 --> 00:28:28.500
Oliver Davies (opdavies): And then we go here, we're back on page 2.

198
00:28:28.770 --> 00:28:33.240
Oliver Davies (opdavies): So, what's done in the background is made…

199
00:28:33.570 --> 00:28:38.409
Oliver Davies (opdavies): a page 2 directory with an index or HTML file in it, so…

200
00:28:38.710 --> 00:28:44.729
Oliver Davies (opdavies): obviously have as many of these as you want, and I have a lot of them. On my side.

201
00:28:45.240 --> 00:28:49.060
Oliver Davies (opdavies): So… Next thing. Let's keep that running.

202
00:28:49.180 --> 00:28:50.620
Oliver Davies (opdavies): And let's…

203
00:28:51.130 --> 00:29:10.570
Oliver Davies (opdavies): do this. So, again, this is a… being a Drupal dev, one of the things I really liked was the idea of having, different content types. So the fact that we've got those in Sculping, yeah, it was really… so it made sense to me, because I'm used to Drupal my content in that way.

204
00:29:12.020 --> 00:29:18.919
Oliver Davies (opdavies): Let's say we run… That again, just because I'm in yet another shell.

205
00:29:19.740 --> 00:29:28.120
Oliver Davies (opdavies): So what we're going to use this time is this contentCreate command, and we're going to create some content types.

206
00:29:28.800 --> 00:29:30.680
Oliver Davies (opdavies): So… still paint…

207
00:29:31.310 --> 00:29:37.880
Oliver Davies (opdavies): So it doesn't create actual content, which I think, to me, was slightly confusing to begin with, until I realized what it did.

208
00:29:38.330 --> 00:29:40.930
Oliver Davies (opdavies): So maybe this should be content type create.

209
00:29:41.190 --> 00:29:43.470
Oliver Davies (opdavies): But there we are, so…

210
00:29:43.940 --> 00:29:47.199
Oliver Davies (opdavies): The first thing it tells me, there's not enough arguments, so…

211
00:29:47.250 --> 00:30:04.399
Oliver Davies (opdavies): What we're gonna build is, let's say, a clone of PHP Meetup Group site, and we'll have talks and speakers. So, rather than everything being just a page, we'll go ahead and we'll make content type for both of those things.

212
00:30:05.180 --> 00:30:12.310
Oliver Davies (opdavies): So, this command is helpful, it just tells you what things are that you need to add for it to work.

213
00:30:13.300 --> 00:30:23.119
Oliver Davies (opdavies): This indentation is a little bit broken, as well. I've got another PR open, when GitHub comes back for that, because I always go… I notice that when going through this.

214
00:30:23.870 --> 00:30:26.679
Oliver Davies (opdavies): For rehearsing for this earlier on.

215
00:30:26.980 --> 00:30:33.450
Oliver Davies (opdavies): And this goes in the, app config sculpting kernel file.

216
00:30:34.010 --> 00:30:43.499
Oliver Davies (opdavies): So we have the duplication there, don't need that, and it's… because YAML… let's make sure everything is…

217
00:30:44.040 --> 00:30:46.409
Oliver Davies (opdavies): Sorted out properly, so…

218
00:30:47.610 --> 00:30:53.729
Oliver Davies (opdavies): Posts are still, here, we just, disabled it, otherwise it would be complaining that there's…

219
00:30:53.870 --> 00:30:55.699
Oliver Davies (opdavies): They're on the posts.

220
00:30:56.180 --> 00:31:05.370
Oliver Davies (opdavies): But we can add, a new one called speakers. Most of this, I think, is just boilerplate.

221
00:31:05.760 --> 00:31:07.939
Oliver Davies (opdavies): So it's sort of implied.

222
00:31:08.350 --> 00:31:10.750
Oliver Davies (opdavies): Type is path by default.

223
00:31:11.220 --> 00:31:18.570
Oliver Davies (opdavies): path, it always becomes underscore something, by default. Singular name just takes the S off the end.

224
00:31:18.990 --> 00:31:22.250
Oliver Davies (opdavies): Layout will keep…

225
00:31:23.000 --> 00:31:30.100
Oliver Davies (opdavies): And then… I think enable just choose default. Let's see if, we can get by with just… just that.

226
00:31:32.110 --> 00:31:37.970
Oliver Davies (opdavies): Let's go back, so I'm not running this double back, so… That's…

227
00:31:38.100 --> 00:31:42.910
Oliver Davies (opdavies): Start running it. Okay, so it's registered it, and said, we didn't find…

228
00:31:43.060 --> 00:31:57.360
Oliver Davies (opdavies): at least one of this type. We've declared a content type, but obviously we haven't… haven't made any of them yet, and it keeps reminding me that that's a thing, until I go ahead and do it. So, while I'm here…

229
00:31:57.880 --> 00:32:02.330
Oliver Davies (opdavies): Let's do, let's change speaker to talk.

230
00:32:02.640 --> 00:32:04.699
Oliver Davies (opdavies): As well, we'll do that in both places.

231
00:32:05.400 --> 00:32:08.380
Oliver Davies (opdavies): So now we've got 2 new custom content types.

232
00:32:08.760 --> 00:32:11.779
Oliver Davies (opdavies): And so hopefully it's probably gonna complain now that I've got…

233
00:32:12.380 --> 00:32:15.279
Oliver Davies (opdavies): Two content types that don't exist.

234
00:32:15.730 --> 00:32:18.359
Oliver Davies (opdavies): Or, maybe it's just got a paper about one.

235
00:32:18.950 --> 00:32:28.579
Oliver Davies (opdavies): Anyway, so, what we're gonna have, once I start adding these in, is a page for each speaker, and a page for each talk.

236
00:32:28.790 --> 00:32:41.250
Oliver Davies (opdavies): And the permalink, or the URL is going to be the site name slash speakers slash the title, and then, for talk, it's gonna be talk slash talk slash title. Don't acceptable.

237
00:32:42.540 --> 00:32:51.419
Oliver Davies (opdavies): So, this is where my sample content comes in. I'm just gonna copy this from here, and I'm just gonna paste it into my folder.

238
00:32:52.100 --> 00:32:57.509
Oliver Davies (opdavies): Let me just do that. So, rather than watching me type a load of Markdown out.

239
00:32:57.720 --> 00:33:07.700
Oliver Davies (opdavies): The name, so here, the speaker's content type is underscore speakers, and then underscore talks is for talks.

240
00:33:07.990 --> 00:33:14.429
Oliver Davies (opdavies): So that's all based on the content type name that we gave it, and then we'll just leave it as the default.

241
00:33:16.100 --> 00:33:19.699
Oliver Davies (opdavies): So, it's no longer complaining that

242
00:33:19.880 --> 00:33:23.199
Oliver Davies (opdavies): There are no speakers or talks, because there are some now.

243
00:33:23.310 --> 00:33:33.559
Oliver Davies (opdavies): But what it's complaining about now is the template called Speaker is not defined. So, each content type gets its own template type.

244
00:33:33.660 --> 00:33:39.330
Oliver Davies (opdavies): So it's looking for a speaker.twig, or a speaker.html.twig.

245
00:33:39.500 --> 00:33:43.529
Oliver Davies (opdavies): So, here I will copy that across as well.

246
00:33:44.440 --> 00:33:51.300
Oliver Davies (opdavies): Just yank those, put those in views, and we'll put it next to the default template.

247
00:33:53.330 --> 00:34:00.139
Oliver Davies (opdavies): So, sometimes it gets a bit stuck. Sometimes we need to restart it for it to pick up the new things.

248
00:34:00.430 --> 00:34:04.620
Oliver Davies (opdavies): But now it's not complaining about any missing… Template files.

249
00:34:05.000 --> 00:34:09.840
Oliver Davies (opdavies): All these ones do is they extend the default template.

250
00:34:10.060 --> 00:34:19.919
Oliver Davies (opdavies): So they're not doing… they're just… it's Twig, so we've got inheritance, and we can extend any other template that we want to extend, in case default.

251
00:34:20.360 --> 00:34:21.139
Oliver Davies (opdavies): So…

252
00:34:21.340 --> 00:34:26.660
Oliver Davies (opdavies): Of course, to speakers, we want to show different stuff, so that's where you would… you would do that.

253
00:34:26.969 --> 00:34:27.770
Oliver Davies (opdavies): What's up?

254
00:34:28.600 --> 00:34:31.170
Oliver Davies (opdavies): Let's go back to this and see where we were.

255
00:34:31.969 --> 00:34:44.300
Oliver Davies (opdavies): So, back to our index. Actually, I haven't actually want to see these. So, speakers, each speaker is its own markdown file, and these are a selection of the speakers that we had at the user group.

256
00:34:44.409 --> 00:34:52.369
Oliver Davies (opdavies): Over the years, and obviously this isn't… isn't the whole list, but I've just picked a couple, at random.

257
00:34:52.590 --> 00:34:56.350
Oliver Davies (opdavies): And what I've done is, in the front matter section.

258
00:34:56.659 --> 00:35:11.570
Oliver Davies (opdavies): The name of the person is the title of the page, and then the image is just their name, with the image extension at the end. This could be name, and this could be anything, like photo, or avatar.

259
00:35:11.820 --> 00:35:17.289
Oliver Davies (opdavies): If I type it… It doesn't matter what they're called from a…

260
00:35:17.880 --> 00:35:23.940
Oliver Davies (opdavies): front matter perspective, as long as they match in the template.

261
00:35:24.810 --> 00:35:27.710
Oliver Davies (opdavies): So, I'll keep them as that for now.

262
00:35:27.890 --> 00:35:32.679
Oliver Davies (opdavies): And then… Everyone's got a title, everyone's got an image.

263
00:35:32.870 --> 00:35:39.389
Oliver Davies (opdavies): Some of it's got links, so a link to websites or, whatever. Again, I'm just…

264
00:35:40.090 --> 00:35:46.190
Oliver Davies (opdavies): Go back on PHP Southwest… Let's find… Speakers.

265
00:35:47.210 --> 00:35:49.440
Oliver Davies (opdavies): Let's look at Davies, maybe.

266
00:35:49.740 --> 00:36:03.780
Oliver Davies (opdavies): Dave doesn't know I'm doing this, by the way. He's kind of a bit of a bit of a shock. So yeah, Dave is the current, sort of, organiser. So you can see here, he's got, his name, and then, his little, sort of, subtitle slash description.

267
00:36:03.820 --> 00:36:14.849
Oliver Davies (opdavies): That's what I've put as the body area, and then this little elsewhere section is links, so I've put those under links here. This is just a YAML list of links.

268
00:36:16.010 --> 00:36:20.399
Oliver Davies (opdavies): Some have them, some don't have them, so it… it depends.

269
00:36:20.580 --> 00:36:31.540
Oliver Davies (opdavies): Some… some of them are really basic, and then some of them are a bit more… mine, again, mine, I've put my image and my links and everything in there, so…

270
00:36:32.920 --> 00:36:42.870
Oliver Davies (opdavies): This is similar to, how things like content types and fields work in Drupal, or, WordPress is, like, advanced custom fields or something.

271
00:36:42.980 --> 00:36:53.290
Oliver Davies (opdavies): So I can sort of see in my mind how this would sort of translate to a Drupal form, and be like, you know, now I need to add an image field called image, and a field called links.

272
00:36:54.830 --> 00:36:59.920
Oliver Davies (opdavies): And then on the other side, we've got Torx. So, Torx is,

273
00:37:00.050 --> 00:37:04.809
Oliver Davies (opdavies): Another type, what the title, the date of the talk.

274
00:37:04.940 --> 00:37:11.050
Oliver Davies (opdavies): And then the speakers. So, this name matches the speaker name. So…

275
00:37:11.300 --> 00:37:18.689
Oliver Davies (opdavies): sort of mine, sort of Rob's. So, yeah, you get the idea, they're all sort of the, sort of the same. So…

276
00:37:19.680 --> 00:37:26.919
Oliver Davies (opdavies): If we go back to our example… Let's say we wanted to… Have our homepage?

277
00:37:27.300 --> 00:37:32.059
Oliver Davies (opdavies): Let's delete this, let's keep the hello, why not? The H1.

278
00:37:33.940 --> 00:37:38.200
Oliver Davies (opdavies): And then… start from scratch. So, let's say…

279
00:37:38.320 --> 00:37:43.690
Oliver Davies (opdavies): The H1 is already there, so let's say our H2 is going to be speakers.

280
00:37:44.290 --> 00:37:47.100
Oliver Davies (opdavies): And, you know, for now, this is just…

281
00:37:47.690 --> 00:37:51.450
Oliver Davies (opdavies): Even though it's a markdown file, it could be a twig file.

282
00:37:51.610 --> 00:37:58.150
Oliver Davies (opdavies): But, HTML still works, and Twig still works. So we can write Twig stuff here.

283
00:37:58.920 --> 00:38:03.380
Oliver Davies (opdavies): So, speakers. So we can say, TWIG,

284
00:38:03.510 --> 00:38:13.150
Oliver Davies (opdavies): the do-something syntax, is what I was referring to as, could embrace, percentage. So we can say 4, speaker in…

285
00:38:13.340 --> 00:38:18.010
Oliver Davies (opdavies): Data dots, because… I wonder when we can end.

286
00:38:19.360 --> 00:38:30.940
Oliver Davies (opdavies): So, remember I said earlier on that, site variables were site.something? The other one we could have is, like, page-level ones. We could say,

287
00:38:32.260 --> 00:38:35.740
Oliver Davies (opdavies): Start year is, like, I don't know, 2020.

288
00:38:36.020 --> 00:38:40.250
Oliver Davies (opdavies): And so that would be a page variable, so I could say page.startyo.

289
00:38:41.050 --> 00:38:41.989
Oliver Davies (opdavies): If I could talk.

290
00:38:45.880 --> 00:38:47.840
Oliver Davies (opdavies): Stop.

291
00:38:48.530 --> 00:38:49.610
Oliver Davies (opdavies): That should be there.

292
00:38:50.160 --> 00:38:52.649
Oliver Davies (opdavies): And there we go, it takes a few seconds.

293
00:38:53.020 --> 00:39:06.880
Oliver Davies (opdavies): So, that's the three, sort of, levels we've got. We've got site-level variables that come from the scoping site file, page variables that come from the front matter area, and then data speakers is from other

294
00:39:07.190 --> 00:39:08.310
Oliver Davies (opdavies): content types.

295
00:39:08.430 --> 00:39:13.670
Oliver Davies (opdavies): So, it's not working yet, there's nothing there. We'll get to that in a second, but…

296
00:39:13.890 --> 00:39:27.500
Oliver Davies (opdavies): Well, whilst we've got the date there, on here somewhere, it says, yeah, 15 years it's been going, it's been going. So, let's say, it started in, I don't know, what was it, 2010, maybe?

297
00:39:27.600 --> 00:39:42.190
Oliver Davies (opdavies): So, the start year for this is going to be… not 2010, but 2010. Like, we don't want to have to update that manually every time, so we can use some twig magic in here, and we can say,

298
00:39:42.480 --> 00:39:50.210
Oliver Davies (opdavies): Sorry, let me get it wrong now, right? Oh, no, sorry, it's, today.

299
00:39:50.560 --> 00:39:53.969
Oliver Davies (opdavies): We want to make this dynamic, right? We want the site to figure out what…

300
00:39:54.280 --> 00:39:57.009
Oliver Davies (opdavies): What it is. So we can say…

301
00:39:57.350 --> 00:40:01.189
Oliver Davies (opdavies): Hopefully I've got that right. So we can say today is a string.

302
00:40:01.360 --> 00:40:14.709
Oliver Davies (opdavies): pass it through a filter called date, and then there we say we want the format of the date to be Y, so the current year, and then from that we want to subtract the start year, which we know is 2010, so…

303
00:40:14.880 --> 00:40:20.210
Oliver Davies (opdavies): 16. Okay, my maths is not good. So let's say it started in…

304
00:40:20.490 --> 00:40:26.690
Oliver Davies (opdavies): 29th, so it started in… Let me put a full stop again.

305
00:40:27.680 --> 00:40:31.979
Oliver Davies (opdavies): Oh, I've done the wrong ground. Live maths.

306
00:40:37.170 --> 00:40:38.980
Oliver Davies (opdavies): Yeah, there we are started.

307
00:40:43.200 --> 00:40:43.940
Oliver Davies (opdavies): Yes.

308
00:40:46.220 --> 00:41:00.970
Oliver Davies (opdavies): So, even though we're talking about static sites, as in they're just HTML and CSS, etc, they can be quite dynamic, and they can have variables and things in them that say, like, every time you build.

309
00:41:01.070 --> 00:41:10.930
Oliver Davies (opdavies): yeah, it's gonna calculate whatever the current day is, and… or the year is, and do something with that data. So, think of,

310
00:41:10.980 --> 00:41:23.769
Oliver Davies (opdavies): copyright years, for example. You don't have to go in and change that manually all the time, so we can make that dynamic build time, and then export it out to a static site at the end.

311
00:41:24.760 --> 00:41:27.559
Gabor Szabo: To try to ask a question again. Olivier, can you hear me?

312
00:41:27.900 --> 00:41:28.909
Oliver Davies (opdavies): I can hear you, yes.

313
00:41:28.910 --> 00:41:35.609
Gabor Szabo: Oh, excellent. I thought you can't hear us. So… so this trig… this is Strig, the… the computation…

314
00:41:35.610 --> 00:41:45.589
Oliver Davies (opdavies): This is, this is the quarter twig, right? Yep, this is Twig, PHP. So it's, it's templated language used by Symphony.

315
00:41:45.870 --> 00:42:00.139
Oliver Davies (opdavies): And then also used by other projects, like Drupal. So instead of, say, PHP tags, like I would have done in Drupal 7, we used double curly brace to echo or print something.

316
00:42:00.150 --> 00:42:09.540
Oliver Davies (opdavies): And we can use this, can't this, do something syntax with curly percent to do things like if statements, for loops, etc.

317
00:42:09.810 --> 00:42:11.929
Oliver Davies (opdavies): Yeah, so it's…

318
00:42:12.180 --> 00:42:18.900
Oliver Davies (opdavies): again, if you're used to using Symphony, or Drupal, or anything else that uses Twig, then, yeah, this one.

319
00:42:18.900 --> 00:42:28.189
Gabor Szabo: They're similar to Ginger in Python. I didn't know that in Ginger you can do these computations, but I'll look it up.

320
00:42:29.390 --> 00:42:36.620
Oliver Davies (opdavies): yeah, again, it's very similar to, sort of, JSX, and, like, Rust uses double curlies as well, so…

321
00:42:36.820 --> 00:42:38.420
Oliver Davies (opdavies): She's pretty familiar,

322
00:42:38.420 --> 00:42:41.280
Gabor Szabo: Yeah, yeah, just I didn't know about the computations.

323
00:42:41.620 --> 00:42:43.550
Oliver Davies (opdavies): Cool.

324
00:42:43.670 --> 00:42:57.689
Oliver Davies (opdavies): So, let's go back to this. So, let's say we want a list of our speakers. So, for speaker in, date speakers, that is currently empty. We haven't actually added anything yet. Speaker…

325
00:42:58.470 --> 00:42:59.500
Oliver Davies (opdavies): That's it.

326
00:43:00.390 --> 00:43:01.630
Oliver Davies (opdavies): And…

327
00:43:02.970 --> 00:43:22.120
Oliver Davies (opdavies): We'll do that as a second. This is why I've kept it as title rather than name. I want to have some consistency, when I'm doing the template inside. But it's still empty, right? There's no… there's no speakers coming through yet, and it's just a thing, usually say use, speakers.

328
00:43:23.170 --> 00:43:24.610
Oliver Davies (opdavies): To bring it into scope.

329
00:43:25.280 --> 00:43:31.750
Oliver Davies (opdavies): So that makes all the speakers available to this page. You don't get them by default.

330
00:43:32.340 --> 00:43:35.000
Oliver Davies (opdavies): So, here are all of our names.

331
00:43:35.200 --> 00:43:38.660
Oliver Davies (opdavies): And let's say we want to wrap this in a list.

332
00:43:43.190 --> 00:43:44.539
Oliver Davies (opdavies): Close the list.

333
00:43:45.740 --> 00:43:50.449
Oliver Davies (opdavies): Watching HP dev type HTML. It's always fun.

334
00:43:51.130 --> 00:43:55.150
Oliver Davies (opdavies): And make that an ally, close the ally, and that will do that.

335
00:43:55.280 --> 00:43:57.120
Oliver Davies (opdavies): But, hopefully we can have a list.

336
00:43:57.540 --> 00:43:58.950
Oliver Davies (opdavies): Thanks. Cool.

337
00:43:59.100 --> 00:44:05.309
Oliver Davies (opdavies): Right, there's all of our speakers, and then… let's do the same, for the talks.

338
00:44:05.820 --> 00:44:07.310
Oliver Davies (opdavies): Let's make it this a choice.

339
00:44:09.390 --> 00:44:20.279
Oliver Davies (opdavies): I mean, I'm not gonna rebuild the whole website, we could, but, Gabor also, he's got work tomorrow, so we could, we could. So let's just replace speaker with talk…

340
00:44:20.450 --> 00:44:21.490
Oliver Davies (opdavies): Let's do that.

341
00:44:21.640 --> 00:44:22.880
Oliver Davies (opdavies): And then…

342
00:44:23.950 --> 00:44:33.859
Oliver Davies (opdavies): Should be the same, right? It's the same for loop, I've just replaced, talk in data.talks with the talk title rather than the speaker title.

343
00:44:34.020 --> 00:44:35.959
Oliver Davies (opdavies): What's sort of what I've done now?

344
00:44:36.740 --> 00:44:38.949
Oliver Davies (opdavies): So, there's our talk.

345
00:44:39.170 --> 00:44:44.079
Oliver Davies (opdavies): heading. Again, we need to just bring this into scope here.

346
00:44:44.860 --> 00:44:49.989
Oliver Davies (opdavies): To make those all come through… Jesus.

347
00:44:50.750 --> 00:45:00.149
Oliver Davies (opdavies): Speakers Talks… Full talk and data talks, talk title…

348
00:45:00.150 --> 00:45:11.270
Gabor Szabo: Can you do, verification, data verification? For example, you have presentations, and each presentation has a speaker, and whether you have that,

349
00:45:11.530 --> 00:45:13.889
Gabor Szabo: Written correctly in the data.

350
00:45:14.470 --> 00:45:24.349
Oliver Davies (opdavies): Yep, we'll… yeah, it's sort of a database, really, in the background. So yeah, we'll look at that, if I could figure out why these talks are running.

351
00:45:25.180 --> 00:45:28.530
Oliver Davies (opdavies): It's not until I got the right variable name.

352
00:45:29.960 --> 00:45:33.550
Oliver Davies (opdavies): speakers… that talks BDD.

353
00:45:34.510 --> 00:45:39.470
Oliver Davies (opdavies): Title… That title should match that title.

354
00:45:43.700 --> 00:45:47.370
Oliver Davies (opdavies): Must be missing something. We are using it, so it should be there.

355
00:45:50.530 --> 00:45:53.450
Oliver Davies (opdavies): Talks… Title.

356
00:45:53.640 --> 00:45:55.700
Oliver Davies (opdavies): Yep, why are these not coming through?

357
00:45:56.410 --> 00:45:57.580
Kevin Boyd: A regenerator?

358
00:45:59.280 --> 00:46:00.210
Oliver Davies (opdavies): So I can?

359
00:46:00.480 --> 00:46:01.710
Kevin Boyd: Did you restart the generator?

360
00:46:02.150 --> 00:46:05.780
Oliver Davies (opdavies): I did not, but let's try again.

361
00:46:09.920 --> 00:46:15.600
Oliver Davies (opdavies): Interestingly, it did tell me earlier on there wasn't any talks when it was building either.

362
00:46:17.790 --> 00:46:19.899
Oliver Davies (opdavies): Oh, there we go. It was a…

363
00:46:20.240 --> 00:46:23.350
Oliver Davies (opdavies): Switch it off and on again situation.

364
00:46:23.550 --> 00:46:28.020
Oliver Davies (opdavies): So… There's all of our speakers, there's all the talks.

365
00:46:28.330 --> 00:46:33.950
Oliver Davies (opdavies): So yeah, let's do something like the board was just saying, let's do something… something cool.

366
00:46:34.230 --> 00:46:39.400
Oliver Davies (opdavies): So, let's… Delete this anymore, go away.

367
00:46:39.570 --> 00:46:44.830
Oliver Davies (opdavies): Right, so each one of these has got a link, so we'll make this into a link.

368
00:46:46.040 --> 00:46:48.830
Oliver Davies (opdavies): So just our normal AHRF.

369
00:46:49.090 --> 00:46:52.800
Oliver Davies (opdavies): Style links, and we will…

370
00:46:56.170 --> 00:46:58.619
Oliver Davies (opdavies): DT4 slot. What's going on?

371
00:46:58.940 --> 00:47:00.680
Oliver Davies (opdavies): Oh, I see. Boyd.

372
00:47:01.060 --> 00:47:10.839
Oliver Davies (opdavies): My brain goes faster than my… than my fingers sometimes. Cool. Okay, so to put in the link, we can say speaker.url.

373
00:47:11.560 --> 00:47:26.880
Oliver Davies (opdavies): So that will get the URL to each speaker's page. If you can see at the bottom left, we've got speakers slash Dan Leitch, and there's Dan Leitch's, subtitle, there's Kieran's subtitle.

374
00:47:26.960 --> 00:47:31.579
Oliver Davies (opdavies): So it's made all of these pages, just from those Markdown files.

375
00:47:31.720 --> 00:47:33.560
Oliver Davies (opdavies): Which is cool.

376
00:47:34.680 --> 00:47:43.320
Oliver Davies (opdavies): These are all in the right order, I think, alphabetically, just because of the way it's loaded them. These are not, so…

377
00:47:43.500 --> 00:47:46.239
Oliver Davies (opdavies): What I'd like to do is do the same thing.

378
00:47:46.720 --> 00:47:50.280
Oliver Davies (opdavies): Here, and let's make these all links as well.

379
00:47:50.570 --> 00:47:56.510
Oliver Davies (opdavies): First… And of course, not the speaker, it seems to be talk.

380
00:47:57.570 --> 00:48:00.429
Oliver Davies (opdavies): And this should hopefully be a list of links now as well, so…

381
00:48:00.650 --> 00:48:04.300
Oliver Davies (opdavies): These are all talks slash something. So…

382
00:48:04.470 --> 00:48:20.819
Oliver Davies (opdavies): instead of going into a UI and, editing, Drupal Admin or something, we can do this all through these Markdown files, or we can use this edit the page thing, which is cool. So yeah, it does update all that as well.

383
00:48:21.080 --> 00:48:22.630
Oliver Davies (opdavies): Which is… awesome.

384
00:48:24.090 --> 00:48:25.240
Oliver Davies (opdavies): So…

385
00:48:25.440 --> 00:48:40.319
Oliver Davies (opdavies): A bit more twig. These are all in a slightly wrong order. So we've got Nix for developers, FPGA, blah, blah, right? So let's say we want them sorted by, the title. We can use twig again, just like sort.

386
00:48:40.560 --> 00:48:45.200
Oliver Davies (opdavies): Then we get A, A, B, part of the function.

387
00:48:45.950 --> 00:48:48.580
Oliver Davies (opdavies): And a dot title…

388
00:48:50.340 --> 00:48:54.359
Oliver Davies (opdavies): I think it's where we can do Spaceship Operator, this is a bit title, I think.

389
00:48:55.530 --> 00:48:57.289
Oliver Davies (opdavies): I guess it's probably a better way of doing that.

390
00:49:00.080 --> 00:49:03.560
Oliver Davies (opdavies): And take those… that bracket off, that probably helps.

391
00:49:03.710 --> 00:49:10.340
Oliver Davies (opdavies): Okay, so now it's sorted all the talks alphabetically, so I didn't… I don't need to do that manually anymore, which is awesome.

392
00:49:12.210 --> 00:49:21.710
Oliver Davies (opdavies): let's say our speakers, we want to pull through those subtitles, so we can say, speaker… Drop blocks.

393
00:49:22.290 --> 00:49:27.950
Oliver Davies (opdavies): dot content… That's the name of the block where that content lives.

394
00:49:28.300 --> 00:49:31.050
Oliver Davies (opdavies): And it's pulling through now all of the…

395
00:49:31.180 --> 00:49:34.940
Oliver Davies (opdavies): Subtitles, or the body content before them.

396
00:49:35.050 --> 00:49:41.089
Oliver Davies (opdavies): We don't want the HTML, so we can play rule for this, so it gets rid of…

397
00:49:41.820 --> 00:49:44.910
Oliver Davies (opdavies): the, the HT… The,

398
00:49:45.960 --> 00:49:51.309
Oliver Davies (opdavies): I think… oh, mine's a bit more, I think we can create strip tags as well, if you don't want the…

399
00:49:51.800 --> 00:49:54.040
Oliver Davies (opdavies): The things.

400
00:49:54.260 --> 00:49:57.329
Oliver Davies (opdavies): Yeah, so it's got rid of HTML completely now.

401
00:49:57.850 --> 00:50:01.059
Oliver Davies (opdavies): So this is what we've got right now.

402
00:50:01.430 --> 00:50:04.669
Oliver Davies (opdavies): Kieran slash developer, so let's put a…

403
00:50:05.670 --> 00:50:08.659
Oliver Davies (opdavies): Little separator there, just so we can see.

404
00:50:09.460 --> 00:50:12.700
Oliver Davies (opdavies): Actually, you know, let's move… let's move that out with the link.

405
00:50:13.560 --> 00:50:15.759
Oliver Davies (opdavies): There we go, just to make it a little bit clearer.

406
00:50:16.550 --> 00:50:17.600
Oliver Davies (opdavies): Here we are.

407
00:50:19.080 --> 00:50:24.199
Oliver Davies (opdavies): This isn't a CSS presentation, by the way. We're just doing a… Yeah.

408
00:50:24.350 --> 00:50:28.510
Oliver Davies (opdavies): This is not really… I've got a different talk talking about CSS until the entire…

409
00:50:28.890 --> 00:50:32.259
Oliver Davies (opdavies): Okay, you can watch that one, separately if you'd like.

410
00:50:33.640 --> 00:50:49.660
Oliver Davies (opdavies): So, that's pulling through all the subtitles for the people. Some of them don't have them, so, like, Mike and Naomi don't have them. So, in this case, we don't want this dash there, so we can use if.

411
00:50:50.020 --> 00:50:53.920
Oliver Davies (opdavies): I'm gonna say, speaker… blocks.

412
00:50:55.190 --> 00:50:57.809
Oliver Davies (opdavies): Content, and then we can end.

413
00:50:58.380 --> 00:51:03.289
Oliver Davies (opdavies): But if, yeah, so… Again, we can do some quite dynamic stuff.

414
00:51:03.750 --> 00:51:05.760
Oliver Davies (opdavies): With this, so,

415
00:51:06.050 --> 00:51:16.509
Oliver Davies (opdavies): But again, it all just generates plain HTML at the end of the day, right? This is all done locally at build time, so it's pre-rendering everything.

416
00:51:16.510 --> 00:51:18.210
Gabor Szabo: to the database again?

417
00:51:18.890 --> 00:51:24.639
Oliver Davies (opdavies): Sure. So I will split this, and…

418
00:51:25.250 --> 00:51:26.720
Oliver Davies (opdavies): Which one did you want to see?

419
00:51:27.720 --> 00:51:30.110
Oliver Davies (opdavies): It's like, I'll open this here, and I'll open…

420
00:51:31.230 --> 00:51:34.774
Oliver Davies (opdavies): Not done that, right? So you can see…

421
00:51:35.590 --> 00:51:40.680
Oliver Davies (opdavies): So yeah, talk, talk at the top, and then speaker underneath, so we can see how those two

422
00:51:40.860 --> 00:51:43.359
Oliver Davies (opdavies): others too much up, and I'll just work in the middle.

423
00:51:43.360 --> 00:51:44.060
Gabor Szabo: Okay.

424
00:51:44.630 --> 00:51:49.190
Gabor Szabo: So the talk have, the, the list of the speakers, yeah.

425
00:51:49.600 --> 00:51:52.059
Oliver Davies (opdavies): Yep. Yeah, exactly. So…

426
00:51:52.430 --> 00:52:00.750
Oliver Davies (opdavies): We've also got a date, so let's put the date in there as well. So, after the talk title, let's do the same thing.

427
00:52:00.920 --> 00:52:04.320
Oliver Davies (opdavies): That we did here. So we can do talk.date.

428
00:52:04.540 --> 00:52:08.779
Gabor Szabo: So what happens if the speaker file doesn't exist?

429
00:52:09.750 --> 00:52:12.309
Oliver Davies (opdavies): If the speaker file doesn't exist…

430
00:52:12.310 --> 00:52:15.760
Gabor Szabo: Change the name of one of the speakers in one of the talks.

431
00:52:15.910 --> 00:52:18.790
Gabor Szabo: What happens? Just remove the letter, whatever.

432
00:52:19.530 --> 00:52:20.330
Oliver Davies (opdavies): Yep.

433
00:52:21.450 --> 00:52:29.860
Oliver Davies (opdavies): Nothing right now, because we're not doing anything with that. But we will in a minute. We will,

434
00:52:29.860 --> 00:52:31.310
Gabor Szabo: I'm really tense, Les.

435
00:52:34.310 --> 00:52:40.250
Oliver Davies (opdavies): Cool. Okay, so it's pulled through the talks and the dates, so it's translated this date.

436
00:52:40.360 --> 00:52:51.050
Oliver Davies (opdavies): Here, but it's pulled us through as a timestamp, which is not really what we want. So we can use data filter again, and say JSFY…

437
00:52:52.450 --> 00:52:58.749
Oliver Davies (opdavies): To give it a format, and it displays it in a nice, date. Date format.

438
00:52:58.920 --> 00:53:01.780
Oliver Davies (opdavies): And I want to get rid of this space.

439
00:53:02.310 --> 00:53:10.250
Oliver Davies (opdavies): triggering my OCD a little bit, having that space after… the thing. Right, cool.

440
00:53:10.700 --> 00:53:17.700
Oliver Davies (opdavies): So, they're in the right order now, because they're alphabetical, but given as we've got the dates here now, we really want them…

441
00:53:17.850 --> 00:53:23.269
Oliver Davies (opdavies): sorted, so, like, the most recent ones at the top. So, we can update our…

442
00:53:23.380 --> 00:53:27.230
Oliver Davies (opdavies): Thing here to say, date.

443
00:53:27.510 --> 00:53:29.320
Oliver Davies (opdavies): It's currently around this date.

444
00:53:30.240 --> 00:53:31.899
Oliver Davies (opdavies): So that's gonna sort them…

445
00:53:32.240 --> 00:53:40.490
Oliver Davies (opdavies): Oldest first, so 2022, 2023, and then 2025 and 26 are sort of hidden.

446
00:53:40.600 --> 00:53:48.710
Oliver Davies (opdavies): Down here somewhere. Let's zoom out, there we go. So there's two ways we could do this. We could use, a reverse…

447
00:53:49.140 --> 00:53:50.630
Oliver Davies (opdavies): Filter.

448
00:53:50.920 --> 00:53:55.639
Oliver Davies (opdavies): That's gonna reverse the order, so the newest ones show up at the top.

449
00:53:55.950 --> 00:54:00.370
Oliver Davies (opdavies): Or we could just swap this condition to say, B date, A date.

450
00:54:00.950 --> 00:54:04.630
Oliver Davies (opdavies): So it's just… Sort of in the other… the other order.

451
00:54:07.150 --> 00:54:10.220
Oliver Davies (opdavies): Cool. Next thing.

452
00:54:10.800 --> 00:54:15.620
Oliver Davies (opdavies): Let's do… on this side, we've got… we don't need these tabs anymore.

453
00:54:15.880 --> 00:54:29.300
Oliver Davies (opdavies): spoke at PHP Southwest X number of times, so we can… we can do that. So, this is where the name stuff is gonna come in… into it. One thing we haven't done, actually, before we get there…

454
00:54:29.630 --> 00:54:31.759
Oliver Davies (opdavies): He's on the homepage.

455
00:54:32.660 --> 00:54:33.900
Oliver Davies (opdavies): We have a…

456
00:54:34.170 --> 00:54:47.889
Oliver Davies (opdavies): by the numbers, so we've got 15 years. We've also got, talks and speakers, so let's… let's do that quickly. So, we've still got our data… data dot thing, so we can say speakers…

457
00:54:48.180 --> 00:55:00.540
Oliver Davies (opdavies): And that will load all the speakers up, and probably crash my server. So, the little local web server. So, if we just want the count value, it's the length filter.

458
00:55:01.110 --> 00:55:05.420
Oliver Davies (opdavies): So that'll just count the number, and then talks.

459
00:55:09.710 --> 00:55:14.270
Oliver Davies (opdavies): So this should give us… Yeah, 10 speakers.

460
00:55:14.530 --> 00:55:17.109
Oliver Davies (opdavies): Who've given 13 different talks.

461
00:55:17.320 --> 00:55:21.989
Oliver Davies (opdavies): So, let's say I update this, the same if we've had another event.

462
00:55:22.220 --> 00:55:28.300
Oliver Davies (opdavies): Added a new Markdown file, rebuild the site, and this number is going to change automatically.

463
00:55:28.410 --> 00:55:32.699
Oliver Davies (opdavies): I don't need to update that number anywhere, which is pretty cool.

464
00:55:33.230 --> 00:55:37.360
Oliver Davies (opdavies): Again, dynamic static sites, or static dynamic sites.

465
00:55:38.310 --> 00:55:40.919
Oliver Davies (opdavies): So, let's…

466
00:55:41.250 --> 00:55:52.770
Oliver Davies (opdavies): do the bit where we've got the number of times each person has spoken at an event. So, Dave has spoken 18 times. There is a list, let me find it.

467
00:55:52.970 --> 00:55:57.480
Oliver Davies (opdavies): It's a stats list. There it is.

468
00:55:58.240 --> 00:56:03.320
Oliver Davies (opdavies): Because he was given… The most talks. Dave's given the most, says 17 there.

469
00:56:03.710 --> 00:56:13.320
Oliver Davies (opdavies): And then Rob and I are both on 12, which I think is pretty cool. So we know how many times each speaker is given… has given talks, so we can do that as well.

470
00:56:13.620 --> 00:56:19.079
Oliver Davies (opdavies): So, let's take… The number of talks…

471
00:56:20.290 --> 00:56:22.599
Oliver Davies (opdavies): Okay, because this is marked down, it's…

472
00:56:23.740 --> 00:56:27.059
Oliver Davies (opdavies): Formatted it for me, very helpfully.

473
00:56:28.490 --> 00:56:31.139
Oliver Davies (opdavies): Let's put it… Yeah, why not?

474
00:56:31.460 --> 00:56:35.440
Oliver Davies (opdavies): Inside the list item, And… After the name.

475
00:56:36.440 --> 00:56:40.460
Oliver Davies (opdavies): So we can see we've got 13 that's had a break.

476
00:56:40.790 --> 00:56:42.580
Oliver Davies (opdavies): Let's give it a bit of space.

477
00:56:43.280 --> 00:56:50.900
Oliver Davies (opdavies): Okay, so we know we've got 13 talks altogether, and everybody's individual

478
00:56:51.030 --> 00:56:58.559
Oliver Davies (opdavies): count is 30, right? So that's the same, because all we're doing is just saying, what's the length?

479
00:57:00.200 --> 00:57:06.519
Oliver Davies (opdavies): Because it's a marked-out file, you can see sometimes it's a bit funny about spacing, which is why it's gone green, so I'll put that back.

480
00:57:07.520 --> 00:57:09.409
Oliver Davies (opdavies): If I made it a twig file, it wouldn't matter.

481
00:57:10.170 --> 00:57:16.699
Oliver Davies (opdavies): But what we want to do now is filter out these numbers so that they only show that person's.

482
00:57:16.860 --> 00:57:18.150
Oliver Davies (opdavies): count.

483
00:57:18.340 --> 00:57:26.240
Oliver Davies (opdavies): So… we're going to link, the name, so Kieran here to Kieran there.

484
00:57:26.350 --> 00:57:32.910
Oliver Davies (opdavies): Which I'm just checking, we've put it… we've put it back, so… We want to keep the length, but let's do a filter.

485
00:57:34.880 --> 00:57:37.669
Oliver Davies (opdavies): So the torque gets passed in.

486
00:57:37.780 --> 00:57:41.720
Oliver Davies (opdavies): To the, the filter, and we want to take the torque.

487
00:57:41.820 --> 00:57:46.389
Oliver Davies (opdavies): Dot speakers… Dot 0 equals…

488
00:57:47.010 --> 00:57:49.239
Oliver Davies (opdavies): Speak a name. Speak a title.

489
00:57:50.250 --> 00:57:51.059
Oliver Davies (opdavies): I think.

490
00:57:53.810 --> 00:58:04.770
Oliver Davies (opdavies): Cool. So, human's given one talk, Dan's given two talks, Dave's given none in this example, Derek's given one, James has given things, so…

491
00:58:05.170 --> 00:58:08.379
Oliver Davies (opdavies): Let's, that is working.

492
00:58:08.950 --> 00:58:13.039
Oliver Davies (opdavies): And… let's save that to a variable.

493
00:58:14.340 --> 00:58:15.750
Oliver Davies (opdavies): called Torquent.

494
00:58:16.610 --> 00:58:25.860
Oliver Davies (opdavies): So we'll just change the syntax from, say something to do something, and again, we'll just put in here, talk count.

495
00:58:26.490 --> 00:58:27.630
Oliver Davies (opdavies): Talks.

496
00:58:27.960 --> 00:58:29.360
Oliver Davies (opdavies): I'm gonna put that in a…

497
00:58:32.730 --> 00:58:33.990
Oliver Davies (opdavies): In the brackets.

498
00:58:34.270 --> 00:58:50.970
Oliver Davies (opdavies): So we can see now, it's got one talk, 2 talks, 0 talks, 1 talk, etc. Probably not that great, you know, technically, with this one, we just want to have it, say, 1, so we can do torque count equals equals 1,

499
00:58:51.190 --> 00:58:56.830
Oliver Davies (opdavies): And we could do a ternary here to say if it's 1, it's torques, and if it's not 1, it's…

500
00:58:57.570 --> 00:58:59.290
Oliver Davies (opdavies): Well, all the way around, doesn't it?

501
00:59:01.990 --> 00:59:05.190
Oliver Davies (opdavies): One… yeah, one… that's the other… the other way around.

502
00:59:05.880 --> 00:59:08.580
Oliver Davies (opdavies): Talk, singular, talk, plural.

503
00:59:10.170 --> 00:59:11.750
Oliver Davies (opdavies): joys of live coding.

504
00:59:12.700 --> 00:59:20.720
Oliver Davies (opdavies): So, if they're given one talk, this has one talk. If it's two talks, it's two… zero is… is zero talks.

505
00:59:22.740 --> 00:59:29.180
Oliver Davies (opdavies): They had the thing that after a while, words just look the same. The word talk looks really weird to me right now.

506
00:59:31.750 --> 00:59:34.130
Oliver Davies (opdavies): Okay.

507
00:59:34.630 --> 00:59:38.400
Oliver Davies (opdavies): So, we probably don't want people with zero talks.

508
00:59:38.710 --> 00:59:45.959
Oliver Davies (opdavies): So, what we can do is take the torque count that we did here, which is now a variable.

509
00:59:46.110 --> 00:59:51.560
Oliver Davies (opdavies): And let's only output that if… They've got talks.

510
00:59:51.760 --> 00:59:56.949
Oliver Davies (opdavies): So, if, toolCount is greater than 0,

511
00:59:57.220 --> 01:00:01.730
Oliver Davies (opdavies): Then we'll wrap that around our ally, and let's just copy-paste this.

512
01:00:02.200 --> 01:00:04.600
Oliver Davies (opdavies): Because why not? Save a bit of typing?

513
01:00:08.740 --> 01:00:10.569
Oliver Davies (opdavies): Sort my indentation out.

514
01:00:12.040 --> 01:00:15.990
Oliver Davies (opdavies): So hopefully now, these ones with zero torques are going to disappear.

515
01:00:18.300 --> 01:00:20.520
Oliver Davies (opdavies): Assuming I've typed everything properly.

516
01:00:21.420 --> 01:00:27.970
Oliver Davies (opdavies): There we go. That's not good. See, big red error. Key must be the name of the thing, if…

517
01:00:32.590 --> 01:00:38.520
Oliver Davies (opdavies): I've not got the holistic. That's fun. There we are.

518
01:00:39.990 --> 01:00:43.940
Oliver Davies (opdavies): Let's… There we go.

519
01:00:44.310 --> 01:00:48.889
Oliver Davies (opdavies): There's an issue that says about it crashing as well, and it recovered nicely there.

520
01:00:49.240 --> 01:00:50.149
Oliver Davies (opdavies): Good job.

521
01:00:50.760 --> 01:00:59.479
Oliver Davies (opdavies): Cool. Okay. So everything now, the… anybody with the zero talks is, is now not, not shown, in here at all.

522
01:00:59.910 --> 01:01:05.689
Oliver Davies (opdavies): So, let's do the thing Gabor said about earlier on, and let's rename this one.

523
01:01:05.910 --> 01:01:11.519
Oliver Davies (opdavies): So, I'd expect… This one's a change now, so Kieran's… Kieran is gone.

524
01:01:12.470 --> 01:01:21.100
Oliver Davies (opdavies): Because he doesn't match any talks, it's now not making that link, because we've based it all on the name matching the speakers.

525
01:01:23.590 --> 01:01:26.070
Oliver Davies (opdavies): There's book here and back, though.

526
01:01:29.950 --> 01:01:41.940
Oliver Davies (opdavies): So, it's pretty cool. And again, let me just, go back to this quick… this point quickly. If you go into output dev, all we've got is the HTML. So we can ignore that, because…

527
01:01:42.170 --> 01:01:46.109
Oliver Davies (opdavies): too old. I'll delete that, because that's…

528
01:01:46.500 --> 01:01:54.510
Oliver Davies (opdavies): our favourite word, contaminated, just because of the stuff I was doing earlier on. There is an option to do clean.

529
01:01:54.960 --> 01:02:01.539
Oliver Davies (opdavies): So just clean out the directory before it builds, to make sure that any old stuff is gone, and just reboot.

530
01:02:02.530 --> 01:02:12.020
Oliver Davies (opdavies): And let's just go back in here again. Right, this is what we've got. So, index of HTML, there's all of our HTML code.

531
01:02:12.480 --> 01:02:14.039
Oliver Davies (opdavies): There's all my talks.

532
01:02:14.430 --> 01:02:16.040
Oliver Davies (opdavies): With the titles in.

533
01:02:16.260 --> 01:02:18.510
Oliver Davies (opdavies): And then, there's all of our speakers, so…

534
01:02:18.730 --> 01:02:22.980
Oliver Davies (opdavies): and deploy this to GitHub pages, or anywhere, it will… it will just work.

535
01:02:23.140 --> 01:02:25.110
Oliver Davies (opdavies): Without the need for anything else.

536
01:02:26.780 --> 01:02:32.589
Oliver Davies (opdavies): And then… finally, I think? Like, we can click on all these, and we can…

537
01:02:32.740 --> 01:02:46.410
Oliver Davies (opdavies): click through and see the names of the people, or the talk. There's no… nothing on there. So, let's… let's build those out a bit. We can do some template inheritance, so…

538
01:02:46.590 --> 01:02:49.729
Oliver Davies (opdavies): Let's see, which one should we do first?

539
01:02:50.270 --> 01:02:51.880
Oliver Davies (opdavies): Let's do this, this.

540
01:02:52.150 --> 01:03:00.369
Oliver Davies (opdavies): titles first. So, views. So, default is our standard, or the default template.

541
01:03:00.790 --> 01:03:09.950
Oliver Davies (opdavies): Could be called anything. So there's nothing in here right now that's outputting that page title anywhere, so that's why it's not showing.

542
01:03:10.130 --> 01:03:14.150
Oliver Davies (opdavies): So… This one also extends default.

543
01:03:14.450 --> 01:03:19.300
Oliver Davies (opdavies): So, we're gonna be editing a speaker, so we're gonna edit the speaker.

544
01:03:19.430 --> 01:03:20.630
Oliver Davies (opdavies): Template funnel.

545
01:03:20.800 --> 01:03:24.419
Oliver Davies (opdavies): And now we want to override, the content.

546
01:03:24.840 --> 01:03:25.640
Oliver Davies (opdavies): block.

547
01:03:25.760 --> 01:03:27.419
Oliver Davies (opdavies): I believe we can do this now.

548
01:03:27.920 --> 01:03:32.919
Oliver Davies (opdavies): As well. So… Let's see…

549
01:03:37.110 --> 01:03:38.330
Oliver Davies (opdavies): We can't.

550
01:03:41.390 --> 01:03:51.089
Oliver Davies (opdavies): Speaker… Okay, I was hoping that was gonna… Let's go again. Yes.

551
01:03:54.590 --> 01:03:58.809
Oliver Davies (opdavies): Let's see… maybe we can't do that. Let's take that away.

552
01:04:03.810 --> 01:04:07.869
Oliver Davies (opdavies): Content? Maybe you'll… what else do we have?

553
01:04:08.550 --> 01:04:11.010
Oliver Davies (opdavies): Contact wrapper, maybe we need to edit contact wrapper.

554
01:04:13.520 --> 01:04:14.710
Oliver Davies (opdavies): There we go, okay.

555
01:04:15.410 --> 01:04:19.630
Oliver Davies (opdavies): And then inside Content Wrapper, we also want the parent. We also want the,

556
01:04:21.220 --> 01:04:23.190
Oliver Davies (opdavies): The content from inside it.

557
01:04:24.430 --> 01:04:26.859
Oliver Davies (opdavies): Right, so now have you got both. Awesome. Okay.

558
01:04:28.210 --> 01:04:34.460
Oliver Davies (opdavies): So, let's say we wanted to put the person's name in there, so we have a H1 tag.

559
01:04:34.790 --> 01:04:40.179
Oliver Davies (opdavies): do our double curly brackets, page, title… And… that was our H1.

560
01:04:42.040 --> 01:04:45.860
Oliver Davies (opdavies): And… there's our name of our person.

561
01:04:47.160 --> 01:04:52.920
Oliver Davies (opdavies): We've already got this, this comes from the content, block that we just… or the period that we just saw.

562
01:04:53.110 --> 01:04:54.659
Oliver Davies (opdavies): Totally tidy wall.

563
01:04:55.270 --> 01:05:04.210
Oliver Davies (opdavies): And… What we can then do, maybe, is have, if page.links.

564
01:05:05.080 --> 01:05:07.619
Oliver Davies (opdavies): Just build up a list of links for each person.

565
01:05:07.950 --> 01:05:09.659
Oliver Davies (opdavies): We'll make that a list again.

566
01:05:12.590 --> 01:05:14.949
Oliver Davies (opdavies): Of, of an ally.

567
01:05:18.720 --> 01:05:24.039
Oliver Davies (opdavies): So if we have links, then we're gonna show a heading, which I think's elsewhere.

568
01:05:27.520 --> 01:05:35.309
Oliver Davies (opdavies): There's a list, there's a URL, so, inside this, you'll see if, full link in page.links.

569
01:05:35.920 --> 01:05:37.809
Oliver Davies (opdavies): Then, N4.

570
01:05:38.850 --> 01:05:46.300
Oliver Davies (opdavies): more talk on Twig than anything else, really. And then in here, we want to put… the link.

571
01:05:48.580 --> 01:05:51.919
Oliver Davies (opdavies): So I don't know whether Kieran had any links. Let's find one.

572
01:05:52.150 --> 01:05:54.600
Oliver Davies (opdavies): Yep, so Dan's got a couple…

573
01:05:54.930 --> 01:06:01.820
Oliver Davies (opdavies): And then, if you want to make that a tag, an actual link, Ahref it, output the link.

574
01:06:01.950 --> 01:06:02.910
Oliver Davies (opdavies): Babe.

575
01:06:03.020 --> 01:06:05.460
Oliver Davies (opdavies): Close that, do that there.

576
01:06:06.280 --> 01:06:10.670
Oliver Davies (opdavies): Close this, go there… And… close our time.

577
01:06:14.620 --> 01:06:16.780
Oliver Davies (opdavies): Okay, so now these are…

578
01:06:16.890 --> 01:06:33.400
Oliver Davies (opdavies): proper links to each… to each thing. So, Kieran doesn't have them, because he doesn't have any links on his… on his page, right? There's no links section, in here, sorry. But if he does have them, or if the speaker has them, then…

579
01:06:33.740 --> 01:06:36.079
Oliver Davies (opdavies): Then that'll show up there.

580
01:06:36.300 --> 01:06:43.950
Oliver Davies (opdavies): And then, let's also just show… Let's bring in the…

581
01:06:44.180 --> 01:06:47.330
Oliver Davies (opdavies): list of talks that they've given.

582
01:06:48.800 --> 01:06:50.189
Oliver Davies (opdavies): So it's got caught.

583
01:06:53.230 --> 01:06:54.640
Oliver Davies (opdavies): Soap.

584
01:06:55.470 --> 01:06:59.540
Oliver Davies (opdavies): We can use the same logic, probably, that we did earlier on. So let's go back here…

585
01:07:00.030 --> 01:07:08.060
Oliver Davies (opdavies): And… we want to find… This same sort of logic, really.

586
01:07:10.850 --> 01:07:18.270
Oliver Davies (opdavies): As that talks… Bring all the talks, filter them so that, again, it matches this person.

587
01:07:18.410 --> 01:07:25.710
Oliver Davies (opdavies): And then we've been… For each… no, not for each, that's PHP, for talk in talks.

588
01:07:26.300 --> 01:07:29.860
Oliver Davies (opdavies): Yeah, same as what we did on the other… on the other page, basically.

589
01:07:30.300 --> 01:07:36.210
Oliver Davies (opdavies): And 4… I should just copy it from the other one, from the other page.

590
01:07:43.230 --> 01:07:44.260
Oliver Davies (opdavies): That's what we want.

591
01:07:46.400 --> 01:07:47.789
Oliver Davies (opdavies): I've lost my place.

592
01:07:48.480 --> 01:07:51.170
Oliver Davies (opdavies): Views.

593
01:07:52.420 --> 01:07:56.450
Oliver Davies (opdavies): So for Itch Talking Talks… We want to output that to list.

594
01:07:57.030 --> 01:07:58.660
Oliver Davies (opdavies): You want to go do the same thing.

595
01:07:59.450 --> 01:08:00.260
Oliver Davies (opdavies): Cheers.

596
01:08:02.890 --> 01:08:09.690
Oliver Davies (opdavies): So… Again, we had this earlier on, that we need to bring the talks themselves into scope.

597
01:08:10.010 --> 01:08:14.499
Oliver Davies (opdavies): So you can see use again, talks.

598
01:08:16.840 --> 01:08:19.559
Oliver Davies (opdavies): That brings them in, hopefully.

599
01:08:21.250 --> 01:08:30.470
Oliver Davies (opdavies): Mmm… Tool, title, tool, date… speakers, just looking at what's not matching.

600
01:08:30.740 --> 01:08:35.310
Oliver Davies (opdavies): That's page, because we're on the page type, not somewhere else.

601
01:08:37.200 --> 01:08:44.089
Oliver Davies (opdavies): We are in uncharted territory, I haven't rehearsed a spit, so we'll see where we are.

602
01:08:44.210 --> 01:08:46.900
Oliver Davies (opdavies): Filter, filter, expect a sequence map.

603
01:08:47.229 --> 01:08:59.750
Oliver Davies (opdavies): Data talks… Did bring my end. I'll start. I'm on the wrong… The wrong one.

604
01:09:00.979 --> 01:09:01.770
Oliver Davies (opdavies): I think.

605
01:09:02.520 --> 01:09:04.319
Oliver Davies (opdavies): Tearing the user talks.

606
01:09:07.529 --> 01:09:11.390
Oliver Davies (opdavies): But Kian… does Kian have talks? Yeah, he's got one. Where's Keenan's talk?

607
01:09:12.040 --> 01:09:18.959
Oliver Davies (opdavies): Kieran McNulty, blah blah blah, data doctor. So we can do, let's see…

608
01:09:20.510 --> 01:09:22.220
Oliver Davies (opdavies): There is a lot here to dump.

609
01:09:22.470 --> 01:09:24.910
Oliver Davies (opdavies): So, like, that's the var dump function.

610
01:09:25.100 --> 01:09:30.250
Oliver Davies (opdavies): And then doodata here, dot talks, let's just see if we're getting any.

611
01:09:30.660 --> 01:09:35.259
Oliver Davies (opdavies): Coming in at all, let's debug where this problem is…

612
01:09:40.240 --> 01:09:41.620
Oliver Davies (opdavies): It's not doing anything.

613
01:09:43.220 --> 01:09:46.900
Oliver Davies (opdavies): To fill the expected sequence map or null.

614
01:09:49.729 --> 01:09:50.830
Oliver Davies (opdavies): blah, blah, blah.

615
01:09:53.569 --> 01:09:55.329
Oliver Davies (opdavies): Let's try a different person.

616
01:09:57.250 --> 01:10:01.539
Oliver Davies (opdavies): See, like I said, uncharted territory, I should have stopped to all your head.

617
01:10:04.170 --> 01:10:04.940
Oliver Davies (opdavies): Let's…

618
01:10:05.620 --> 01:10:07.770
Gabor Szabo: The event doesn't have a speaker?

619
01:10:10.090 --> 01:10:14.410
Oliver Davies (opdavies): So this is the speaker type, so it should be… there should be…

620
01:10:14.960 --> 01:10:17.210
Oliver Davies (opdavies): I was expecting this one to show up here.

621
01:10:17.910 --> 01:10:19.200
Oliver Davies (opdavies): News Talks…

622
01:10:21.320 --> 01:10:25.589
Oliver Davies (opdavies): Let's get rid of this, because that's the bit that was… that's the bit that it's not happy with. Let's…

623
01:10:29.360 --> 01:10:33.429
Oliver Davies (opdavies): So this, yeah, it's 13, so it's obviously getting the talks through.

624
01:10:33.870 --> 01:10:39.359
Oliver Davies (opdavies): Data.talks, filter, talk… The speakers…

625
01:10:44.360 --> 01:10:44.900
Gabor Szabo: See what we can.

626
01:10:45.310 --> 01:10:47.950
Gabor Szabo: For every, every person?

627
01:10:49.440 --> 01:10:52.620
Gabor Szabo: For every presentation, do we have a speaker?

628
01:10:53.050 --> 01:10:57.990
Oliver Davies (opdavies): We should do, because we were using that earlier on, because that's what we're using on the front page to count it.

629
01:10:59.250 --> 01:11:03.689
Oliver Davies (opdavies): Let's just now, let's not… let's… let's go back to basics.

630
01:11:04.030 --> 01:11:08.829
Oliver Davies (opdavies): And… let's just… Do all the talks. Let's just check that works.

631
01:11:09.990 --> 01:11:11.139
Oliver Davies (opdavies): So that works.

632
01:11:12.700 --> 01:11:13.620
Oliver Davies (opdavies): Okay.

633
01:11:15.080 --> 01:11:16.780
Oliver Davies (opdavies): Let's debug this.

634
01:11:16.980 --> 01:11:17.850
Oliver Davies (opdavies): Page.

635
01:11:18.010 --> 01:11:21.530
Oliver Davies (opdavies): Title. Something probably just isn't named properly.

636
01:11:22.260 --> 01:11:25.809
Oliver Davies (opdavies): Kieran, and then it's also dumped out.

637
01:11:26.790 --> 01:11:36.689
Oliver Davies (opdavies): Page… Blah, blah, blah, blah, set, date, talk, let's dot this out here, page, talk.

638
01:11:39.010 --> 01:11:41.310
Oliver Davies (opdavies): tool.speakers.1.

639
01:11:46.860 --> 01:11:51.950
Oliver Davies (opdavies): Okay, so we're getting the names through, so that is fine.

640
01:11:52.390 --> 01:11:55.540
Oliver Davies (opdavies): Let's try again. Filter… Baby.

641
01:12:00.890 --> 01:12:04.929
Oliver Davies (opdavies): These are my… nope, that's the other thing we drink.

642
01:12:05.390 --> 01:12:06.260
Oliver Davies (opdavies): Talk.

643
01:12:06.600 --> 01:12:08.940
Oliver Davies (opdavies): Talk.speakers.

644
01:12:09.230 --> 01:12:14.710
Oliver Davies (opdavies): Dot zero equals equals… Pictures, please.

645
01:12:21.090 --> 01:12:23.729
Oliver Davies (opdavies): Yeah, it doesn't like that. What is going on with this?

646
01:12:25.250 --> 01:12:27.250
Oliver Davies (opdavies): God, no. Why would it be no?

647
01:12:29.610 --> 01:12:34.490
Oliver Davies (opdavies): Talking talks, we know it's not normal.

648
01:12:35.200 --> 01:12:36.849
Oliver Davies (opdavies): Because I can't do it.

649
01:12:39.380 --> 01:12:42.259
Oliver Davies (opdavies): Maybe I found a bug.

650
01:12:43.300 --> 01:12:44.590
Oliver Davies (opdavies): Let's give it one more go.

651
01:12:47.110 --> 01:12:50.860
Oliver Davies (opdavies): Filter expects a sequence mapping, or traversable.null.

652
01:12:52.840 --> 01:12:53.920
Oliver Davies (opdavies): Filter.

653
01:12:54.420 --> 01:12:55.420
Oliver Davies (opdavies): Tool.

654
01:13:00.840 --> 01:13:02.140
Oliver Davies (opdavies): Yeah, the tool.

655
01:13:04.430 --> 01:13:08.919
Oliver Davies (opdavies): Yeah, it's definitely not null, because we know they're not null, because this line tells us it's not null.

656
01:13:11.610 --> 01:13:13.370
Oliver Davies (opdavies): There we go, okay.

657
01:13:13.940 --> 01:13:17.149
Oliver Davies (opdavies): That's what I'd… it works on the other page!

658
01:13:17.280 --> 01:13:26.260
Oliver Davies (opdavies): Page title… Hmm. Okay. Let's not get too caught up, but we'll come back to it.

659
01:13:27.560 --> 01:13:33.849
Oliver Davies (opdavies): Imagine we could filter this down by the people, like we did on the other page, but for some reason isn't working on this page.

660
01:13:34.190 --> 01:13:35.140
Oliver Davies (opdavies): So…

661
01:13:35.290 --> 01:13:42.629
Oliver Davies (opdavies): We could link here and click on a person's page, see all their talks, and then click through and see them all.

662
01:13:42.940 --> 01:13:47.360
Oliver Davies (opdavies): And then the other side of that is changing the…

663
01:13:50.660 --> 01:13:53.190
Oliver Davies (opdavies): Talk content up. So, tap bait.

664
01:13:53.390 --> 01:13:54.479
Oliver Davies (opdavies): So let's end.

665
01:13:54.690 --> 01:13:55.640
Oliver Davies (opdavies): that block.

666
01:13:56.040 --> 01:14:00.039
Oliver Davies (opdavies): And then we can do the same thing, and say page title.

667
01:14:00.300 --> 01:14:03.960
Oliver Davies (opdavies): So there's our page title. Let's wrap that in a H1.

668
01:14:04.600 --> 01:14:08.579
Oliver Davies (opdavies): And let's close that off with the other H1.

669
01:14:09.710 --> 01:14:19.620
Oliver Davies (opdavies): And… Let's do parent… So it shows the contents of the other one.

670
01:14:20.340 --> 01:14:23.999
Oliver Davies (opdavies): Nothing in that, I don't think, so…

671
01:14:28.520 --> 01:14:33.620
Oliver Davies (opdavies): I'm gonna play as one of mine, so I should… Let's grab this quickly.

672
01:14:35.150 --> 01:14:37.840
Oliver Davies (opdavies): Blah blah blah… grab.

673
01:14:39.110 --> 01:14:39.870
Oliver Davies (opdavies): Paste.

674
01:14:42.880 --> 01:14:44.519
Oliver Davies (opdavies): There you go. So…

675
01:14:44.770 --> 01:14:51.969
Oliver Davies (opdavies): And then, let's say you want to do the opposite, but we could do speakers.

676
01:14:52.770 --> 01:14:55.430
Oliver Davies (opdavies): Let's make that, another H2.

677
01:14:56.370 --> 01:15:01.169
Oliver Davies (opdavies): And… let's… Make that a list. I should have made the snippet for this.

678
01:15:01.510 --> 01:15:07.259
Oliver Davies (opdavies): And then say… 4 speaker.

679
01:15:07.800 --> 01:15:09.380
Oliver Davies (opdavies): In Page.

680
01:15:09.530 --> 01:15:15.070
Oliver Davies (opdavies): Because… It's hand not floated.

681
01:15:21.470 --> 01:15:22.820
Oliver Davies (opdavies): People are taller.

682
01:15:25.520 --> 01:15:26.920
Oliver Davies (opdavies): Interesting.

683
01:15:28.220 --> 01:15:29.909
Oliver Davies (opdavies): Oh, because it's just me.

684
01:15:33.830 --> 01:15:40.630
Oliver Davies (opdavies): quite a lot of the H1 anymore. H1, H1, H2, let's close our H2 there.

685
01:15:42.990 --> 01:15:46.480
Oliver Davies (opdavies): Cool, and then let's link that to the other page.

686
01:15:51.680 --> 01:15:59.890
Oliver Davies (opdavies): Yeah, we're gonna have to do more logic there to load up the speakers and filter them by name and everything else. We can see where we are, but…

687
01:16:00.090 --> 01:16:04.340
Oliver Davies (opdavies): Yeah, imagine if we were to do that, we could click here, go back to the page.

688
01:16:04.850 --> 01:16:09.890
Oliver Davies (opdavies): And go back to see here, and click to the page, so… Yeah, that's…

689
01:16:10.500 --> 01:16:12.540
Oliver Davies (opdavies): That's what I was planning on demoing.

690
01:16:12.840 --> 01:16:15.330
Oliver Davies (opdavies): And… yeah, you can see how…

691
01:16:15.760 --> 01:16:32.349
Oliver Davies (opdavies): We've made a fairly dynamic site with a lot of interesting information, like the number of speakers and talks, a list of the speakers, how many talks each one has given, and when each talk was, etc. We can see the speakers for each talk.

692
01:16:32.470 --> 01:16:35.600
Oliver Davies (opdavies): And we can see the talks that each person has given.

693
01:16:35.860 --> 01:16:41.619
Oliver Davies (opdavies): Which is how it's not loading. It's weird. I'm gonna look at that in a minute.

694
01:16:41.800 --> 01:16:53.300
Oliver Davies (opdavies): Yeah, and again, this is all driven by these markdown files and these twig templates that Skullpin is then processing, and then outputting the HTML for, and…

695
01:16:53.610 --> 01:16:59.029
Oliver Davies (opdavies): We could take this HTML, and then deploy it anywhere, or the HTML.

696
01:16:59.410 --> 01:17:01.740
Oliver Davies (opdavies): That's HTML. The whole thing.

697
01:17:01.940 --> 01:17:08.659
Gabor Szabo: Yeah, it's pretty impressive. There is one bug at, it's not called Maven PHP, it's called PHP Maven.

698
01:17:08.660 --> 01:17:10.810
Oliver Davies (opdavies): Oh, sorry, you should have told me at the beginning.

699
01:17:10.810 --> 01:17:11.290
Gabor Szabo: Yeah, yeah.

700
01:17:12.070 --> 01:17:14.349
Gabor Szabo: I was wondering what I should say yours.

701
01:17:14.350 --> 01:17:15.350
Oliver Davies (opdavies): Oh, well, it's neat.

702
01:17:15.350 --> 01:17:20.590
Gabor Szabo: Like, after this time, I started looking, and I really, really have to say that

703
01:17:20.590 --> 01:17:23.869
Oliver Davies (opdavies): Yeah. So he… On the back.

704
01:17:23.870 --> 01:17:26.839
Gabor Szabo: like PHP expert, just PHP Media, right?

705
01:17:27.520 --> 01:17:30.130
Oliver Davies (opdavies): I could say I did this on purpose, right?

706
01:17:30.760 --> 01:17:35.090
Oliver Davies (opdavies): This is the thing, right, with the static sites. If I want to change it now.

707
01:17:35.320 --> 01:17:40.480
Oliver Davies (opdavies): on every page, I can change it in one place, and it will update it on every page.

708
01:17:40.670 --> 01:17:52.010
Oliver Davies (opdavies): So I can just go into the site generator, and change this here, and say PHP Maven Type.

709
01:17:52.650 --> 01:17:56.990
Oliver Davies (opdavies): Now, there's… Is going to need a restart, because it caches.

710
01:17:57.350 --> 01:17:59.270
Oliver Davies (opdavies): The settings for all.

711
01:18:00.090 --> 01:18:03.569
Oliver Davies (opdavies): Let's just start that again. Let's do no interaction.

712
01:18:07.870 --> 01:18:08.940
Oliver Davies (opdavies): Alright, fine.

713
01:18:10.420 --> 01:18:21.620
Oliver Davies (opdavies): So now it's changed here, on this heading, PHP Maven, it's changed there to PHP Maven, it's changed this one, it's changed everywhere, to PHP Maven.

714
01:18:21.620 --> 01:18:22.680
Gabor Szabo: Yeah, of course.

715
01:18:22.680 --> 01:18:26.550
Oliver Davies (opdavies): Which is cool, and then if I refresh this as well, so…

716
01:18:26.780 --> 01:18:36.930
Oliver Davies (opdavies): Yeah, if I, you know, for example, this is one of the big benefits, is let's say, on my site, and I change one of these links down here.

717
01:18:37.130 --> 01:18:38.740
Oliver Davies (opdavies): I change it in one place.

718
01:18:38.850 --> 01:18:45.140
Oliver Davies (opdavies): And then when I regenerate the site, it will regenerate it for every page.

719
01:18:45.140 --> 01:18:57.360
Gabor Szabo: I have implemented… I maintain a website for a local Python community, one for the local Rust community, and each one, I wrote the whole thing in Python and Rust. It's very impressive.

720
01:18:57.360 --> 01:18:57.750
Oliver Davies (opdavies): Is that true?

721
01:18:57.750 --> 01:19:00.350
Gabor Szabo: I'm using only templating system for that.

722
01:19:01.800 --> 01:19:18.130
Oliver Davies (opdavies): And this is why my… this is why my site takes 30 seconds to build, because the number of content, the amount of posts, but also I'm using it in a few different ways. So, I've got latest posts here, I've got upcoming presentations, I've got featured presentations, so…

723
01:19:18.310 --> 01:19:19.210
Oliver Davies (opdavies): Show you.

724
01:19:19.210 --> 01:19:19.720
Gabor Szabo: Lisa.

725
01:19:19.720 --> 01:19:27.999
Oliver Davies (opdavies): So, on my site here, I've got, sites… My sight, souls.

726
01:19:28.400 --> 01:19:34.739
Oliver Davies (opdavies): So, I have a presentation's content type, and we can see in here, scoping…

727
01:19:35.290 --> 01:19:46.589
Oliver Davies (opdavies): It's gonna get very meta. Yeah, this one is featured, so I'm just filtering that based on only showing me presentations that are featured, and then each…

728
01:19:46.680 --> 01:20:01.260
Oliver Davies (opdavies): each presentation has, events, and each event has got a date, and yeah, so instead of having all this in a database, the YAML file, or the Markdown files, are my database, essentially.

729
01:20:03.020 --> 01:20:03.939
Gabor Szabo: Very nice.

730
01:20:04.300 --> 01:20:12.929
Oliver Davies (opdavies): So yeah, that's… that's it. Yeah, I could deploy this anywhere, and it would just work, which I think is pretty cool.

731
01:20:14.630 --> 01:20:20.260
Gabor Szabo: Well, thank you. Anyone with any questions we'd like to ask now?

732
01:20:25.570 --> 01:20:35.629
Gabor Szabo: Can you, actually, if you, if you don't, if you, what I, I think, I think I started to ask, I'm not sure if the answer was there and just missed it, if you,

733
01:20:37.190 --> 01:20:51.280
Gabor Szabo: So you showed what happens if the name is incorrect, or what, but it doesn't show on the website, but can we get some error message on the processing, or…

734
01:20:51.280 --> 01:21:02.699
Gabor Szabo: some way to the… so I… so it won't just disappear from the website, which I will probably miss, but I will get some, let's say, an error message

735
01:21:02.720 --> 01:21:04.450
Gabor Szabo: While building the website.

736
01:21:05.420 --> 01:21:21.769
Oliver Davies (opdavies): So, because it's all fairly freeform, you can put anything in the markdown area that you want, it's not strongly typed or anything like that, it wouldn't flag it as an error on build, by default.

737
01:21:21.920 --> 01:21:36.380
Oliver Davies (opdavies): But there are hooks and events that you can subscribe to where you can add that type of logic. On my site, for example, if I've got, like, a ZZ or a to-do, in a post, it won't… it won't build.

738
01:21:36.380 --> 01:21:41.969
Oliver Davies (opdavies): Because I want to make sure that I'm not publishing a post accidentally, but I'm in the middle of writing.

739
01:21:42.130 --> 01:21:49.910
Oliver Davies (opdavies): So you can extend, you know, this by having, custom bundles and…

740
01:21:50.060 --> 01:21:53.960
Oliver Davies (opdavies): whatever else, and I can show you an example of that, if you like.

741
01:21:54.860 --> 01:22:02.810
Oliver Davies (opdavies): Which is going to be in… Packages… I think?

742
01:22:03.130 --> 01:22:04.100
Oliver Davies (opdavies): They go.

743
01:22:07.020 --> 01:22:23.820
Oliver Davies (opdavies): presentation span, I'm trying to think where it is anymore. The other thing that's interesting is I've been playing with this thing called Sculpting Validator, so it's gone to look at what's in the front matter, and if I've typed something, which I do fairly often, I always do uses, that would throw an error.

744
01:22:23.920 --> 01:22:28.300
Oliver Davies (opdavies): But it's, that's a separate, sort of, symphony validator type of thing.

745
01:22:28.860 --> 01:22:33.929
Oliver Davies (opdavies): Yeah, I don't know where that is, Ryan, actually. Must be in there somewhere.

746
01:22:34.220 --> 01:22:36.260
Kevin Boyd: Yeah, and there is a bit of documentation.

747
01:22:36.260 --> 01:22:51.649
Oliver Davies (opdavies): And that's why I like it, because it's, it's PHP. So I know how dependency injection container works, I know how Symphony works, and the components that it uses from Symphony. So I know how…

748
01:22:51.700 --> 01:23:00.079
Oliver Davies (opdavies): I could extend it, whereas I wouldn't be able to do that as easily if it was in Go, or JavaScript, or Python, or Rust, or something.

749
01:23:01.690 --> 01:23:04.009
Gabor Szabo: Yeah. Kevin, did you want to say something? Oh, yeah.

750
01:23:04.010 --> 01:23:07.880
Kevin Boyd: I was gonna say that on the Skelton website, Look at that.

751
01:23:07.880 --> 01:23:09.230
Oliver Davies (opdavies): Really?

752
01:23:09.230 --> 01:23:10.670
Gabor Szabo: An oliver doesn't hear you.

753
01:23:10.670 --> 01:23:11.980
Oliver Davies (opdavies): You could do a lot with it.

754
01:23:11.980 --> 01:23:13.200
Gabor Szabo: Can you hear me?

755
01:23:13.200 --> 01:23:15.810
Kevin Boyd: I can hear you, yep. Can you hear me, Oliver?

756
01:23:15.810 --> 01:23:27.550
Oliver Davies (opdavies): The website is sculpin.io, that's where the main documentation is. The codes are GitHub, and the starting point for this demo, is on this URL, but I will…

757
01:23:27.900 --> 01:23:35.359
Oliver Davies (opdavies): Commit everything and push you to a branch after this, so people can refer back to it, or if anybody wants to, after watching the video.

758
01:23:35.690 --> 01:23:40.290
Oliver Davies (opdavies): Got some examples on, PHP and Nix versions.

759
01:23:40.430 --> 01:23:59.859
Oliver Davies (opdavies): And then my site we were just looking at was, is, is there as well, and I do quite a lot of writing about Sculping as well, so quite, quite also recently. Again, it's a bit meta, it's a Sculpin site where I'm posting about sculpting, a lot. But, yeah.

760
01:24:00.090 --> 01:24:08.050
Oliver Davies (opdavies): Follow, follow me, this is all… we talked about RSS earlier on, this is all about RSS for everything on here, so, go mad, go mad.

761
01:24:08.720 --> 01:24:14.170
Oliver Davies (opdavies): So, yeah, feel free to get in touch with him, he's got questions, after the recording, or…

762
01:24:14.170 --> 01:24:14.919
Gabor Szabo: Can you hear me now?

763
01:24:14.920 --> 01:24:15.870
Oliver Davies (opdavies): If you'd like.

764
01:24:16.760 --> 01:24:17.849
Gabor Szabo: Can you hear me now?

765
01:24:18.860 --> 01:24:31.269
Gabor Szabo: No, he can't hear us. This happened, so I'm going to close the video now, soon. Did you try to say something, Kevin? Apparently, Oliver sometimes doesn't hear us, and this is one of the cases, because he…

766
01:24:31.270 --> 01:24:32.090
Oliver Davies (opdavies): Questions?

767
01:24:32.630 --> 01:24:34.250
Kevin Boyd: Yeah,

768
01:24:34.250 --> 01:24:38.260
Oliver Davies (opdavies): Anything else I should show that's really cool in scoping 4 that I've missed?

769
01:24:39.660 --> 01:24:41.129
Kevin Boyd: Can you hear me, Oliver?

770
01:24:44.220 --> 01:24:45.490
Gabor Szabo: Apparently can't.

771
01:24:48.710 --> 01:25:02.460
Kevin Boyd: Yeah, there are cool things in Sculpin 4, such as this editor, where you can edit the page directly or add a file. This editor only shows when you are in the local run mode, it doesn't show on your actual web page.

772
01:25:03.460 --> 01:25:09.870
Kevin Boyd: And it is actually injected on the fly as you… Are in development mode.

773
01:25:10.550 --> 01:25:11.210
Gabor Szabo: Yeah.

774
01:25:11.610 --> 01:25:12.829
Kevin Boyd: It's pretty fun to implement.

775
01:25:16.510 --> 01:25:17.290
Gabor Szabo: Okay.

776
01:25:19.080 --> 01:25:24.909
Gabor Szabo: Yeah, well, Oliver can't hear us, I guess, so I will just…

777
01:25:25.610 --> 01:25:38.759
Gabor Szabo: thank him, then later write to him, so you'll know that he actually liked what he did. So thank you very much for all of you also who were here and commented.

778
01:25:38.760 --> 01:25:48.749
Gabor Szabo: And thanks for those people who are watching us, thanks for watching, and please like the video, and follow the channel, and below the video, you will find the links.

779
01:25:48.870 --> 01:26:05.260
Gabor Szabo: to also, Olivia said that he is going to publish whatever he made into his Git repository, and we'll put the link there, so you can find that, and that's it. I hope to see you in…

780
01:26:05.260 --> 01:26:09.849
Gabor Szabo: one of the upcoming, events. Thank you, and, bye-bye!

