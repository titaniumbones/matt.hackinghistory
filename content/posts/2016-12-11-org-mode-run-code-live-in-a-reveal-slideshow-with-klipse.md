---
title: 'Org-Mode: Run Code Live in a Reveal slideshow with Klipse'
author: matt
type: post
date: 2016-12-11T11:41:00+00:00
url: /2016/12/11/org-mode-run-code-live-in-a-reveal-slideshow-with-klipse/
dsq_thread_id:
  - "5371868933"
categories:
  - emacs

---
<nav id="table-of-contents"> 

## Table of Contents

<div id="text-table-of-contents">
  <ul>
    <li>
      <a href="#org7713ada">The Basics: ox-reveal</a>
    </li>
    <li>
      <a href="#org80f4b5b">Running Code in Klipse</a> <ul>
        <li>
          <a href="#org45d7e1d">Add Support for Third-Party Plugins</a>
        </li>
        <li>
          <a href="#org35a8a7f">Rewrite org-reveal-src-block</a>
        </li>
      </ul>
    </li>
  </ul>
</div></nav> 

Slide-based presentations are pretty useful and in any case _de rigeur_ for contemporary lecturing, but it can be a REAL pain to switch form a slideshow to a code editor and then back again to a web browser to show the results of the code&#x2026; Much better would be to have all the code embedded directly in your presentation. This also makes it easier to check over your code when you&rsquo;re rewriting your lectures from year to year, and also helps with literate programming. 

It turns out that there are actually several solutions for doing this with Org. They are: 

<ul class="org-ul">
  <li>
    directly embed JSBin in your slideshow
  </li>
  <li>
    Use RevealEditor for HTML, CSS, and JS that you want to render in a web page.
  </li>
  <li>
    Use the new klipse plugin for code that you want to <i>evaluate</i> (rather than render)
  </li>
</ul>

Each method has advantages and disadvantages, and each requires a certain amount of setup in your config. In this post I&rsquo;m talking about using Klipse because I&rsquo;ve bene able to make it actually work! 

<div id="outline-container-org7713ada" class="outline-2">
  <h2 id="org7713ada">
    The Basics: ox-reveal
  </h2>
  
  <div class="outline-text-2" id="text-org7713ada">
    <p>
      To start with, you will need to install <a href="https://github.com/yjwen/org-reveal#requirements-and-installation">org-reveal</a> as well as its dependencies (org-mode and reveal.js). I install <code>org-reveal</code> from github instead of from ELPA, in part because I need to make modifications to both it and <code>reveal.js</code>.
    </p>
    
    <p>
      Follow the detailed instructions until you have a working export to reveal. Make sure that you have activated the &ldquo;highlight&rdquo; plugin in <code>org-reveal-plugins</code>, which you can do with <code>M-x customize-variable org-reveal-plugins</code>. None of the tricks described below will work without that plugin!
    </p>
  </div>
</div>

<div id="outline-container-org80f4b5b" class="outline-2">
  <h2 id="org80f4b5b">
    Running Code in Klipse
  </h2>
  
  <div class="outline-text-2" id="text-org80f4b5b">
    <p>
      <a href="https://github.com/viebel/klipse">Klipse</a> is a web repl that supports a handful of languages, and can be extended to support more. Code is embedded in a CodeMirror instance and evaluated as you type (pretty cool).
    </p>
    
    <p>
      Unfortunately CodeMirror and Reveal don&rsquo;t get along very well so it&rsquo;s not completely straightforward to embed Klipse directly into a reveal.js slideshow. The workaround is to replace a code-snippet tag with an <code>iframe</code> element containing the code in a form that klipse can read. Klipse&rsquo;s author @viebel has kindly provided an example of how to do this <a href="http://viebel.github.io/klipse/examples/klipse_reveal.js">here</a>.
    </p>
    
    <p>
      The next trick is to set up org-reveal to use this snippet. After messing around for a bit it became obvious that org-mode&rsquo;s standard <a href="http://orgmode.org/worg/exporters/filter-markup.html">Export filters</a> were going to be pretty hard to use, so I decided to rewrite some of <code>org-reveal</code>&rsquo;s functionality:
    </p>
  </div>
  
  <div id="outline-container-org45d7e1d" class="outline-3">
    <h3 id="org45d7e1d">
      Add Support for Third-Party Plugins
    </h3>
    
    <div class="outline-text-3" id="text-org45d7e1d">
      <p>
        Org-reveal has excellent support for reveal.js&rsquo;s &ldquo;build-in&rdquo; plugins, but using third-party plugins requires you to use the <code>or-reveal~postamble</code> variable~, which didn&rsquo;t seem right to me. So I <a href="https://github.com/yjwen/org-reveal/pull/256">added a defcustom</a> and a few small generic tricks to support loading third-party plugins. Then I downloaded @viebel&rsquo;s <code>klipse_reveal.js</code> script and saved it to the <code>plugin</code> folder of my <code>reveal.js</code> installation. Finally, I set org-reveal-external-plugins to <code>((klipse . "{src: '%splugin/klipse_reveal.js'}"))</code> using the customize-variable interface.
      </p>
    </div>
  </div>
  
  <div id="outline-container-org35a8a7f" class="outline-3">
    <h3 id="org35a8a7f">
      Rewrite org-reveal-src-block
    </h3>
    
    <div class="outline-text-3" id="text-org35a8a7f">
      <p>
        At this point, the plugin loads, but it couldn&rsquo;t find the exported src blocks. As I mentioned above, I was unable to find a satisfactory solution using export filters, so in the end I just rewrote the function <code>org-reveal-src-block</code>, which ox-reveal uses to process src blocks. I decided to keep the original <code>&lt;pre&gt;&lt;code&gt;</code> syntax and simply add on an additional <code>&lt;klipse-snippet</code> block, using the <code>display:none</code> CSS property for the old code block. This ensures compatibility with the reveal-editor solution (which isn&rsquo;t currently working for me, but offers some other features.
      </p>
      
      <p>
        Here&rsquo;s my modified function:
      </p>
      
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">defun</span> <span style="color: #87cefa;">org-reveal-src-block</span> <span style="color: #8c8c8c;">(</span>src-block contents info<span style="color: #8c8c8c;">)</span>
  <span style="color: #ffa07a;">"Transcode a SRC-BLOCK element from Org to Reveal.</span>
<span style="color: #ffa07a;">CONTENTS holds the contents of the item.  INFO is a plist holding</span>
<span style="color: #ffa07a;">contextual information."</span>
  <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> <span style="color: #8c8c8c;">(</span>org-export-read-attribute <span style="color: #b0c4de;">:attr_html</span> src-block <span style="color: #b0c4de;">:textarea</span><span style="color: #8c8c8c;">)</span>
      <span style="color: #8c8c8c;">(</span>org-html--textarea-block src-block<span style="color: #8c8c8c;">)</span>
    <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">let*</span> <span style="color: #8c8c8c;">((</span>use-highlight <span style="color: #8c8c8c;">(</span>org-reveal--using-highlight.js info<span style="color: #8c8c8c;">))</span>
           <span style="color: #8c8c8c;">(</span>lang <span style="color: #8c8c8c;">(</span>org-element-property <span style="color: #b0c4de;">:language</span> src-block<span style="color: #8c8c8c;">))</span>
           <span style="color: #8c8c8c;">(</span>caption <span style="color: #8c8c8c;">(</span>org-export-get-caption src-block<span style="color: #8c8c8c;">))</span>
           <span style="color: #8c8c8c;">(</span>code <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> <span style="color: #8c8c8c;">(</span>not use-highlight<span style="color: #8c8c8c;">)</span>
                     <span style="color: #8c8c8c;">(</span>org-html-format-code src-block info<span style="color: #8c8c8c;">)</span>
                   <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">cl-letf</span> <span style="color: #8c8c8c;">(((</span>symbol-function 'org-html-htmlize-region-for-paste<span style="color: #8c8c8c;">)</span>
                              #'buffer-substring<span style="color: #8c8c8c;">))</span>
                     <span style="color: #8c8c8c;">(</span>org-html-format-code src-block info<span style="color: #8c8c8c;">))))</span>
           <span style="color: #8c8c8c;">(</span>frag <span style="color: #8c8c8c;">(</span>org-export-read-attribute <span style="color: #b0c4de;">:attr_reveal</span> src-block <span style="color: #b0c4de;">:frag</span><span style="color: #8c8c8c;">))</span>
           <span style="color: #8c8c8c;">(</span>code-attribs <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">or</span> <span style="color: #8c8c8c;">(</span>org-export-read-attribute
                         <span style="color: #b0c4de;">:attr_reveal</span> src-block <span style="color: #b0c4de;">:code_attribs</span><span style="color: #8c8c8c;">)</span> <span style="color: #ffa07a;">""</span><span style="color: #8c8c8c;">))</span>
           <span style="color: #8c8c8c;">(</span>label <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">let</span> <span style="color: #8c8c8c;">((</span>lbl <span style="color: #8c8c8c;">(</span>org-element-property <span style="color: #b0c4de;">:name</span> src-block<span style="color: #8c8c8c;">)))</span>
                    <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> <span style="color: #8c8c8c;">(</span>not lbl<span style="color: #8c8c8c;">)</span> <span style="color: #ffa07a;">""</span>
                      <span style="color: #8c8c8c;">(</span>format <span style="color: #ffa07a;">" id=\"%s\""</span> lbl<span style="color: #8c8c8c;">))))</span>
           <span style="color: #8c8c8c;">(</span>klipsify  <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">and</span> <span style="color: #8c8c8c;">(</span>assoc 'klipse org-reveal-external-plugins<span style="color: #8c8c8c;">)</span>
                           <span style="color: #8c8c8c;">(</span>member lang '<span style="color: #8c8c8c;">(</span><span style="color: #ffa07a;">"javascript"</span> <span style="color: #ffa07a;">"ruby"</span> <span style="color: #ffa07a;">"scheme"</span> <span style="color: #ffa07a;">"clojure"</span> <span style="color: #ffa07a;">"php"</span><span style="color: #8c8c8c;">))))</span>
                      <span style="color: #8c8c8c;">)</span>
      <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> <span style="color: #8c8c8c;">(</span>not lang<span style="color: #8c8c8c;">)</span>
          <span style="color: #8c8c8c;">(</span>format <span style="color: #ffa07a;">"&lt;pre %s%s&gt;\n%s&lt;/pre&gt;"</span>
                  <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">or</span> <span style="color: #8c8c8c;">(</span>frag-class frag info<span style="color: #8c8c8c;">)</span> <span style="color: #ffa07a;">" class=\"example\""</span><span style="color: #8c8c8c;">)</span>
                  label
                  code<span style="color: #8c8c8c;">)</span>
        <span style="color: #8c8c8c;">(</span>format
         <span style="color: #ffa07a;">"&lt;div %sclass=\"org-src-container\"&gt;\n%s%s\n&lt;/div&gt;%s"</span>
         <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> klipsify <span style="color: #ffa07a;">"style=\"display:none;\" "</span> <span style="color: #ffa07a;">""</span>)
         <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> <span style="color: #8c8c8c;">(</span>not caption<span style="color: #8c8c8c;">)</span> <span style="color: #ffa07a;">""</span>
           <span style="color: #8c8c8c;">(</span>format <span style="color: #ffa07a;">"&lt;label class=\"org-src-name\"&gt;%s&lt;/label&gt;"</span>
                   <span style="color: #8c8c8c;">(</span>org-export-data caption info<span style="color: #8c8c8c;">)))</span>
         <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> use-highlight
             <span style="color: #8c8c8c;">(</span>format <span style="color: #ffa07a;">"\n&lt;pre%s%s&gt;&lt;code class=\"%s\" %s&gt;%s&lt;/code&gt;&lt;/pre&gt;"</span>
                     <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">or</span> <span style="color: #8c8c8c;">(</span>frag-class frag info<span style="color: #8c8c8c;">)</span> <span style="color: #ffa07a;">""</span><span style="color: #8c8c8c;">)</span>
                     label lang code-attribs code<span style="color: #8c8c8c;">)</span>
           <span style="color: #8c8c8c;">(</span>format <span style="color: #ffa07a;">"\n&lt;pre %s%s&gt;%s&lt;/pre&gt;"</span>
                   <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">or</span> <span style="color: #8c8c8c;">(</span>frag-class frag info<span style="color: #8c8c8c;">)</span>
                       <span style="color: #8c8c8c;">(</span>format <span style="color: #ffa07a;">" class=\"src src-%s\""</span> lang<span style="color: #8c8c8c;">))</span>
                   label code<span style="color: #8c8c8c;">)</span>
           <span style="color: #8c8c8c;">)</span>
         <span style="color: #8c8c8c;">(</span><span style="color: #00ffff;">if</span> klipsify <span style="color: #8c8c8c;">(</span>format <span style="color: #ffa07a;">"&lt;klipse-snippet data-language=\"%s\"&gt;%s&lt;/klipse-snippet&gt;"</span>
                              lang code<span style="color: #8c8c8c;">)</span> <span style="color: #ffa07a;">""</span><span style="color: #8c8c8c;">))))))</span>
</pre>
      </div>
      
      <p>
        As you can see, it now decides whether or not to &ldquo;klipsify&rdquo; the code aafter checking if the klipse plugin is loaded, and if it supports the language of the src block. Klipsifying code just means hiding the original code block and instead showing the klipse-snippet, which will be replaced by an iframe in javascript. I guess I could just do that work right in this same elisp function! Oh well, maybe next time.
      </p>
      
      <p>
        So far, this seems to work great, though I don&rsquo;t have access to the DOM of the larger document ,which is sort of a drag.
      </p>
      
      <p>
        <b>[UPDATE <span class="timestamp-wrapper"><span class="timestamp">2016-12-11 Sun</span></span>]:</b> However, @viebel pointed out to me that you can access the parent frame with:
      </p>
      
      <div class="org-src-container">
        <pre class="src src-js"><span style="color: #00ffff;">var</span> <span style="color: #eedd82;">d</span> = parent.document
</pre>
      </div>
      
      <p>
        Pretty cool! Note: klipse will hang if you try to type this in directly. It will only work if you pass the completed line in as part of the initial code block.
      </p>
      
      <p>
        Also, this is not a solution that works for rendering HTML pages. For this we need something else. I would like to use RevealEditor, but am not quite there yet (can&rsquo;t get its code to load properly). So for now I&rsquo;m embedding a JSBin instance with preoaded code &#x2013; I just add an iframe tag directly in my org-mode file. It&rsquo;s not a s pretty or robust, and I can&rsquo;t see my code when I&rsquo;m writing, which is too bad, but it&rsquo;s still pretty useful.
      </p>
      
      <p>
        I would really like to hear what other people think! I&rsquo;d especially like to hear about alternatives or improvements.
      </p>
    </div>
  </div>
</div>