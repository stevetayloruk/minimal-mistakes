---
title: "Cascading Dropdowns with WCF RESTful webservices, LINQ to SQL, JSON and jQuery"
tags:
  - .Net
  - Automotive
  - JavaScript
  - Linq
  - WCF
---

For anyone that has ever built an automotive dealer website, you would have come across the need to implement dropdown lists that link to each other, or as I call them, Cascading Dropdowns. When doing a search for a used car, the user typically wants to narrow down the search based on the make and model and specification of the car. 

Before AJAX frameworks became popular and cross browser compatible, the only real choice was to output a JavaScript array. This solution was fine for a small amount of data, but the implementation tended to be messy and didn't fit well with a well defined architecture.&nbsp; This situation got a little better when the webservice.htc was released, but again this was IE only. 

So, how would we implement this common scenario with today's technology? 

First of all create a new ASP.NET website and choose your preferred language, I'm using C#. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image11_thumb.png" alt="Visual Studio">

Next create a new SQL database. For this demo I've created a SQL Express 2005 DB and called it "VehicleDatabase". 

Next create three tables, Makes, Models and Derivatives. All ID's are of type INT and the Name fields are of type VARCHAR.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/VehicleDBDiagram_thumb.gif" alt="ERD">

Next we create the stored procedures to query the tables.&nbsp; Create the following three stored procedures: 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">create</span> <span style="color: #0000ff">proc</span> dbo.GetMakes
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span> <span style="color: #0000ff">as</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span> <span style="color: #0000ff">set</span> nocount <span style="color: #0000ff">on</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span> <span style="color: #0000ff">select</span> MakeId, MakeName <span style="color: #0000ff">from</span> Makes
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span> <span style="color: #0000ff">return</span>
</pre>
</div>
</div>
<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">create</span> <span style="color: #0000ff">proc</span> dbo.GetModels
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>     @MakeId <span style="color: #0000ff">int</span> = -1
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span> <span style="color: #0000ff">as</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span> <span style="color: #0000ff">set</span> nocount <span style="color: #0000ff">on</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span> <span style="color: #0000ff">select</span> ModelId, ModelName <span style="color: #0000ff">from</span> Models <span style="color: #0000ff">where</span> MakeId = <span style="color: #0000ff">case</span> <span style="color: #0000ff">when</span> @MakeId = -1 <span style="color: #0000ff">then</span> MakeId <span style="color: #0000ff">else</span> @MakeId <span style="color: #0000ff">end</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span> return
</pre>
</div>
</div>
<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">create</span> <span style="color: #0000ff">proc</span> dbo.GetDerivatives
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>     @MakeId <span style="color: #0000ff">int</span> = -1,
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>     @ModelId <span style="color: #0000ff">int</span> = -1
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span> <span style="color: #0000ff">as</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span> <span style="color: #0000ff">set</span> nocount <span style="color: #0000ff">on</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span> <span style="color: #0000ff">select</span> DerivativeId, DerivativeName <span style="color: #0000ff">from</span> Derivatives d
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span> <span style="color: #0000ff">inner</span> <span style="color: #0000ff">join</span> Models m <span style="color: #0000ff">on</span> m.ModelId = d.ModelId
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span> <span style="color: #0000ff">where</span> (d.ModelId = <span style="color: #0000ff">case</span> <span style="color: #0000ff">when</span> @ModelId = -1 <span style="color: #0000ff">then</span> d.ModelId <span style="color: #0000ff">else</span> @ModelId <span style="color: #0000ff">end</span>)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span> <span style="color: #0000ff">and</span> (m.MakeId = <span style="color: #0000ff">case</span> <span style="color: #0000ff">when</span> @MakeId = -1 <span style="color: #0000ff">then</span> m.MakeId <span style="color: #0000ff">else</span> @MakeId <span style="color: #0000ff">end</span>)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span> return
</pre>
</div>
</div>

These stored procedures simply just return the required fields, the only thing to note is on the GetModels and GetDerivatives SP's, the parameters have a default value of -1. We also do an inline case statement to check for that default value and if matched, return all of the records. This allows us to populate the dropdowns with all available data before we perform the filter. 

Add some test data to the tables. 

Now that the database is sorted we need to create our entities. For this example, we don't really need the entities as we are using stored procs that return a generic dictionaries but it gives you a flavour on how it can fit into you existing architecture. Add a new item to our website and choose the LINQ to SQL Classes. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image_thumb.png" alt="Add New Item">

Now drag you tables from the Server Explorer onto the left hand side of the designer. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image3_thumb.png" alt="Class Diagram">

And drag the stored procs onto the right hand side of the designer. Alternatively we could have dropped the stored procs onto their corresponding table. This would tell the designer that the return type of SP would be of that table/entity. So for example, if we were to drop the &quot;GetMakes&quot; SP onto the "Makes" table we would be telling the designer that the "GetMakes" SP returns a set of Make objects. 

I have decided to return a Generic Dictionary&lt;string, string&gt; instead of the entities as I only want to return a key and value for each make, model or derivative. 

As we are exposing this data via WCF we need to change the Serialization Mode from "None" to "Unidirectional". This can be achieved through the properties of the LINQ to SQL Classes. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image_thumb_3.png" alt="Unidirectional">

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image_thumb_4.png" alt="Class Diagram">

Right, now we have our data, we need to expose it through WCF webservices. Add a new item to our website and choose the WCF service item. I've called my service "VehicleService.svc". Click yes when it asks you if you want to place it into the App_Code folder. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image19_thumb.png" alt="Add New Item">

Once we have added the service, we need to alter the web.config. When adding a service to the project, Visual Studio changes the web.config and puts some default settings in for us. Copy and paste the XML below and replace the serviceModel section of the web.config. The main points here to notice is the endpoint binding is set to webHttpBinding rather than the SOAP default and the behaviour has httpGetEnabled = true. 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">system.serviceModel</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span>         <span style="color: #0000ff">&lt;</span><span style="color: #800000">services</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>             <span style="color: #0000ff">&lt;</span><span style="color: #800000">service</span> <span style="color: #ff0000">behaviorConfiguration</span><span style="color: #0000ff">=&quot;VehicleServiceBehavior&quot;</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">=&quot;VehicleService&quot;</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>                 <span style="color: #0000ff">&lt;</span><span style="color: #800000">endpoint</span> <span style="color: #ff0000">binding</span><span style="color: #0000ff">=&quot;webHttpBinding&quot;</span> <span style="color: #ff0000">contract</span><span style="color: #0000ff">=&quot;IVehicleService&quot;</span> <span style="color: #ff0000">behaviorConfiguration</span><span style="color: #0000ff">=&quot;WebHttpBehavior&quot;</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>                 <span style="color: #0000ff">&lt;/</span><span style="color: #800000">endpoint</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>             <span style="color: #0000ff">&lt;/</span><span style="color: #800000">service</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>         <span style="color: #0000ff">&lt;/</span><span style="color: #800000">services</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>     <span style="color: #0000ff">&lt;</span><span style="color: #800000">behaviors</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>       <span style="color: #0000ff">&lt;</span><span style="color: #800000">serviceBehaviors</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>         <span style="color: #0000ff">&lt;</span><span style="color: #800000">behavior</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">=&quot;VehicleServiceBehavior&quot;</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>           <span style="color: #0000ff">&lt;</span><span style="color: #800000">serviceMetadata</span> <span style="color: #ff0000">httpGetEnabled</span><span style="color: #0000ff">=&quot;true&quot;</span><span style="color: #0000ff">/&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>           <span style="color: #0000ff">&lt;</span><span style="color: #800000">serviceDebug</span> <span style="color: #ff0000">includeExceptionDetailInFaults</span><span style="color: #0000ff">=&quot;true&quot;</span><span style="color: #0000ff">/&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>         <span style="color: #0000ff">&lt;/</span><span style="color: #800000">behavior</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span>       <span style="color: #0000ff">&lt;/</span><span style="color: #800000">serviceBehaviors</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  15:</span>       <span style="color: #0000ff">&lt;</span><span style="color: #800000">endpointBehaviors</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  16:</span>         <span style="color: #0000ff">&lt;</span><span style="color: #800000">behavior</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">=&quot;WebHttpBehavior&quot;</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  17:</span>           <span style="color: #0000ff">&lt;</span><span style="color: #800000">webHttp</span><span style="color: #0000ff">/&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  18:</span>         <span style="color: #0000ff">&lt;/</span><span style="color: #800000">behavior</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  19:</span>       <span style="color: #0000ff">&lt;/</span><span style="color: #800000">endpointBehaviors</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  20:</span>     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">behaviors</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  21:</span>     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">system.serviceModel</span><span style="color: #0000ff">&gt;</span>
</pre>
</div>
</div>

The first service we'll create is to populate the makes dropdown. With WCF we first need to create a contract for the service. Open up the interface IVehicleService and type or copy in the following code. 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> [ServiceContract]
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> IVehicleService
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span> {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>     [OperationContract]
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>     [WebInvoke(Method = <span style="color: #006080">&quot;GET&quot;</span>, BodyStyle = WebMessageBodyStyle.Bare, ResponseFormat = WebMessageFormat.Json, UriTemplate = <span style="color: #006080">&quot;GetMakes&quot;</span>)]
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>     Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; GetMakes();
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span> }
</pre>
</div>
</div>

The things to notice about this code is the WebInvoke attribute. This is where all the work goes on. The first thing we set is the method, as we are wanting RESTful services, we will set this to GET. We could have also used the WebGet attribute here instead of the WebInvoke attribute. The next setting is the BodyStyle property and we set this to WebMessageBodyStyle.Bare. This removes the wrapper around the data. If we were exposes as POX (Plain Old XML) then it would remove the namespace declaration etc. but as we are using JSON, it removes a wrapper object. 

We then set the ResponseFormat to WebMessageFormat.Json which exposes the data as JavaScript instead of the default XML. The last property we set is the UriTemplate. This property denotes the endpoint of our service, which uniquely identifies the method of the service. 

As I mentioned earlier, the return type is of Dictionary&lt;string, string&gt; which allows us to expose just the data required. 

Next we need to implement the interface we&#39;ve just created. Open up the code behind for the service VehicleService.cs which is located in the App_Code folder. Create the following method. 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">public</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; GetMakes()
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span>     {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>         Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; makes = <span style="color: #0000ff">new</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt;();
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>         
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>         <span style="color: #0000ff">using</span> (VehiclesDataContext vdc = <span style="color: #0000ff">new</span> VehiclesDataContext())
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>         {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>             makes = vdc.GetMakes().ToDictionary(m =&gt; m.MakeId.ToString(), m =&gt; m.MakeName);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>         }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>         <span style="color: #0000ff">return</span> makes;
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>     }
</pre>
</div>
</div>

The code above creates a generic dictionary and populates it with the results of the stroed proc. The thing to notice here is the use of Lambda expressions to specify the key and value fields for the toDictionary() extension method. 

Now we can test the first service.&nbsp; Open a browser and navigate to the service endpoint. 

My endpoint is http://localhost/VehicleService.svc/GetMakes 

Depending on your browser (I'm using IE8 beta 1), you will either see some JavaScript on the screen or you'll be asked to save a .js file. A nice little utility for Internet Explorer is <a href="http://www.nikhilk.net/" target="_blank">Nikhil's Web Development Helper</a> Using this tool you can see the JSON results coming back from the service. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image10_thumb.png" alt="JSON View">

Now we have a working service, we can implement the other two services we require. Notice how we pass the parameters through to the services. The UriTemplate uses place holders that get replaced with the passed in params. The reason why we pass the MakeId and ModelId to the GetDerivatives is so that if only the Make dropdown is selected the derivatives dropdown is still filtered. Here is the interface implementation: 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> [OperationContract]
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span>  [WebInvoke(Method = <span style="color: #006080">&quot;GET&quot;</span>, BodyStyle = WebMessageBodyStyle.Bare, ResponseFormat = WebMessageFormat.Json, UriTemplate = <span style="color: #006080">&quot;GetModels/{makeId}&quot;</span>)]
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>  Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; GetModels(<span style="color: #0000ff">string</span> makeId);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>  [OperationContract]
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>  [WebInvoke(Method = <span style="color: #006080">&quot;GET&quot;</span>, BodyStyle = WebMessageBodyStyle.Bare, ResponseFormat = WebMessageFormat.Json, UriTemplate = <span style="color: #006080">&quot;GetDerivatives/{makeId}/{modelId}&quot;</span>)]
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>  Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; GetDerivatives(<span style="color: #0000ff">string</span> makeId, <span style="color: #0000ff">string</span> modelId);
</pre>
</div>
</div>

And the service implementation: 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">public</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; GetModels(<span style="color: #0000ff">string</span> make)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>     Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; models = <span style="color: #0000ff">new</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt;(); 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>     <span style="color: #0000ff">int</span> makeId;
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>     <span style="color: #0000ff">int</span>.TryParse(make, <span style="color: #0000ff">out</span> makeId);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>     <span style="color: #0000ff">using</span> (VehiclesDataContext vdc = <span style="color: #0000ff">new</span> VehiclesDataContext())
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>     {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>         models = vdc.GetModels(makeId).ToDictionary(m =&gt; m.ModelId.ToString(), m =&gt; m.ModelName);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>     }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>     <span style="color: #0000ff">return</span> models;
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span> }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  15:</span> <span style="color: #0000ff">public</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; GetDerivatives(<span style="color: #0000ff">string</span> make, <span style="color: #0000ff">string</span> model)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  16:</span> {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  17:</span>     Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt; derivatives = <span style="color: #0000ff">new</span> Dictionary&lt;<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>&gt;();
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  18:</span>     <span style="color: #0000ff">int</span> makeId, modelId;
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  19:</span>     <span style="color: #0000ff">int</span>.TryParse(make, <span style="color: #0000ff">out</span> makeId);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  20:</span>     <span style="color: #0000ff">int</span>.TryParse(model, <span style="color: #0000ff">out</span> modelId);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  21:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  22:</span>     <span style="color: #0000ff">using</span> (VehiclesDataContext vdc = <span style="color: #0000ff">new</span> VehiclesDataContext())
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  23:</span>     {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  24:</span>         derivatives = vdc.GetDerivatives(makeId, modelId).ToDictionary(d =&gt; d.DerivativeId.ToString(), d =&gt; d.DerivativeName);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  25:</span>     }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  26:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  27:</span>     <span style="color: #0000ff">return</span> derivatives;
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  28:</span> }
</pre>
</div>
</div>

I we were to set our "GetModels" service method to return of type "Model" we would have sent over the "MakeId" with the data, but as we didn't select it in our stored proc then they come through as zeros. This can be seen below. We test our services by navigating directly to them and passing in dummy parameters like so: <a href="http://localhost/CascadingDropdowns/VehicleService.svc/GetModels/1">http://localhost/CascadingDropdowns/VehicleService.svc/GetModels/1</a> 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/image7_thumb.png" alt="JSON Models View">

Now we have all of our server side stuff done, we can move onto the client side jQuery. 

First off, add a new item to our website and choose a JavaScript file, call it JScripts.js. 

We next add the reference to the jQuery library, I've linked to the google hosted version which gives a couple of benefits. 

 - Due to restrictions on most browsers of only two http requests at one time,&nbsp; the file will be download asynchronously due to it being hosted on another domain to the website.
 - If the user has already visited a site that has linked to the same file then they will have it in their cache.


Go to <a href="http://code.google.com/apis/ajaxlibs/documentation/index.html#jquery" title="http://code.google.com/apis/ajaxlibs/documentation/index.html#jquery">http://code.google.com/apis/ajaxlibs/documentation/index.html#jquery</a> to find out more. 

Then download a jQuery plugin that makes it easier to deal with select boxes. <a href="http://www.texotela.co.uk/code/jquery/select/" title="http://www.texotela.co.uk/code/jquery/select/">http://www.texotela.co.uk/code/jquery/select/</a> 

Please note that I've opted for jQuery over ASP.NET AJAX due to it being lightweight and in my opinion more elegant. 

Link to the scripts in our Default.aspx page ensuring to keep the correct order. 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">script</span> <span style="color: #ff0000">type</span><span style="color: #0000ff">=&quot;text/javascript&quot;</span> <span style="color: #ff0000">src</span><span style="color: #0000ff">=&quot;http://ajax.googleapis.com/ajax/libs/jquery/1.2.6/jquery.min.js&quot;</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">script</span><span style="color: #0000ff">&gt;</span>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> &lt;script type=<span style="color: #006080">&quot;text/javascript&quot;</span> src=<span style="color: #006080">&quot;/Scripts/jquery.selectboxes.js&quot;</span>&gt;
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> &lt;/script&gt;
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> &lt;script type=<span style="color: #006080">&quot;text/javascript&quot;</span> src=<span style="color: #006080">&quot;/Scripts/JScripts.js&quot;</span>&gt;
</pre>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">script</span><span style="color: #0000ff">&gt;</span>
</pre>
</div>
</div>
Next add the three dropdowns to the page.&nbsp; These can be normal html elements as we don&#39;t need any server side involvement. 
<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">select</span> <span style="color: #ff0000">id</span><span style="color: #0000ff">=&quot;Makes&quot;</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">select</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">select</span> <span style="color: #ff0000">id</span><span style="color: #0000ff">=&quot;Models&quot;</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">select</span><span style="color: #0000ff">&gt;</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">select</span> <span style="color: #ff0000">id</span><span style="color: #0000ff">=&quot;Derivatives&quot;</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">select</span><span style="color: #0000ff">&gt;</span>
</pre>
</div>
</div>

Now for the JavaScript. Its good practice to namespace your JavaScript so not to override other functions and objects, which is easy to do.&nbsp; I first check and then add a namespace to our JScript.js file. I then create a couple of helper functions to handle errors and make the AJAX call to the services. 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #008000">// Check to see if namespace already exists and create</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> <span style="color: #0000ff">var</span> SteveTaylor = window.SteveTaylor || {};
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>&nbsp; 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span> <span style="color: #008000">// Some helper methods</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span> SteveTaylor.Common = <span style="color: #0000ff">function</span>(){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>     <span style="color: #0000ff">return</span>{
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>         <span style="color: #008000">// Handle errors here</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>         handleError: <span style="color: #0000ff">function</span>(msg){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>             alert(msg);   
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>         },
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>         <span style="color: #008000">// Main call service helper</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>         callService: <span style="color: #0000ff">function</span>(url, method, callback)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>         {
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span>             $.ajax({
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  15:</span>                 url: url,
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  16:</span>                 type: method,
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  17:</span>                 contentType: <span style="color: #006080">&quot;application/json&quot;</span>,
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  18:</span>                 dataType: <span style="color: #006080">&quot;json&quot;</span>, 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  19:</span>                 success: callback,
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  20:</span>                 error: <span style="color: #0000ff">function</span>(xhr, message, ex) 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  21:</span>                     { SteveTaylor.Common.handleError(<span style="color: #006080">&quot;Ajax error:&quot;</span> + message); }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  22:</span>             });
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  23:</span>         }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  24:</span>     }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  25:</span> }();
</pre>
</div>
</div>

We then need to create an init function that hooks up the events and binds once the page is loaded. I start off by creating another singleton object prefixed with the namespace of SteveTaylor. The first two lines in the init function binds an onchange event to the makes and models select boxes. The third line calls another function that populates the makes dropdown. As the makes and models dropdowns have an onchange event, once the makes are populated the models and derivatives automatically follow suit. 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #008000">// This object populates our dropdowns</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> SteveTaylor.VehicleSearch = <span style="color: #0000ff">function</span>(){    
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>     <span style="color: #0000ff">return</span>{
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>    
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>         init: <span style="color: #0000ff">function</span>(resultsPage){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>             <span style="color: #008000">// Bind events</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>             $(<span style="color: #006080">&quot;#Makes&quot;</span>).bind(<span style="color: #006080">&quot;change&quot;</span>, SteveTaylor.VehicleSearch.getModels);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>             $(<span style="color: #006080">&quot;#Models&quot;</span>).bind(<span style="color: #006080">&quot;change&quot;</span>, SteveTaylor.VehicleSearch.getDerivatives);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>             
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>             <span style="color: #008000">// Initialise makes dropdown</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>             SteveTaylor.VehicleSearch.getMakes();
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>         }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>     }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span> }();
</pre>
</div>
</div>

Next add the three functions that will populate the select boxes. Here is the full vehicle search object: 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #008000">// This object populates our dropdowns</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> SteveTaylor.VehicleSearch = <span style="color: #0000ff">function</span>(){    
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>     <span style="color: #0000ff">return</span>{
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>         getMakes: <span style="color: #0000ff">function</span>(){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>              $(<span style="color: #006080">&quot;#Makes&quot;</span>).removeOption(/./).addOption(<span style="color: #006080">&quot;-1&quot;</span>, <span style="color: #006080">&quot;Loading...&quot;</span>);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>             
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>              SteveTaylor.Common.callService(<span style="color: #006080">&quot;VehicleService.svc/GetMakes&quot;</span>, <span style="color: #006080">&quot;GET&quot;</span>, <span style="color: #0000ff">function</span>(result){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>                  
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>                  <span style="color: #0000ff">for</span>(<span style="color: #0000ff">var</span> i=0; i &lt; result.length; i++)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>                     $(<span style="color: #006080">&quot;#Makes&quot;</span>).addOption(result[i].Key, result[i].Value);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>             
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>                  $(<span style="color: #006080">&quot;#Makes&quot;</span>).addOption(<span style="color: #006080">&quot;-1&quot;</span>, <span style="color: #006080">&quot;Any Make&quot;</span>).trigger(<span style="color: #006080">&quot;change&quot;</span>); 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>              });
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span>         },
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  15:</span>         getModels: <span style="color: #0000ff">function</span>(){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  16:</span>             $(<span style="color: #006080">&quot;#Models&quot;</span>).removeOption(/./).addOption(<span style="color: #006080">&quot;-1&quot;</span>, <span style="color: #006080">&quot;Loading...&quot;</span>);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  17:</span>             
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  18:</span>             SteveTaylor.Common.callService(<span style="color: #006080">&quot;VehicleService.svc/GetModels/&quot;</span> + $(<span style="color: #006080">&quot;#Makes&quot;</span>).val(), <span style="color: #006080">&quot;GET&quot;</span>,                <span style="color: #0000ff">function</span>(result){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  19:</span>                     <span style="color: #0000ff">for</span>(<span style="color: #0000ff">var</span> i=0; i &lt; result.length; i++)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  20:</span>                         $(<span style="color: #006080">&quot;#Models&quot;</span>).addOption(result[i].Key, result[i].Value);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  21:</span>                 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  22:</span>                     $(<span style="color: #006080">&quot;#Models&quot;</span>).addOption(<span style="color: #006080">&quot;-1&quot;</span>, <span style="color: #006080">&quot;Any Model&quot;</span>).trigger(<span style="color: #006080">&quot;change&quot;</span>); 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  23:</span>                 });
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  24:</span>         },   
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  25:</span>         getDerivatives: <span style="color: #0000ff">function</span>(){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  26:</span>             $(<span style="color: #006080">&quot;#Derivatives&quot;</span>).removeOption(/./).addOption(<span style="color: #006080">&quot;-1&quot;</span>, <span style="color: #006080">&quot;Loading...&quot;</span>);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  27:</span>             
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  28:</span>             SteveTaylor.Common.callService(<span style="color: #006080">&quot;VehicleService.svc/GetDerivatives/&quot;</span> + $(<span style="color: #006080">&quot;#Makes&quot;</span>).val() + <span style="color: #006080">&quot;/&quot;</span> + $(<span style="color: #006080">&quot;#Models&quot;</span>).val(), <span style="color: #006080">&quot;GET&quot;</span>,
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  29:</span>                 <span style="color: #0000ff">function</span>(result){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  30:</span>                     <span style="color: #0000ff">for</span>(<span style="color: #0000ff">var</span> i=0; i &lt; result.length; i++)
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  31:</span>                         $(<span style="color: #006080">&quot;#Derivatives&quot;</span>).addOption(result[i].Key, result[i].Value);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  32:</span>                 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  33:</span>                      $(<span style="color: #006080">&quot;#Derivatives&quot;</span>).addOption(<span style="color: #006080">&quot;-1&quot;</span>, <span style="color: #006080">&quot;Any Derivative&quot;</span>).trigger(<span style="color: #006080">&quot;change&quot;</span>); 
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  34:</span>                 });
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  35:</span>         },    
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  36:</span>         init: <span style="color: #0000ff">function</span>(resultsPage){
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  37:</span>             <span style="color: #008000">// Bind events</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  38:</span>             $(<span style="color: #006080">&quot;#Makes&quot;</span>).bind(<span style="color: #006080">&quot;change&quot;</span>, SteveTaylor.VehicleSearch.getModels);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  39:</span>             $(<span style="color: #006080">&quot;#Models&quot;</span>).bind(<span style="color: #006080">&quot;change&quot;</span>, SteveTaylor.VehicleSearch.getDerivatives);
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  40:</span>             
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  41:</span>             <span style="color: #008000">// Initialise makes dropdown</span>
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  42:</span>             SteveTaylor.VehicleSearch.getMakes();
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  43:</span>         }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<span style="color: #606060">  44:</span>     }
</pre>
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">  45:</span> }();
</pre>
</div>
</div>

You will notice from the above code that the three functions are very similar and could have even been abstracted out a bit more. Lets analyse the getMakes function first. 

The first line uses the removeOption functions of the plugin and passes in a regex to remove all items in the dropdown. This is necessary due the caching that the plugin implements. Then utilising jQuery's chaining, we add a temporary option that shows the user that data is loading. We then call the service and pass in the URI, the method and a callback function. Note we pass in an anonymous function that receives the results and loops through the key value pairs and adds them to the dropdown. We then add the default "Any Make" option that has the -1 value to the list. 

The other two functions are almost identical, the only difference being we add the values of the other dropdowns to the URI to pass them into the service. 

The last thing we do is call the init function once the DOM is ready. 

<div style="font-size: 8pt; margin: 20px 0px 10px; overflow: auto; width: 97.5%; cursor: text; max-height: 200px; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border: gray 1px solid; padding: 4px">
<div style="font-size: 8pt; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: #f4f4f4; border-style: none; padding: 0px">
<pre style="font-size: 8pt; margin: 0em; overflow: visible; width: 100%; color: black; line-height: 12pt; font-family: consolas, 'Courier New', courier, monospace; background-color: white; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> $(document).ready(SteveTaylor.VehicleSearch.init);
</pre>
</div>
</div>
<div id="scid:18d43e01-4549-4fde-8ca6-c7b4b7385fac:12871cf1-d7b1-4e0e-9298-2b194bb04250" class="wlWriterSmartContent" style="margin: 0px; display: inline; padding: 0px">
<p>
Download Solution - <a href="/assets/downloads/CascadingDropdowns.zip">CascadingDropdowns.zip</a> 
</p>
</div>

Hope it all makes sense and let me know how you get on. 
