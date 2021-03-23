***The included source code, service and information is provided as is, and OmniUpdate makes no promises or guarantees about its use or misuse. The source code provided is recommended for advanced users and may not be compatible with all implementations of OU Campus.***

# Redirect Template

This is an OU Campus template (TCF, TMPL, Image) that creates a simple meta redirect page. This allows you to create a "page" in a folder named `/rd` that will effectively become a short URL that points to the full page.

## PHP Redirect

This package includes a `/php-redirect` folder that contains an alternative redirect option, allowing the user to create a page in one location that publishes a PHP redirect pointing to another specified location through the use of the PHP `header` function. This templateuses a PCF page transformed by XSL code to make the redirect location easily editable. The XSL could also be modified by a web developer to output different types of redirects.

## IIS Config Redirect

This package includes an `/iis-config-redirect` folder that contains an alternative redirect option, allowing the user to create a series of XML files which specify source/destination URL pairs which can be compiled into a `rewriteMap` element which can be imported into an IIS web.config file's rewrite element:

```
<configuration>
    <system.webServer>
        <rewrite>
            <!--
            web-rewrite-templates.config is generated by publishing web-rewrite-templates.pcf.
            -->
            <rewriteMaps configSource="web-rewrite-templates.config" />
            <rules>
                <!--
                example rules using the generated TemplateRedirects rewriteMap
                -->
                <rule name="template mapped with trailing slash">
                    <match url=".*/$" />
                    <conditions>
                        <add input="{TemplateRedirects:{URL}}" pattern="(.+)" />
                    </conditions>
                    <action type="Redirect" redirectType="Permanent" url="{C:1}" appendQueryString="true" />
                </rule>
                <rule name="template mapped without trailing slash">
                    <match url=".*" />
                    <conditions>
                        <add input="{TemplateRedirects:{URL}/}" pattern="(.+)" />
                    </conditions>
                    <action type="Redirect" redirectType="Permanent" url="{C:1}" appendQueryString="true" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
</configuration>
```

The redirect.tcf will create /web-rewrite-templates.pcf and an XML file (in /_redirects/) named after the path entered for the source URL. Source URLs are root-relative and must be unique, while destination URLs may be either root-relative or absolute. To change the destination of a particular source URL, edit the XML file.
