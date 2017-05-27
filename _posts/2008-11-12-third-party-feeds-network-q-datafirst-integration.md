---
title: "Third Party Feeds: Network Q/Datafirst Integration"
tags:
  - .Net
  - Automotive
  - Online Marketing
  - Third Party Feeds
---

Network Q is the used car brand of Vauxhall that offers the consumer piece of mind when buying a used car. Vauxhall, a UK based marque owned by General Motors (GM) allows Vauxhall dealers to list their Network Q qualifying vehicles on their <a href="http://www.networkq.co.uk" target="_blank" title="Network Q website">website</a>. In my previous post: <a href="/third-party-stock-inventory-data-feeds" title="Third Party Stock/Inventory Data Feeds">Third Party Stock/Inventory Data Feeds</a> I talked about how important and how much choice there is now when displaying your vehicles online, with the Network Q website, its free for dealers to list their vehicles. This integration saves the dealer manually uploading the vehicles one by one though the admin area and can be fully automated by taking the stock from their DMS (Dealer Management System) such as Kerridge/ADP, Pinnacle etc. 

The vehicles need to undergo a set of multi point checks and meet some other requirements such as age and mileage. 

Vauxhall use a company called <a href="http://www.datafirst.fr/" target="_blank">Datafirst</a> who are based in France to manage the Network Q website. Datafirst offer a webservice API (Application Programming Interface) for you to upload the stock. Unlike most third party websites that accept feeds the API the Datafirst provide allows the updates to be made in real time. From a developer's perspective this is far better to work with than CSV (Comma Separated Values) file. The biggest problem with CSV files is that they normally get transferred via FTP (File Transfer Protocol) which makes it difficult to determine whether the transfer succeeded or not. 

The Datafirst API is session based and a number of calls need to be made in a particular order. 

The main decision you need to make is how the webservice gets called. Normally I would have built an SSIS (SQL Server Integration Services) package to deal with this by utilising the webservice and xml tasks. But due to a number of reasons, such as deployment etc. I opted for a simple console application that is run from Windows scheduled task manager. 

The 4 steps required are: 

 1. Initialise Session
 2. Send Vehicle Data
 3. Send Vehicle Photos 
 4. Finalise Session

Something I recommend is that you put good error checking at every stage plus a catch all to catch unhandled errors. Is not uncommon to find that Datafirst's service is unavailable from time to time. The error checking will alert you by email so you can rerun or notify someone of the failure. 

I do this by adding the following piece of code: 

<div style="line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: consolas, 'Courier New', courier, monospace; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: gray 1px solid; padding: 4px">
<div style="line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> AppDomain currentDomain = AppDomain.CurrentDomain;
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> currentDomain.UnhandledException += <span style="color: #0000ff">new</span> UnhandledExceptionEventHandler(UnhandledExceptionHandler);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> UnhandledExceptionHandler(<span style="color: #0000ff">object</span> sender, UnhandledExceptionEventArgs args)
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>         {
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>             StringBuilder sb = <span style="color: #0000ff">new</span> StringBuilder();
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>             
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>             Exception ex = ((Exception)(args.ExceptionObject));
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>             sb.Append(<span style="color: #006080">&quot;****** Unhandled Exception ******&lt;br/&gt;&lt;br/&gt;&quot;</span>);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>             sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;ExceptionType: &quot;</span> + ex.GetType().FullName);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>             sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;HelpLine: &quot;</span> + ex.HelpLink);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>             sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;Message: &quot;</span> + ex.Message);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>             sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;Source: &quot;</span> + ex.Source);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span>             sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;StackTrace: &quot;</span> + ex.StackTrace);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  15:</span>             sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;TargetSite: &quot;</span> + ex.TargetSite);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  16:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  17:</span>             Exception ie = ex;
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  18:</span>             <span style="color: #0000ff">while</span> (!((ex.InnerException == <span style="color: #0000ff">null</span>)))
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  19:</span>             {
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  20:</span>                 ie = ie.InnerException;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  21:</span>                 sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;&lt;br/&gt;****** Inner Exception ******&lt;br/&gt;&lt;br/&gt;&quot;</span>);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  22:</span>                 sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;ExceptionType: &quot;</span> + ie.GetType().Name);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  23:</span>                 sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;HelpLine: &quot;</span> + ie.HelpLink);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  24:</span>                 sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;Message: &quot;</span> + ie.Message);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  25:</span>                 sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;Source: &quot;</span> + ie.Source);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  26:</span>                 sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;StackTrace: &quot;</span> + ie.StackTrace);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  27:</span>                 sb.Append(<span style="color: #006080">&quot;&lt;br/&gt;TargetSite: &quot;</span> + ie.TargetSite);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  28:</span>             }
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  29:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  30:</span>             Utilities.SendEmail(Utilities.Preferences.ErrorEmails, sb.ToString());
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  31:</span>         }
</pre>
</div>
</div>
<p>
The next thing to do is set up the objects we need to pass over.&nbsp; First off we create a base class with only one method that serialises the object. 
</p>
<div style="line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: consolas, 'Courier New', courier, monospace; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: gray 1px solid; padding: 4px">
<div style="line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">abstract</span> <span style="color: #0000ff">class</span> RequestBase
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span>     {
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>         <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> SerialiseToString()
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>         {
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>             XmlSerializer xs = <span style="color: #0000ff">new</span> XmlSerializer(<span style="color: #0000ff">this</span>.GetType());
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>             XmlWriterSettings xws = <span style="color: #0000ff">new</span> XmlWriterSettings();
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>             xws.Encoding = <span style="color: #0000ff">new</span> UTF8Encoding(<span style="color: #0000ff">false</span>);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>             <span style="color: #0000ff">string</span> xml = <span style="color: #0000ff">string</span>.Empty;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>             <span style="color: #0000ff">using</span> (MemoryStream ms = <span style="color: #0000ff">new</span> MemoryStream())
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>             {
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>                 <span style="color: #0000ff">using</span> (XmlWriter xw = XmlWriter.Create(ms, xws))
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>                 {
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span>                     xs.Serialize(xw, <span style="color: #0000ff">this</span>);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  15:</span>                     xw.Close();
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  16:</span>                 }
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  17:</span>                 xml = Encoding.UTF8.GetString(ms.ToArray());
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  18:</span>             }
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  19:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  20:</span>             <span style="color: #0000ff">return</span> xml;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  21:</span>         }
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  22:</span>     }
</pre>
</div>
</div>
<p>
&nbsp;
</p>
<p>
Then the objects, here is the vehicle request object that takes a collection of vehicles. 
</p>
<div style="line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: consolas, 'Courier New', courier, monospace; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: gray 1px solid; padding: 4px">
<div style="line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> [Serializable]
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> [XmlRoot(<span style="color: #006080">&quot;REQUEST&quot;</span>)]
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> VehicleRequest : RequestBase
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span> {
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>     [XmlAttribute(<span style="color: #006080">&quot;NAME&quot;</span>)]
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>     <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> NAME { get; set; }
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>     [XmlAttribute(<span style="color: #006080">&quot;SESSID&quot;</span>)]
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>     <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> SESSID { get; set; }
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>     [XmlArrayAttribute(<span style="color: #006080">&quot;UVSTOCK&quot;</span>)]
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>     [XmlArrayItemAttribute(<span style="color: #006080">&quot;USEDVEH&quot;</span>)]
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>     <span style="color: #0000ff">public</span> List&lt;Vehicle&gt; Vehicles { get; set; }  
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span> }
</pre>
</div>
</div>
<p>
Once all the objects are specified and marked-up with the relevant attributes we need to send them over to the webservice.&nbsp; Now, normally it would be easy to create a proxy class and call the webservice direct from code, but the way that Datafirst work is just to receive POX (Plain Old Xml) via HTTP requests and responses.&nbsp; To send the data then we need to us the HttpWebRequest from the System.Net namespace like so: 
</p>
<div style="line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: consolas, 'Courier New', courier, monospace; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: gray 1px solid; padding: 4px">
<div style="line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">string</span> PostRequest(<span style="color: #0000ff">string</span> xml, <span style="color: #0000ff">string</span> uri)
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> {
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>     Uri address = <span style="color: #0000ff">new</span> Uri(uri);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>     <span style="color: #0000ff">string</span> responseXml = <span style="color: #0000ff">string</span>.Empty;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>     HttpWebRequest request = WebRequest.Create(address) <span style="color: #0000ff">as</span> HttpWebRequest;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>     request.Method = <span style="color: #006080">&quot;POST&quot;</span>;
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>     request.ContentType = <span style="color: #006080">&quot;text/xml&quot;</span>;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>     <span style="color: #0000ff">byte</span>[] byteData = UTF8Encoding.UTF8.GetBytes(xml.ToString());
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>     request.ContentLength = byteData.Length;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span>  
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  14:</span>     <span style="color: #0000ff">using</span> (Stream postStream = request.GetRequestStream())
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  15:</span>     {
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  16:</span>         postStream.Write(byteData, 0, byteData.Length);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  17:</span>     }
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  18:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  19:</span>     <span style="color: #008000">// Get response   </span>
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  20:</span>     <span style="color: #0000ff">using</span> (HttpWebResponse response = request.GetResponse() <span style="color: #0000ff">as</span> HttpWebResponse)
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  21:</span>     {
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  22:</span>         StreamReader reader = <span style="color: #0000ff">new</span> StreamReader(response.GetResponseStream());
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  23:</span>         responseXml = reader.ReadToEnd();
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  24:</span>     }
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  25:</span>     UpdateSession(responseXml);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  26:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  27:</span>     <span style="color: #0000ff">return</span> responseXml;
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  28:</span> }
</pre>
</div>
</div>
<p>
Now you might be wondering how do the images get transmitted.&nbsp; Well, we need to encode the images in base64. 
</p>
<div style="line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: consolas, 'Courier New', courier, monospace; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: gray 1px solid; padding: 4px">
<div style="line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   1:</span> <span style="color: #0000ff">internal</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">string</span> ConvertImageToBase64(<span style="color: #0000ff">string</span> path)
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   2:</span> {
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   3:</span>     <span style="color: #0000ff">byte</span>[] byteArray = <span style="color: #0000ff">null</span>;
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   4:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   5:</span>     <span style="color: #0000ff">using</span>(FileStream fs = <span style="color: #0000ff">new</span> FileStream(path, FileMode.Open, FileAccess.Read))
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   6:</span>     {
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   7:</span>         byteArray = <span style="color: #0000ff">new</span> <span style="color: #0000ff">byte</span>[fs.Length];
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   8:</span>         fs.Position = 0;
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">   9:</span>         fs.Read(byteArray, 0, (<span style="color: #0000ff">int</span>)fs.Length);
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  10:</span>     }
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  11:</span>&nbsp; 
</pre>
<pre style="line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  12:</span>     <span style="color: #0000ff">return</span> Convert.ToBase64String(byteArray);
</pre>
<pre style="line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: consolas, 'Courier New', courier, monospace; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px">
<span style="color: #606060">  13:</span> }
</pre>
</div>
</div>


It's recommended due to the amount of vehicle images that can be transferred to only send over modified or new images, so you'll need to update the database with a sent flag or similar. Alternatively, what I did that works well is to set the creation time of image to some time in the past and the next time you look for the images only get images without that date.&nbsp; This is extremely useful if this application was to be deployed to a computer within a dealership or you don&rsquo;t want to mess around with databases. 

So there you have it. Not difficult but different to how other people do it, and in my opinion a lot better then the CSV/FTP model I tend to come across. 
  