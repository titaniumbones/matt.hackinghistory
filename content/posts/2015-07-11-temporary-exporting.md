---
title: Exporting org-files to a temporary location
author: matt
type: post
date: 2015-07-11T16:25:00+00:00
url: /2015/07/11/temporary-exporting/
dsq_thread_id:
  - "4316962072"
categories:
  - emacs
  - emacs-init

---
I have a private journal, which lives in an encrypted file in a Dropbox-backed-up directory. I use html export to examine the contents sometimes &#x2013; there are some big old tables that are hard to read in org-mode &#x2013; but I don&rsquo;t want the html file to end up in Dropbox. 

So I just copied the definition of org-export-html-as-html and made trivial modifications. There&rsquo;s probably a better way to do this. 

<div class="org-src-container">
  <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">export html to tmp dir</span>
(<span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-html-to-tmp</span>
    (<span style="color: #228b22;">&optional</span> async subtreep visible-only body-only ext-plist)
  <span style="color: #8b2252;">"Export current buffer to a HTML file in the tmp directory.</span>

<span style="color: #8b2252;">If narrowing is active in the current buffer, only export its</span>
<span style="color: #8b2252;">narrowed part.</span>

<span style="color: #8b2252;">If a region is active, export that region.</span>

<span style="color: #8b2252;">A non-nil optional argument ASYNC means the process should happen</span>
<span style="color: #8b2252;">asynchronously.  The resulting file should be accessible through</span>
<span style="color: #8b2252;">the `</span><span style="color: #008b8b;">org-export-stack</span><span style="color: #8b2252;">' interface.</span>

<span style="color: #8b2252;">When optional argument SUBTREEP is non-nil, export the sub-tree</span>
<span style="color: #8b2252;">at point, extracting information from the headline properties</span>
<span style="color: #8b2252;">first.</span>

<span style="color: #8b2252;">When optional argument VISIBLE-ONLY is non-nil, don't export</span>
<span style="color: #8b2252;">contents of hidden elements.</span>

<span style="color: #8b2252;">When optional argument BODY-ONLY is non-nil, only write code</span>
<span style="color: #8b2252;">between \"&lt;body&gt;\" and \"&lt;/body&gt;\" tags.</span>


<span style="color: #8b2252;">EXT-PLIST, when provided, is a property list with external</span>
<span style="color: #8b2252;">parameters overriding Org default settings, but still inferior to</span>
<span style="color: #8b2252;">file-local settings.</span>

<span style="color: #8b2252;">Return output file's name."</span>
  (<span style="color: #a020f0;">interactive</span>)
  (<span style="color: #a020f0;">let*</span> ((extension (concat <span style="color: #8b2252;">"."</span> (<span style="color: #a020f0;">or</span> (plist-get ext-plist <span style="color: #483d8b;">:html-extension</span>)
                                    org-html-extension
                                    <span style="color: #8b2252;">"html"</span>)))
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this is the code I've changed from the original function. </span>
         (file (org-export-output-file-name extension subtreep <span style="color: #8b2252;">"/home/matt/tmp/"</span>))

         (org-export-coding-system org-html-coding-system))
    (org-export-to-file 'html file
      async subtreep visible-only body-only ext-plist)
    (org-open-file file)))

(org-defkey org-mode-map
            (kbd <span style="color: #8b2252;">"C-c 0"</span>) 'mwp-org-html-to-tmp)
</pre>
</div>