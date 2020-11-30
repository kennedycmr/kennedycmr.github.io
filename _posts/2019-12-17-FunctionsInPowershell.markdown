---
layout: post
title: "Functions in PowerShell"
date: 2019-12-17 15:20:00 -0500
background: '/img/posts/powershellHeader.jpg'
categories: powershell
tags: powershell functions
---

<p>Functions in PowerShell, and most languages, are a way to create smaller, condensed units of reusable code inside a larger piece of code. 
I remember back in high school learning BASIC or even PASCAL, I would have a tenancy to create little programs that read like a novel - from start to finish. 
However, your code should really be more like a reference manual, that has chapters of information that are regularly consulted while reading. That is what functions are for.</p> 

<p>By using functions, I find the following benefits: 
<li>The code is smaller - with the basic stuff I write, this doesn't seem like a big deal because who cares if a script is 5 kB or 10 kB.</li> 
<li>The code is easier to read - assuming the functions are labeled well and commented.</li> 
<li>It forces me to parametrize everything and not use hard-coded variables.</li>
<li>It forces me to think more strategically about my code for it to glue together well.</li></p> 

<p>The downsides I find are: 
<li>It takes longer to write - because i have to spend time considering how to make the function re-usable it takes more time to consider the ramifications to other code that may not be created yet.</li> 
<li>Syntax around passing variables gets more complicated.</li> 
<li>My personal style is to start a script that I am not entirely sure how it will work and then, before I know it, it is 1000 lines long so now I have to go back and move things into functions.</li> 

<p>A basic function example that i use often looks like this:</p> 

{% highlight powershell %}
Function do-Something {  # This does something awesome.
  [cmdletbinding()]
  Param (
    [string]$vCenter,
	[string]$reportOutput,
	[string]$login,
	[string]$password
  )

  $pwd = ConvertTo-SecureString $password -AsPlainText -Force;
  $Cred = New-Object -TypeName System.Management.Automation.PSCredential -argumentlist $login,$pwd;
  & "$SomeVMwareScript" -vCenterServer $vCenter -ReportOutput "$WorkingFolder" -Credential $Cred
}

### I call the function like this
$vCenter = 'Appliance.domain.com'
$InventoryLocation = '\\Server\Some\Share'
$login = 'RO-Username'
$password = 'password'

do-Something -vCenter $vCenter -WorkingFolder $InventoryLocation -login $login -password $password
{% endhighlight %}

<p>In the above example, I have a function called "do-Something" that accepts various string parameters. When I call that function, I pass variables as parameters. That function then calls another PowerShell script and passes variables as parameters to it. In the above example I am also showing one way to handle credentials. Personally I try at all costs not to have credentials handled in a plain text fashion, but in the world of Infrastructure where you are gluing disparate applications together, you just don't have a choice.</p> 

