<h1>KissMetrics in Chrome Extensions wi/kiss-include.js</h1>

<p>To use KissMetrics in Chrome Extensions, we made a script that magically patches the JavaScript api for use everywhere within Chrome Extensions. You simply include it in your background page and content scripts, and call <code>_kmq.push</code> like normal.</p>

Download the patch script right from this repository, or do <code>npm install kiss-include</code>

<h3>Important points</h3>
<ul>
  <li>
    To use in <strong>content scripts</strong>, you will also 
    need to include it in the background page.
  </li>
</ul>

<h3>Initialize</h3>
<ul>
  <li>
    <h4>Asynchronously (recommended)</h4>
    <p>
      Setup this code, and being 
      <code>push</code>'ing to <code>_kmq:</code>
    </p><pre><code>  if (typeof _kmq === 'undefined') {
    window._kmq = [];
  }
  window._kmk = 'c168ad9f6287ggbcfe92a883fc3c8c0f904e7972';</code></pre>
    <p>
      At some later point, be sure to include kiss-include.js.
      It will detect your _kmk id, and set itself up.
    </p>
  </li>
  <li>
    <h4>Directly</h4>
    <p>Include kiss-include.js before this code in your extension, and then call <code>kissInclude</code> with your <code>_kmk</code> account id:</p>
    <pre><code>  kissInclude('ioheiorfhaksjnaoiewhoasdhjf');</code></pre>
  </li>
  <li>
    <h4>Setting a domain</h4>
    <p>
      As an extension, you might want to manually
      specify a domain like so:
    </p>
    <pre><code>  window.KM_COOKIE_DOMAIN = 'www.mydomain.com';</code></pre>
    <p>This code needs to be placed before the library is initialized.</p>  
  </li>
</ul>    

<br>
<br>
<strong>Extra details on the script</strong>
<p>Wherever you include the script, it detects it's environment and responds accordingly. This script alleviates the headache associated with chrome's isolated world's - you just use the JS API as defined in the docs.</p>
<strong>Including in content scripts</strong>
<p>To work in content scripts, you need to make sure it's also included in the background page</p>
<pre><code>  //in you manifest.json:
  "content_scripts": [
     {
        "js": [ "YourScripts.js", "kiss-include.js" ],
        "matches": [ "&lt;all_urls&gt;" ],
        "run_at": "document_start"
     },
  ],
  </code></pre>


<strong>Including in background page:</strong>
<pre><code>  //in your background.html:
  &lt;script src="kiss-include.js"&gt;&lt;/script&gt;
  </code>
</pre>

<p>If you include kiss-include.js after your code, you need to use asynchronous initialization. If you include it before your code, you'll need to use direct initialization.</p>

<strong></strong>
<p>This also includes a convenience <code>event</code> method on <code>_kmq.push</code>. Instead of <code>_kmq.push(['record', 'something']);</code> use: <code>event('record', 'something');</code>, but if you want to just record something, one argument defaults to 'record' so you can go: <code>event('something');</code> </p>
