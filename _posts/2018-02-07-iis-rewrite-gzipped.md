---
layout: post
title: "URL Rewriting to serve gzipped assets on IIS"
date: 2018-02-07 11:23
comments: false
categories: Blag
---

Saving this here because I haven't found a complete setup that works, and I'm probably going to need it again later.

First, it handles both `*.js` and `*.css`. Adding the `$` to the end of the match rule is important to avoid matching `*.js.map` and having it serve the gzipped JavaScript file instead. Basic regular expressions of course, but too easy to forget.

We want to make sure the file actually exists before attempting to serve it. It's better to serve an uncompressed asset than a 404. So add a condition to see if it fits. Each path probably depends on the configuration of the project. Ideally it would just check in the directory it was already going to serve from, but that seemed not to work very well.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Serve gzipped javascript" enabled="true" stopProcessing="false">
          <match url="(.*).js$" />
          <conditions>
            <add input="{DOCUMENT_ROOT}/wwwroot/{R:0}.gz" matchType="IsFile" negate="false" />
          </conditions>
          <action type="Rewrite" url="{R:1}.js.gz" appendQueryString="true" logRewrittenUrl="true" />
        </rule>
        <rule name="Serve gzipped CSS" enabled="true" stopProcessing="false">
          <match url="(.*).css$" />
          <conditions>
            <add input="{DOCUMENT_ROOT}/wwwroot/{R:0}.gz" matchType="IsFile" negate="false" />
          </conditions>
          <action type="Rewrite" url="{R:1}.css.gz" appendQueryString="true" logRewrittenUrl="true" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Then we have to make sure we set the correct headers on the outbound response.
All gzipped files should get `Content-Encoding` set to `gzip`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
      <outboundRules>
        <rule name="Rewrite gzip content encoding header" preCondition="IsGzipped" stopProcessing="false">
          <match serverVariable="RESPONSE_CONTENT_ENCODING" pattern=".*" />
          <action type="Rewrite" value="gzip" />
        </rule>
        <preConditions>
          <preCondition name="IsGzipped">
            <add input="{URL}" pattern="\.gz$" />
          </preCondition>
        </preConditions>
      </outboundRules>
    </rewrite>
  </system.webServer>
</configuration>
```

Then we override the `Content-Type` and set it to `application/javascript` or `text/css` depending on the file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <outboundRules>
      <rule name="Rewrite gzip content type header (JS)" preCondition="IsGzippedJavaScript" stopProcessing="false">
        <match serverVariable="RESPONSE_CONTENT_TYPE" pattern=".*" />
        <action type="Rewrite" value="application/javascript" />
      </rule>
      <rule name="Rewrite gzip content type header (CSS)" preCondition="IsGzippedCSS" stopProcessing="false">
        <match serverVariable="RESPONSE_CONTENT_TYPE" pattern=".*" />
        <action type="Rewrite" value="text/css" />
      </rule>
      <preConditions>
        <preCondition name="IsGzippedJavaScript">
          <add input="{URL}" pattern="\.js\.gz$" />
        </preCondition>
        <preCondition name="IsGzippedCSS">
          <add input="{URL}" pattern="\.css\.gz$" />
        </preCondition>
      </preConditions>
    </outboundRules>
  </system.webServer>
</configuration>
```

Finally, we make sure IIS doesn't decide to do any static compression of its own. Doing the compression once at build time is much better use of our time than doing it over and over and over again on each request.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <urlCompression doStaticCompression="false" />
  </system.webServer>
</configuration>
```

[Full file available](/blag/files/iis-rewrite-gzip.xml)
