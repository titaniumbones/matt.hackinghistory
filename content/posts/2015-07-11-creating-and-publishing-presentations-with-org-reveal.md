---
title: Creating and Publishing Presentations with Org-reveal
author: matt
type: post
date: 2015-07-11T14:20:00+00:00
url: /2015/07/11/creating-and-publishing-presentations-with-org-reveal/
dsq_thread_id:
  - "4318126190"
categories:
  - emacs

---
For several years, I&rsquo;ve been using Org-mode to compose slides for my lectures. This method is great, because I get to work in plain-text and focus on the content of my lectures rather than animations; but it&rsquo;s meant that when I want to share my presentations with others, there&rsquo;s a certain amount of work involved as I move from a local copy on my computer to a web-based version. (This has largely been an issue because I sometimes _compose_ my lectures sitting in a café with lousy Internet, and I sometimes _give_ my lectures in a horrible classroom at U of T with terrible Internet reception.) I&rsquo;ve now largely solved this problem, though there is hopefully an improvement coming down the pipe which will make it even easier. 

Org-mode has the capacity to export to a number of slide-like formats, including the [LaTeX-based Beamer format][1], which also makes good PDF presentations, a couple of Emacs-based presentation tools, and a number of HTML5 formats. Since I teach about the web all the time, the HTML5 formats have always been the most appealing to me. 

<div id="outline-container-orgheadline1" class="outline-2">
  <h2 id="orgheadline1">
    Org-Reveal Setup
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline1">
    <p>
      I have used and still very much like <a href="https://github.com/imakewebthings/deck.js/wiki">deck.js</a> (<a href="https://github.com/cybercode/org-slides">exporter here</a>), but have recently switched to <a href="https://github.com/yjwen/org-reveal">org-reveal</a>, which I really like a lot. It&rsquo;s not part of the official org distribution, so installation and setup are a little more involved, but not difficult. I just cloned the org-reveal and reveal.js repositories:
    </p>
    
    <div class="org-src-container">
      <pre class="src src-sh"><span style="color: #483d8b;">cd</span> ~/src
git clone https://github.com/hakimel/reveal.js.git
git clone https://github.com/yjwen/org-reveal.git
</pre>
    </div>
    
    <p>
      and put this in my <code>emacs-init.el</code>:
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">org-reveal</span>
(add-to-list 'load-path <span style="color: #8b2252;">"~/src/org-reveal"</span>)
(<span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-reveal</span>)
<span style="color: #b22222;">;; </span><span style="color: #b22222;">set local root</span>
(<span style="color: #a020f0;">setq</span> org-reveal-root <span style="color: #8b2252;">"file:///home/matt/src/reveal.js"</span>)
</pre>
    </div>
    
    <p>
      That&rsquo;s all that&rsquo;s needed to get export working! I find it&rsquo;s really fast to prepare lectures.
    </p>
  </div>
</div>

<div id="outline-container-orgheadline2" class="outline-2">
  <h2 id="orgheadline2">
    Publishing
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline2">
    <p>
      That&rsquo;s great for <i>giving</i> lectures, and is all I really need at 9:55 when I&rsquo;m trying to type my lecture and walk to class at the same time. But after lecture I want to put my slides somewhere my students can see them. Even if I wanted to, it would be impossible for me to post to Blackboard, which turns these files into garbage. What I want to do is publish them to the web; but I need to make sure that all the JS and CSS links are pointing to the web-based libraries and not my local copies, which of course no one but me can see. To do this I had to make one small change to <code>org-reveal.el</code>, which I have submitted as a <a href="https://github.com/yjwen/org-reveal/pull/125/files">pull request</a>. This creates a new variable, <code>org-reveal-extra-css</code>, which I can refer to in my own functions.
    </p>
    
    <p>
      Then I use org-mode&rsquo;s fantastic built-in <a href="http://orgmode.org/worg/org-tutorials/org-publish-html-tutorial.html">Publishing functions</a> to push my slides to a public website. Publishing allows you to perform an export on many files, and customize the output in powerful ways that are mostly beyond me, actually. Still, I have a setup that I like a lot.
    </p>
    
    <p>
      First, my <code>org-publish-project-alist</code>, which defines the publishing targets. Note especially the top part, which defines &ldquo;meta-projects&rdquo;: for instance, I can publish all the slides and source files for all my classes with one command, <code>M-x org-publish-projects [RET] courses</code>.
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp">(<span style="color: #a020f0;">setq</span> org-publish-project-alist
      '(
        (<span style="color: #8b2252;">"courses"</span>
         <span style="color: #483d8b;">:components</span> (<span style="color: #8b2252;">"dh"</span> <span style="color: #8b2252;">"rlg231"</span>))
        (<span style="color: #8b2252;">"rlg231"</span>
         <span style="color: #483d8b;">:components</span> (<span style="color: #8b2252;">"rlg231-lecture-slides"</span> <span style="color: #8b2252;">"rlg231-lecture-source"</span>))
        (<span style="color: #8b2252;">"dh"</span>
         <span style="color: #483d8b;">:components</span> (<span style="color: #8b2252;">"digital-history-lecture-slides"</span> <span style="color: #8b2252;">"digital-history-lecture-source"</span>))

        (<span style="color: #8b2252;">"rlg231-lecture-slides"</span>
         <span style="color: #483d8b;">:base-directory</span> <span style="color: #8b2252;">"~/RLG231/Lectures/"</span>
         <span style="color: #483d8b;">:base-extension</span> <span style="color: #8b2252;">"org"</span>
         <span style="color: #483d8b;">:publishing-directory</span> <span style="color: #8b2252;">"/ssh:matt@shimano:/var/www/sandbox/RLG231/Lectures/Slides"</span>
         <span style="color: #483d8b;">:recursive</span> t
         <span style="color: #483d8b;">:publishing-function</span> mwp-org-reveal-publish-to-html
         <span style="color: #483d8b;">:preparation-function</span> nil 
         <span style="color: #483d8b;">:completion-function</span> nil
         <span style="color: #483d8b;">:headline-levels</span> 4             <span style="color: #b22222;">; </span><span style="color: #b22222;">Just the default for this project.</span>
         <span style="color: #483d8b;">:exclude</span> <span style="color: #8b2252;">"LectureOutlines.org"</span>
         <span style="color: #483d8b;">:exclude-tags</span> note noexport
         <span style="color: #483d8b;">:auto-preamble</span> t)

        (<span style="color: #8b2252;">"rlg231-lecture-source"</span>
         <span style="color: #483d8b;">:base-directory</span> <span style="color: #8b2252;">"~/RLG231/Lectures/"</span>
         <span style="color: #483d8b;">:base-extension</span> <span style="color: #8b2252;">"org"</span>
         <span style="color: #483d8b;">:publishing-directory</span> <span style="color: #8b2252;">"/ssh:matt@shimano:/var/www/sandbox/RLG231/Lectures/Source"</span>
         <span style="color: #483d8b;">:recursive</span> t
         <span style="color: #483d8b;">:publishing-function</span> org-org-publish-to-org
         <span style="color: #483d8b;">:preparation-function</span> nil
         <span style="color: #483d8b;">:completion-function</span> nil
         <span style="color: #483d8b;">:headline-levels</span> 4             <span style="color: #b22222;">; </span><span style="color: #b22222;">Just the default for this project.</span>
         <span style="color: #483d8b;">:exclude</span> <span style="color: #8b2252;">"LecturePlans.org"</span>
         <span style="color: #b22222;">;; </span><span style="color: #b22222;">:exclude "LectureOutlines.org"</span>
         <span style="color: #483d8b;">:exclude-tags</span> note noexport
         <span style="color: #483d8b;">:auto-preamble</span> t)

        (<span style="color: #8b2252;">"digital-history-lecture-source"</span>
         <span style="color: #483d8b;">:base-directory</span> <span style="color: #8b2252;">"~/DH/Lectures"</span>
         <span style="color: #483d8b;">:base-extension</span> <span style="color: #8b2252;">"org"</span>
         <span style="color: #483d8b;">:publishing-directory</span> <span style="color: #8b2252;">"/ssh:matt@shimano:/var/www/sandbox/DigitalHistory/Lectures/Source"</span>
         <span style="color: #483d8b;">:recursive</span> t
         <span style="color: #483d8b;">:publishing-function</span> org-org-publish-to-org
         <span style="color: #483d8b;">:preparation-function</span> 
         <span style="color: #483d8b;">:completion-function</span> 
         <span style="color: #483d8b;">:headline-levels</span> 4             <span style="color: #b22222;">; </span><span style="color: #b22222;">Just the default for this project.</span>
         <span style="color: #b22222;">;; </span><span style="color: #b22222;">:exclude "LecturePlans.org"</span>
         <span style="color: #483d8b;">:exclude</span> <span style="color: #8b2252;">"LectureOutlines.org"</span>
         <span style="color: #483d8b;">:exclude-tags</span> note noexport
         <span style="color: #483d8b;">:auto-preamble</span> t)

        (<span style="color: #8b2252;">"digital-history-lecture-slides"</span>
         <span style="color: #483d8b;">:base-directory</span> <span style="color: #8b2252;">"~/DH/Lectures"</span>
         <span style="color: #483d8b;">:base-extension</span> <span style="color: #8b2252;">"org"</span>
         <span style="color: #483d8b;">:publishing-directory</span> <span style="color: #8b2252;">"/ssh:matt@shimano:/var/www/sandbox/DigitalHistory/Lectures/Slides"</span>
         <span style="color: #483d8b;">:recursive</span> t
         <span style="color: #483d8b;">:publishing-function</span> mwp-org-reveal-publish-to-html
         <span style="color: #483d8b;">:preparation-function</span> 
         <span style="color: #483d8b;">:completion-function</span> 
         <span style="color: #483d8b;">:headline-levels</span> 4             <span style="color: #b22222;">; </span><span style="color: #b22222;">Just the default for this project.</span>
         <span style="color: #b22222;">;; </span><span style="color: #b22222;">:exclude "LecturePlans.org"</span>
         <span style="color: #483d8b;">:exclude</span> <span style="color: #8b2252;">"LectureOutlines.org"</span>
         <span style="color: #483d8b;">:exclude-tags</span> note noexport
         <span style="color: #483d8b;">:auto-preamble</span> t)

        <span style="color: #b22222;">;; </span><span style="color: #b22222;">("newone-lecture-slides"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:base-directory "~/NewOne/Lectures/"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:base-extension "org"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:publishing-directory "/ssh:matt@shimano:/var/www/sandbox/NewOne/Lectures"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:recursive t</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:publishing-function org-deck-publish-to-html</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:headline-levels 4             ; Just the default for this project.</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:exclude-tags note noexport</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:auto-preamble t)</span>

        <span style="color: #b22222;">;; </span><span style="color: #b22222;">("newone-lecture-notes"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:base-directory "~/NewOne/Lectures/"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:base-extension "org"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:publishing-directory "/ssh:matt@shimano:/var/www/sandbox/NewOne/Lectures-with-notes"</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:recursive t</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:publishing-function org-html-publish-to-html</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:headline-levels 4             ; Just the default for this project.</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:exclude-tags noexport</span>
        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">:auto-preamble t)</span>

        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">("newone-images"</span>
        <span style="color: #b22222;">;;        </span><span style="color: #b22222;">:base-directory "~/NewOne/Images/"</span>
        <span style="color: #b22222;">;;        </span><span style="color: #b22222;">:base-extension "jpg\\|gif\\|png"</span>
        <span style="color: #b22222;">;;        </span><span style="color: #b22222;">:publishing-directory "/ssh:matt@shimano:/var/www/sandbox/NewOne/Images"</span>
        <span style="color: #b22222;">;;        </span><span style="color: #b22222;">:publishing-function org-publish-attachment)</span>

        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">("newone" :components ("newone-lecture-slides" "newone-lecture-notes" "newone-images") )</span>

        <span style="color: #b22222;">;;  </span><span style="color: #b22222;">("presentations"</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">:base-directory "~/Dropbox/Work/Talks/"</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">:base-extension "org"</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">:publishing-directory "/ssh:matt@shimano:/var/www/sandbox/Presentations"</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">:headline-levels 4 ; just the default for this project</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">:exclude-tags noexport</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">:auto-preamble t</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">:publishing-function mwp-org-deck-publish-to-html</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">;; :completion-function mwp-update-published-paths</span>
        <span style="color: #b22222;">;;   </span><span style="color: #b22222;">)</span>

      ))
</pre>
    </div>
    
    <p>
      Notice the publishing function, which is set to <code>mwp-org-deck-publish-to-html</code>. This is a simple function that resets the base url and <code>extra-css</code> values to web-based ones before publication, so that the presentations work when online. Notice I&rsquo;ve also reset the <code>deck.js</code> base url, in case I ever decide to change back to deck.
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp">(<span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-reveal-publish-to-html</span> (plist filename pub-dir)
  <span style="color: #8b2252;">"Publish an org file to reveal.js HTML Presentation.</span>
<span style="color: #8b2252;">FILENAME is the filename of the Org file to be published.  PLIST</span>
<span style="color: #8b2252;">is the property list for the given project.  PUB-DIR is the</span>
<span style="color: #8b2252;">publishing directory. Returns output file name."</span>
  (<span style="color: #a020f0;">let</span> ((org-deck-base-url <span style="color: #8b2252;">"http://sandbox.hackinghistory.ca/Tools/deck.js/"</span>)
        (org-reveal-root <span style="color: #8b2252;">"http://sandbox.hackinghistory.ca/Tools/reveal.js/"</span>)
        (org-reveal-extra-css <span style="color: #8b2252;">"http://sandbox.hackinghistory.ca/Tools/reveal.js/css/local.css"</span>))

    (org-publish-org-to 'reveal filename <span style="color: #8b2252;">".html"</span> plist pub-dir))
  )
</pre>
    </div>
    
    <p>
      And that&rsquo;s it, magic!
    </p>
  </div>
</div>

<div id="outline-container-orgheadline3" class="outline-2">
  <h2 id="orgheadline3">
    Still to do
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline3">
    <p>
      I like this a lot, but there are a couple of pieces I&rsquo;d still like to implement.
    </p>
    
    <dl class="org-dl">
      <dt>
        Fix all local file URL&rsquo;s
      </dt>
      
      <dd>
        I&rsquo;d like to write a function to take a final pass through all the links and change <code>file:///</code> links to <b>HTML relative links</b>. That will take some work though.
      </dd>
      
      <dt>
        Export as standalone
      </dt>
      
      <dd>
        There is work underway to allow presentations to be generated as stand-alone files that can be, e.g, sent by email. I like this idea a lot. <a href="https://github.com/yjwen/org-reveal/issues/121">See this Github issue</a>.
      </dd>
      
      <dt>
        Standardize notes, fragments
      </dt>
      
      <dd>
        Every time I switch from one presentation framework to another, I have to learn a whole different syntax for things like fragments (bits of content that don&rsquo;t appear on the slide immediately, but are instead stepped through) and speaker notes (that don&rsquo;t appear on the slide that your viewers see, but are only visible to you in some kind of preview mode). It would be great if the various slide modes could work towards a common syntax for these things. If I have time, energy, and skills, I would like to help develop this a little.
      </dd>
    </dl>
  </div>
</div>

<div id="outline-container-orgheadline4" class="outline-2">
  <h2 id="orgheadline4">
    See my slides
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline4">
    <p>
      If you want to see some examples of the end product, <a href="http://sandbox.hackinghistory.ca/DigitalHistory/Lectures/">here is a link to my Digital History lecture archive</a> (still being built!). Many of my course materials are also <a href="https://github.com/titaniumbones?tab=repositories">online at Github</a>.
    </p>
  </div>
</div>

 [1]: http://orgmode.org/worg/exporters/beamer/beamer-dual-format.html