# chefmill-cookbook
Empty cookbook for the ChefMill application

## Goal

Create a Chef cookbook with a recipe that will install and configure "Chef Mill",
a custom .NET console application that when run should display "Congratulations Chef!"
when configured properly.

## Background

If you've never heard of Chef you should go read up about Chef. At the very least you 
should learn what a Chef cookbook and recipe is. From there I strongly recommend you read
[this blog post](http://engineering.daptiv.com/getting-started-writing-a-chef-cookbook-for-windows/)
which walks you through the steps of creating a very basic Windows cookbook with Vagrant.

## Instructions

You should fork this GitHub repository and use it as a starting place to do your work. The
expectation is that you use the same resources you would if you were creating a real
cookbook at work.

The Chef Mill cookbook you're creating should install the [Chef Mill](https://github.com/daptiv/ChefMill)
application onto a Windows box that already has .NET installed. You will need to use the
[windows_package](https://docs.chef.io/resource_windows_package.html) resource from the
Chef Windows cookbooks in order to install the
[MSI that we've already pre-built](https://github.com/daptiv/ChefMill/releases/download/1.0.0.0/ChefMillInstaller.msi)
for you.

In addition to downloading and installing the MSI, you're Chef cookbook will need to properly
configure Chef Mill's .NET configuration file to properly set the license key. Chef Mill
expects the license key to be set in its configuration file like so:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <add key="license_key" value="license key goes here"/>
  </appSettings>
  <startup> 
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
  </startup>
</configuration>
```

If you're not familiar with .NET configuration files, they're just regular XML files that
follow the convention:

1. Must be in the same directory as the assembly or executable
2. Must have the same name as the assembly + '.config' (i.e. ChefMill.exe.config)

The license key value should be configurable based off a cookbook attribute referred to
as `node['chefmill']['license_key']` in the cookbook. For _this_ exercise we're going to
set the default value of this attribute to '96DBA4DB-A86B-43E0-9256-370986D05A12', which
will allow the Chef Mill application to work correctly. You'll likely want to use a Chef
[template](https://docs.chef.io/resource_template.html) resource for create and configure
the Chef Mill application configuration file.

When you're all done, Chef should have installed Chef Mill and configured it so that when
you run the console application it outputs "Congratulations Chef!".
