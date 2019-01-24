# Getting Started with a Local Server

1. Use Terminal (Mac) or Command Prompt (PC), which both represent the command line, and you'll want to go to the folder you added the files to.
	* For both Macs and PCs, you'll use the ```cd``` command 
2. Once there, start a local server 
	* For Macs, you can simply run: ``` python -m http.server 8000 ``` if using python 3; ``` python -m SimpleHTTPServer 8000```\* if using python 2
	* For PCs, it's a bit more complicated. You'll need to either download python or XAMPP. You can checkout [this page](https://www.apachefriends.org/index.html)
3. Go to your browser (Chrome is recommended), and go to this URL ```localhost:8000```\*

\*Can substitute out `8000` for any other number.

#### Note on Local Servers: 

In some cases, you can view local HTML files directly in your web browser. However, some browsers have restrictions that prevent them from loading local files via JavaScript, for security reasons. That means if your D3 code is trying to pull in any external datafiles (like CSVs or JSON), it will fail with no good explanation. This isn’t D3’s fault; it’s a browser feature that prevents loading of scripts and other external files from third-party, untrusted websites.

For this reason, it is much more reliable to load your page via a web server. Although you could use a remote web server, it is much, much faster to store and host everything locally (meaning, on the same computer, the one right in front of you). It is a strange idea, to use your local computer to host and serve files to itself, but you can think about it as the different programs talking to each other: the browser program requests files from the server program, which responds by serving them back.
