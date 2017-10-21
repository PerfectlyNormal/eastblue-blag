---
layout: post
title: "Hosting an ASP.NET Core and an ASP.NET application in the same IIS site"
date: 2017-10-21 11:23
comments: false
categories: Blag
---

I've been working on an Angular app for a client for a while now, and we've finally been getting ready for a beta-release. The app has the Angular frontend, the API written in ASP.NET and a separate admin-section also written in ASP.NET. The Angular frontend has a tiny ASP.NET Core wrapper that handles URL rewriting, setting HTTP headers and so on, and integrates nicely into IIS.

As we were getting closer, it got decided that instead of running the admin-section on a separate subdomain, it should be served as part of the main site, as a subapplication in IIS. No problems, right?

Turns out, the ASP.NET Core module for IIS is a bit greedy, or something else interferes, but every request gets sent down to the Core app, which doesn't know how to handle requests to `/admin` and fails. In my mind, this is something IIS should handle, by looking at the URL and deciding this should hit the subapplication, never involving our Core app.

Did a few searches, which turned up a ton of other people having the same issue, but no real solution in sight.
So I did what I do, and kept banging my head against the wall until something broke. This time it was IIS, and I managed to fix my issue.

First, the ASP.NET Core app has a basic `Web.config`:

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" type="" modules="AspNetCoreModule" scriptProcessor="" resourceType="Unspecified" requireAccess="Script" allowPathInfo="false" preCondition="" responseBufferLimit="4194304" />
    </handlers>
    <aspNetCore processPath="dotnet" arguments=".\bin\Debug\netcoreapp2.0\myapp.dll" stdoutLogEnabled="true" stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

and then the trick was, in my subapplication, to remove the `aspNetCore` handler, again in `Web.config`. This doesn't seem to affect any other configured site, or have any effects beyond the scope of our subapplication.

```xml
<configuration>
  <!-- stripped -->
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
  </system.webServer>
</configuration>
```

It's been happily serving requests for a few days now without any issues popping up.

I'm guessing the `aspNetCore` handler runs for our admin-application, detects that the app "one level up" contains an ASP.NET Core app and launches that instead of passing along control to the next handler, as I think it should.