---


---

<h1 id="dns-resolve">DNS Resolve</h1>
<p>When a device tries to reach another through a network, it needs to know its target address, the IP address. Remembering every IP address is troublesome by itself, but it goes beyond that when load balancers are in place or an IP address for a feature is changed. To make it easy for communication, there are domain names, featuring ways to refer to an IP address through a kind of synonym.</p>
<p>The problem is, devices need IP Addresses. Domain names hold no value by itself for them. To translate a name into an IP Address is the job of special servers known as <strong>DNS</strong>, but you can provide custom names for your device to read from before addressing the request to a DNS.</p>
<h2 id="resolving-locally">Resolving Locally</h2>
<p>Let’s say you have a printer connected to your network, with a user interface provided on its IP Address. you don’t want to type in 0.0.200.201 every time you need to access the printer, so you’d like to create a name so when you type http://job_printer, your browser converts to the printer IP Address.</p>
<p>To solve it, you may make a change directly into your device’s <strong>/etc/hosts</strong> file and add a new custom name. That should be done as <strong>sudo</strong> using your text editor of choice, like so:</p>
<pre><code>$ sudo vi /etc/hosts
</code></pre>
<p>Then create a new definition, appending to the existing ones:</p>
<pre><code># IP Domain sorthost
0.0.200.201 job_printer
</code></pre>
<p>Save and quit the configuration file.<br>
Now you have to assure your device is using its hosts before redirecting to a <strong>DNS</strong>. Open the <strong>/etc/nsswitch.conf</strong> file using <strong>grep</strong>:</p>
<pre><code>$ grep host /etc/nsswitch.conf
</code></pre>
<p>if the result should look like <code>hosts: files dns</code>, displaying the order in which the device resolves the domain names. The device will try to resolve the name using the resources from left to right, so what we want is to have <strong>files</strong> coming before <strong>dns</strong>.</p>
<p>You may test your new custom domain name with ping. <strong>Do not</strong> use nslookup nor host as those bypasses the configurations made at /etc/hosts:</p>
<pre><code>$ ping -c 1 job_printer 
</code></pre>
<p>You should get as a result the following line:<br>
<code>PING job_printer (0.0.200.201) XX(YY) bytes of data.</code></p>
<p>All done, your small enterprise can now look into the created synonym list for IPs.</p>
<h2 id="resolving-dns-inside-a-small-network">Resolving DNS inside a small network</h2>
<p>The above solution is great if there are few devices that use said names, but maintain it is really crumble some if you have to edit each device from a local network every time an IP address is changed. In those cases, you can have one device working as DNS Resolver on your network, having your network router redirect domain names requests to it before addressing to a proper DNS.<br>
Using a DNS Resolver with a cache option can also improve communication speed as it stores recently used conversions and reuses those cached results instead of making new requests.<br>
Note that a DNS Resolver should not be open to any address, as it is a security flaw that may lead to DNS attack. A tablespace names list or a plain list of authorized IP address should be used to grant access to the DNS Resolver.</p>
<h3 id="setting-up-a-dns-resolver">Setting up a DNS Resolver</h3>
<p>How to set a dedicated DNS Resolver for your network is beyond the scope of this article. But if you believe this is more what you are looking for, here are some tips on how to learn more about it.</p>
<p>There are lots of solutions out there and you should explore them. You can check this amazing tutorial on how to set up bind9, one of the options out there: <a href="https://www.perfacilis.com/blog/systeembeheer/linux/setup-a-public-dns-server.html">https://www.perfacilis.com/blog/systeembeheer/linux/setup-a-public-dns-server.html</a>.</p>
<p>To configure your network router, you will need to look into its manual for how to set up its DNS. If you don’t have the manual at hand, try to search it online. Most are featured as a free resource on its website or support forum.</p>

