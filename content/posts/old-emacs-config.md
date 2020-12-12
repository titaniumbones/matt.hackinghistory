---
title: No Title
author: matt
type: page
date: 2015-07-15T18:13:00+00:00
draft: true

---
<div id="outline-container-orgheadline1" class="outline-2">
  <h2 id="orgheadline1">
    Load Paths
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline1">
    <p>
      Allow emacs to find local files, which are mostly in <code>/home/me/src/</code> and <code>/home/me/.emacs.d/site-lisp/</code>
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> site-lisp-dir
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"site-lisp"</span> user-emacs-directory<span style="color: #707183;">)</span>
      src-dir
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"src"</span> <span style="color: #8b2252;">"/home/matt/"</span><span style="color: #707183;">)</span>
      org-dir
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"org-mode"</span> src-dir<span style="color: #707183;">)</span>
      org-lisp
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"lisp"</span> org-dir<span style="color: #707183;">)</span>
      org-contrib
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"contrib/lisp"</span> org-dir<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Set up load path</span>
<span style="color: #707183;">(</span>add-to-list 'load-path site-lisp-dir<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-to-list 'load-path src-dir<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> load-path <span style="color: #707183;">(</span>cons org-lisp load-path<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> load-path <span style="color: #707183;">(</span>cons org-contrib load-path<span style="color: #707183;">))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this should be moved!</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> load-path <span style="color: #707183;">(</span>cons <span style="color: #8b2252;">"~/src/org2blog/"</span> load-path<span style="color: #707183;">))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">get current Org-mode docs</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> Info-default-directory-list <span style="color: #707183;">(</span>cons <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"docs"</span> org-dir<span style="color: #707183;">)</span> Info-default-directory-list<span style="color: #707183;">))</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline4" class="outline-2">
  <h2 id="orgheadline4">
    Official Packages
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline4">
  </div>
  
  <div id="outline-container-orgheadline2" class="outline-3">
    <h3 id="orgheadline2">
      Initialize the Package system
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline2">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;; </span><span style="color: #b22222;">elpa interface</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">This was installed by package-install.el.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">This provides support for the package system and</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">interfacing with ELPA, the package archive.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Move this code earlier if you want to reference</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">packages in your .emacs.</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> package-archives <span style="color: #707183;">())</span>
<span style="color: #707183;">(</span>add-to-list 'package-archives '<span style="color: #707183;">(</span><span style="color: #8b2252;">"marmalade"</span> . <span style="color: #8b2252;">"http://marmalade-repo.org/packages/"</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>add-to-list 'package-archives '<span style="color: #707183;">(</span><span style="color: #8b2252;">"gnu"</span> . <span style="color: #8b2252;">"http://elpa.gnu.org/packages/"</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>add-to-list 'package-archives '<span style="color: #707183;">(</span><span style="color: #8b2252;">"org"</span> . <span style="color: #8b2252;">"http://orgmode.org/elpa/"</span><span style="color: #707183;">)</span> t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-to-list 'package-archives '<span style="color: #707183;">(</span><span style="color: #8b2252;">"melpa"</span> . <span style="color: #8b2252;">"http://melpa.milkbox.net/packages/"</span><span style="color: #707183;">)</span> t<span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">basic initialization, (require) non-ELPA packages, etc.</span>
<span style="color: #707183;">(</span>package-initialize<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline3" class="outline-3">
    <h3 id="orgheadline3">
      Require the packages
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline3">
      <p>
        I&rsquo;m not sure why I have most of these. I&rsquo;m currently experimenting with commenting out some of them.
      </p>
      
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">(require) your ELPA packages, configure them as normal</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">yasnippet</span> <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ac-js2</span> <span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">autopair</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">clener modeline</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">http://whattheemacsd.com/init.el-04.html</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">diminish</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">C-= to expand selection by semantic unit</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">https://github.com/magnars/expand-region.el</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">expand-region</span><span style="color: #707183;">)</span>


<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">highlight-symbol</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">essential for all html exports</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">htmlize</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">js stuff</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">jquery-doc</span> <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">js2-mode</span> <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">js-doc</span> <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">json-mode</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">I don't actually use this much</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">markdown-mode</span> <span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">multiple cursors extras</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">not using right now</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'mc-extras )</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">php</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">php-eldoc</span> <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">php-extras</span> <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">php-mode</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">difficult but cool</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">paredit</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">essential</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">sass-mode</span> <span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">mostly a dependency</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">simple-httpd</span> <span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">live editing for javascript, but haven't been able to get it to work.</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">skewer-mode</span> <span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">not sure if I need this or not</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'smex )</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this has become super important -- allows me to go back to any previous state of buffer</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">undo-tree</span><span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">not sure of the status of these either</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">web-beautify</span> <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">web-mode</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">modern replacement for flymake</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">lfycheck is dependent on external tools. Not set up properly yet</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">http://www.flycheck.org/manual/latest/Quickstart.html#Quickstart</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">This really needs its own lesson, it's fantastic!</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">flycheck</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'after-init-hook #'global-flycheck-mode<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline5" class="outline-2">
  <h2 id="orgheadline5">
    Site-local Packages
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline5">
    <p>
      Packages that I don&rsquo;t use ELPA to manage &#x2013; mostly org and related org-mode stuff, and local modifications. It would be better to move some of this to ELPA.
    </p>
  </div>
  
  <div id="outline-container-orgheadline6" class="outline-3">
    <h3 id="orgheadline6">
      From site-lisp
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline6">
      <p>
        A lot of this is from Magnar
      </p>
      
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">start with some saner/better defaults</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">stole this from Magnar, can't remember what's in it.  </span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">better-defaults</span><span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">more of magnar's defuns and modes</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">some of these I should get rid of</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> defuns-dir <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"defuns"</span> user-emacs-directory<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">dolist</span> <span style="color: #707183;">(</span>file <span style="color: #707183;">(</span>directory-files defuns-dir t <span style="color: #8b2252;">"\\w+"</span><span style="color: #707183;">))</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>file-regular-p file<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>load file<span style="color: #707183;">)))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">paredit for html. not using it yet, too hard</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">tagedit</span><span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">https://github.com/magnars/simplezen.el</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">it's cool but not currently in use by me.  I would like to give it a tryhttps://github.com/magnars/simplezen.el</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">simplezen</span><span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">not using yet, too hard</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">multiple-cursors</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">delsel</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">jump-char</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;;</span><span style="color: #b22222;">(require 'eproject)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">also super cool but too hard for me right now.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'wgrep)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'smart-forward)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'change-inner)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'multifiles)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Smart M-x is smart</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'smex)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(smex-initialize)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this is supposed to provide gnome recent files integration, but doesn't</span>
<span style="color: #707183;">(</span>add-to-list 'load-path <span style="color: #8b2252;">"~/src/zeitgeist-dataproviders/emacs/"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">zeitgeist</span><span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this next bit is actually just part of my configuration, which needs to be incorporated</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">into this file</span>
<span style="color: #b22222;">;;; </span><span style="color: #b22222;">web programming tweaks</span>
<span style="color: #707183;">(</span>load-library <span style="color: #8b2252;">"webstuff"</span><span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline7" class="outline-3">
    <h3 id="orgheadline7">
      From <code>src</code> dir
    </h3>
  </div>
</div>

<div id="outline-container-orgheadline11" class="outline-2">
  <h2 id="orgheadline11">
    User Experience
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline11">
  </div>
  
  <div id="outline-container-orgheadline8" class="outline-3">
    <h3 id="orgheadline8">
      Backup and AUtosave
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline8">
      <ul class="org-ul">
        <li>
          backup everything to ~/.backup
        </li>
        <li>
          keep 5 recent and 5 old versions of every file
        </li>
        <li>
          rename to <code>!home!matt!etc</code>
        </li>
        <li>
          save place in files on exit
        </li>
      </ul>
      
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;; </span><span style="color: #b22222;">backup and autosave</span>
<span style="color: #b22222;">;;</span>
<span style="color: #b22222;">;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> make-backup-files t<span style="color: #707183;">)</span>       <span style="color: #b22222;">; </span><span style="color: #b22222;">enable backup file</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">put packups in ~/.backup</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> backup-directory-alist
      <span style="color: #707183;">(</span>cons <span style="color: #707183;">(</span>cons <span style="color: #8b2252;">"\\.*$"</span> <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"~/.backup"</span><span style="color: #707183;">))</span>
            backup-directory-alist<span style="color: #707183;">))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> version-control t<span style="color: #707183;">)</span>     <span style="color: #b22222;">; </span><span style="color: #b22222;">enable versions of backup</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> kept-new-versions 5<span style="color: #707183;">)</span>   <span style="color: #b22222;">; </span><span style="color: #b22222;">how many keep new verisons</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> kept-old-versions 5<span style="color: #707183;">)</span>   <span style="color: #b22222;">; </span><span style="color: #b22222;">how many keep old versions</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> delete-old-versions t<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">delete old version without asking</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> vc-make-backup-files t<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">still make a backup for version-controled files</span>

<span style="color: #b22222;">;;;;; </span><span style="color: #b22222;">Autosave in .backup dir</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> auto-save-file-name-transforms
      '<span style="color: #707183;">((</span><span style="color: #8b2252;">"</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">[</span><span style="color: #8b2252;">^</span><span style="color: #8b2252;">/]*/</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">*</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">[</span><span style="color: #8b2252;">^</span><span style="color: #8b2252;">/]*</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">\\'"</span> <span style="color: #8b2252;">"~/.backup/\\2"</span> t<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">make emacs remember where it is in the file you just closed</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">saveplace</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq-default</span> save-place t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> server-visit-hook <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">(</span>save-place-find-file-hook<span style="color: #707183;">)))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> save-place-file <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">".places"</span> user-emacs-directory<span style="color: #707183;">))</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline9" class="outline-3">
    <h3 id="orgheadline9">
      Guidekey
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline9">
      <p>
        This has been helpful for when I&rsquo;m forgetful. It pops up a help buffer showing possible completions of chord keys.
      </p>
      
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">learn some more key bindings with guide-key</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">guide-key</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">guide-key</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> guide-key/guide-key-sequence '<span style="color: #707183;">(</span><span style="color: #8b2252;">"C-x r"</span> <span style="color: #8b2252;">"C-x 4"</span> <span style="color: #8b2252;">"C-x v"</span> <span style="color: #8b2252;">"C-x 8"</span> <span style="color: #8b2252;">"C-x +"</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>guide-key-mode 1<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> guide-key/recursive-key-sequence-flag t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> guide-key/popup-window-position 'bottom<span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">some specific settings for org-mode</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">guide-key/my-hook-function-for-org-mode</span> <span style="color: #707183;">()</span>
  <span style="color: #707183;">(</span>guide-key/add-local-guide-key-sequence <span style="color: #8b2252;">"C-c"</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>guide-key/add-local-guide-key-sequence <span style="color: #8b2252;">"C-c C-x"</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>guide-key/add-local-highlight-command-regexp <span style="color: #8b2252;">"org-"</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>add-hook 'org-mode-hook 'guide-key/my-hook-function-for-org-mode<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline10" class="outline-3">
    <h3 id="orgheadline10">
      Tramp, other advanced stuff
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline10">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;;; </span><span style="color: #b22222;">sml-modeline-scrollbar...</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">I think this is for smooth scrolling</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">sml-modeline</span> nil 'noerror<span style="color: #707183;">)</span>    <span style="color: #b22222;">;; </span><span style="color: #b22222;">use sml-modeline if available</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">progn</span>
      <span style="color: #707183;">(</span>sml-modeline-mode 1<span style="color: #707183;">)</span>                   <span style="color: #b22222;">;; </span><span style="color: #b22222;">show buffer pos in the mode line</span>
      <span style="color: #707183;">(</span>scroll-bar-mode -1<span style="color: #707183;">))</span>                   <span style="color: #b22222;">;; </span><span style="color: #b22222;">turn off the scrollbar</span>
  <span style="color: #707183;">(</span>scroll-bar-mode 1<span style="color: #707183;">)</span>                       <span style="color: #b22222;">;; </span><span style="color: #b22222;">otherwise, show a scrollbar...</span>
  <span style="color: #707183;">(</span>set-scroll-bar-mode 'right<span style="color: #707183;">))</span>             <span style="color: #b22222;">;; </span><span style="color: #b22222;">... on the right</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">more scrolling stuff</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span>
 scroll-margin 0
 scroll-conservatively 100000
 scroll-preserve-screen-position 1<span style="color: #707183;">)</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">flyspell</span>
<span style="color: #707183;">(</span>add-hook 'wl-mail-setup-hook
          <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span><span style="color: #707183;">()</span>
            <span style="color: #707183;">(</span>flyspell-mode 1<span style="color: #707183;">)))</span>



<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">iswitchb-mode</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">I want to be able to conmute between a split and a single window (sort of "C-x 1" for the one on focus)</span>
<span style="color: #707183;">(</span>iswitchb-mode 1<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">my-iswitchb-select</span><span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Jump to buffer without having to hit 'RET' key or C-j. The binding to C-2 is more ergonomic"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>window-minibuffer-p <span style="color: #707183;">(</span>selected-window<span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span>iswitchb-select-buffer-text<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span>define-key global-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-2"</span><span style="color: #707183;">)</span> 'my-iswitchb-select<span style="color: #707183;">)</span>


<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">my-iswitchb-close</span><span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Open iswitchb or, if in minibuffer go to next match. Handy way to cycle through the ring."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>window-minibuffer-p <span style="color: #707183;">(</span>selected-window<span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span>keyboard-escape-quit<span style="color: #707183;">)))</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">tramp mode</span>
 <span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">tramp</span><span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline12" class="outline-2">
  <h2 id="orgheadline12">
    Function Definitions
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline12">
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">here's a quick macro to select and copy a buffer</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">F6 copy whole buffer</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-copy-whole-buffer</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Copy the whole buffer into the kill ring"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>mark-whole-buffer<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>copy-region-as-kill<span style="color: #707183;">(</span>region-beginning<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>region-end<span style="color: #707183;">))</span>
  <span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">[</span>f6<span style="color: #707183;">])</span> 'mwp-copy-whole-buffer<span style="color: #707183;">)</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">better C-a behaviour everywhere (http://emacsredux.com/blog/2013/05/22/smarter-navigation-to-the-beginning-of-a-line/)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">smarter-move-beginning-of-line</span> <span style="color: #707183;">(</span>arg<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Move point back to indentation of beginning of line.</span>

<span style="color: #8b2252;">Move point to the first non-whitespace character on this line.</span>
<span style="color: #8b2252;">If point is already there, move to the beginning of the line.</span>
<span style="color: #8b2252;">Effectively toggle between the first non-whitespace character and</span>
<span style="color: #8b2252;">the beginning of the line.</span>

<span style="color: #8b2252;">If ARG is not nil or 1, move forward ARG - 1 lines first.  If</span>
<span style="color: #8b2252;">point reaches the beginning or end of the buffer, stop there."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"^p"</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> arg <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> arg 1<span style="color: #707183;">))</span>

  <span style="color: #b22222;">;; </span><span style="color: #b22222;">Move lines first</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>/= arg 1<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>line-move-visual nil<span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span>forward-line <span style="color: #707183;">(</span>1- arg<span style="color: #707183;">))))</span>

  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>orig-point <span style="color: #707183;">(</span>point<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span>back-to-indentation<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>= orig-point <span style="color: #707183;">(</span>point<span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span>move-beginning-of-line 1<span style="color: #707183;">))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">remap C-a to `</span><span style="color: #008b8b;">smarter-move-beginning-of-line</span><span style="color: #b22222;">'</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">[</span>remap move-beginning-of-line<span style="color: #707183;">]</span>
                'smarter-move-beginning-of-line<span style="color: #707183;">)</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">prodigy looks really ocol,</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">https://github.com/rejeep/prodigy.el</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">but I am not using it at the moment ,so commented out</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'prodigy)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(global-set-key (kbd "C-x M-m") 'prodigy)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Font lock dash.el</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">dash.el is amazing, maybe to oadvanced for me</span>
<span style="color: #707183;">(</span>eval-after-load <span style="color: #8b2252;">"dash"</span> '<span style="color: #707183;">(</span>dash-enable-font-lock<span style="color: #707183;">))</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline13" class="outline-2">
  <h2 id="orgheadline13">
    More Load Paths
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline13">
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;</span>
<span style="color: #b22222;">;;</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">some universal customizations</span>
<span style="color: #b22222;">;;</span>
<span style="color: #b22222;">;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Set path to dependencies</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> site-lisp-dir
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"site-lisp"</span> user-emacs-directory<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> src-dir
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"src"</span> <span style="color: #8b2252;">"/home/matt/"</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-dir
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"org-mode"</span> src-dir<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-lisp
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"lisp"</span> org-dir<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-contrib
      <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"contrib/lisp"</span> org-dir<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Set up load path</span>
<span style="color: #707183;">(</span>add-to-list 'load-path site-lisp-dir<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-to-list 'load-path src-dir<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">new load paths</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> load-path <span style="color: #707183;">(</span>cons org-lisp load-path<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> load-path <span style="color: #707183;">(</span>cons org-contrib load-path<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> load-path <span style="color: #707183;">(</span>cons <span style="color: #8b2252;">"~/.emacs.d/org2blog/"</span> load-path<span style="color: #707183;">))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">get current docs</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> Info-default-directory-list <span style="color: #707183;">(</span>cons <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"docs"</span> org-dir<span style="color: #707183;">)</span> Info-default-directory-list<span style="color: #707183;">))</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">Keep emacs Custom-settings in separate file</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> custom-file <span style="color: #707183;">(</span>expand-file-name <span style="color: #8b2252;">"custom.el"</span> user-emacs-directory<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>load custom-file<span style="color: #707183;">)</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline14" class="outline-2">
  <h2 id="orgheadline14">
    Paredit
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline14">
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">paredit stuff.  belongs in setup-paredit, oh well</span>
<span style="color: #707183;">(</span>autoload 'enable-paredit-mode <span style="color: #8b2252;">"paredit"</span> <span style="color: #8b2252;">"Turn on pseudo-structural editing of Lisp code."</span> t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'emacs-lisp-mode-hook       #'enable-paredit-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'eval-expression-minibuffer-setup-hook #'enable-paredit-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'ielm-mode-hook             #'enable-paredit-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'lisp-mode-hook             #'enable-paredit-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'lisp-interaction-mode-hook #'enable-paredit-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'scheme-mode-hook           #'enable-paredit-mode<span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">;; Setup key bindings</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'key-bindings)</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline15" class="outline-2">
  <h2 id="orgheadline15">
    Server
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline15">
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">;; Emacs server</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">server</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">unless</span> <span style="color: #707183;">(</span>server-running-p<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>server-start<span style="color: #707183;">))</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline19" class="outline-2">
  <h2 id="orgheadline19">
    Second Half
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline19">
  </div>
  
  <div id="outline-container-orgheadline16" class="outline-3">
    <h3 id="orgheadline16">
      Magnars Keybindings
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline16">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">some stuff form magnars,</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">https://github.com/magnars/.emacs.d/blob/master/key-bindings.el</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this should be moved to own file keybindings.el</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this is old, and some of this gets written over later in the process.</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">misc</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"s-."</span><span style="color: #707183;">)</span> 'copy-from-above-command<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">find function!  a must</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-h C-f"</span><span style="color: #707183;">)</span> 'find-function<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Smart M-x</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-x"</span><span style="color: #707183;">)</span> 'smex<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-X"</span><span style="color: #707183;">)</span> 'smex-major-mode-commands<span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(global-set-key (kbd "C-c C-c M-x") 'execute-extended-command) ;</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Use C-x C-m to do M-x per Steve Yegge's advice</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-x C-m"</span><span style="color: #707183;">)</span> 'smex<span style="color: #707183;">)</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">M-i for back-to-indentation</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-i"</span><span style="color: #707183;">)</span> 'back-to-indentation<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Turn on the menu bar for exploring new modes</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-&lt;f10&gt;"</span><span style="color: #707183;">)</span> 'menu-bar-mode<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">transposing</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Transpose stuff with M-t</span>
<span style="color: #707183;">(</span>global-unset-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-t"</span><span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">which used to be transpose-words</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-t l"</span><span style="color: #707183;">)</span> 'transpose-lines<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-t w"</span><span style="color: #707183;">)</span> 'transpose-words<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-t s"</span><span style="color: #707183;">)</span> 'transpose-sexps<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-t p"</span><span style="color: #707183;">)</span> 'transpose-params<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Killing text</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-S-k"</span><span style="color: #707183;">)</span> 'kill-and-retry-line<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-w"</span><span style="color: #707183;">)</span> 'kill-region-or-backward-word<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c C-w"</span><span style="color: #707183;">)</span> 'kill-to-beginning-of-line<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Use M-w for copy-line if no active region</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-w"</span><span style="color: #707183;">)</span> 'save-region-or-current-line<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"s-w"</span><span style="color: #707183;">)</span> 'save-region-or-current-line<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-W"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">&#955;</span> <span style="color: #707183;">(</span>save-region-or-current-line 1<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Make shell more convenient, and suspend-frame less</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-z"</span><span style="color: #707183;">)</span> 'shell<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-x M-z"</span><span style="color: #707183;">)</span> 'suspend-frame<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Zap to char</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-z"</span><span style="color: #707183;">)</span> 'zap-up-to-char<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"s-z"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>char<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"cZap up to char backwards: "</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>zap-up-to-char -1 char<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-Z"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>char<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"cZap to char: "</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>zap-to-char 1 char<span style="color: #707183;">)))</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"s-Z"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>char<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"cZap to char backwards: "</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>zap-to-char -1 char<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Jump to a definition in the current file. (This is awesome)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">I'm using this keybinding for spell correcton though. Need to set to something else.</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(global-set-key (kbd "C-x C-i") 'ido-imenu)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Perform general cleanup.</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c n"</span><span style="color: #707183;">)</span> 'cleanup-buffer<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c C-n"</span><span style="color: #707183;">)</span> 'cleanup-buffer<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c C-&lt;return&gt;"</span><span style="color: #707183;">)</span> 'delete-blank-lines<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">zen</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this is pretty awesome.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">div.class#id[TAB] --&gt; &lt;div id="id" class="class"&gt;&lt;/div&gt;</span>
<span style="color: #707183;">(</span>define-key html-mode-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c C-z"</span><span style="color: #707183;">)</span> 'simplezen-expand<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>define-key html-mode-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"TAB"</span><span style="color: #707183;">)</span> 'simplezen-expand-or-indent-for-tab<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>define-key web-mode-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c C-z"</span><span style="color: #707183;">)</span> 'simplezen-expand<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline17" class="outline-3">
    <h3 id="orgheadline17">
      Programming
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline17">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;;; </span><span style="color: #b22222;">add eldoc for python</span>
<span style="color: #707183;">(</span>add-hook 'python-mode-hook
          '<span style="color: #707183;">(</span>lambda <span style="color: #707183;">()</span> <span style="color: #707183;">(</span>eldoc-mode 1<span style="color: #707183;">))</span> t<span style="color: #707183;">)</span>
<span style="color: #b22222;">;;;;; </span><span style="color: #b22222;">indent yanked code</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">dolist</span> <span style="color: #707183;">(</span>command '<span style="color: #707183;">(</span>yank yank-pop<span style="color: #707183;">))</span>
  <span style="color: #707183;">(</span>eval `<span style="color: #707183;">(</span><span style="color: #a020f0;">defadvice</span> ,command <span style="color: #707183;">(</span>after indent-region activate<span style="color: #707183;">)</span>
           <span style="color: #707183;">(</span><span style="color: #a020f0;">and</span> <span style="color: #707183;">(</span>not current-prefix-arg<span style="color: #707183;">)</span>
                <span style="color: #707183;">(</span>member major-mode '<span style="color: #707183;">(</span>emacs-lisp-mode lisp-mode
                                                     clojure-mode    scheme-mode
                                                     haskell-mode    ruby-mode
                                                     rspec-mode      python-mode
                                                     c-mode          c++-mode
                                                     objc-mode       latex-mode
                                                     plain-tex-mode<span style="color: #707183;">))</span>
                <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>mark-even-if-inactive transient-mark-mode<span style="color: #707183;">))</span>
                  <span style="color: #707183;">(</span>indent-region <span style="color: #707183;">(</span>region-beginning<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>region-end<span style="color: #707183;">)</span> nil<span style="color: #707183;">))))))</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">Find a Function Definition</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">A simple lambda function to search and find the definition of a function or variable.  Only works In Elisp.  Bound to C-c f.</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c f"</span><span style="color: #707183;">)</span>
                <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span>
                  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
                  <span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">finder</span><span style="color: #707183;">)</span>
                  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>thing <span style="color: #707183;">(</span>intern <span style="color: #707183;">(</span>thing-at-point 'symbol<span style="color: #707183;">))))</span>
                    <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>functionp thing<span style="color: #707183;">)</span>
                        <span style="color: #707183;">(</span>find-function thing<span style="color: #707183;">)</span>
                      <span style="color: #707183;">(</span>find-variable thing<span style="color: #707183;">)))))</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline18" class="outline-3">
    <h3 id="orgheadline18">
      More UI and Misc
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline18">
      <ul class="org-ul">
        <li>
          turn on visual-line-mode
        </li>
        <li>
          use auto-complete
        </li>
        <li>
          use abbrev-mode (spell correction mostly)
        </li>
        <li>
          word-count
        </li>
        <li>
          integrate abbrev and ispell
        </li>
        <li>
          windows
        </li>
        <li>
          hippie expand
        </li>
        <li>
          anzu, which is a really neat feature.
        </li>
      </ul>
      
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">modeline appearance</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this enables smart-mode-line, which colorizes the modeline.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">it's pretty helpful.  </span>
<span style="color: #707183;">(</span>sml/setup<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> sml/no-confirm-load-theme t<span style="color: #707183;">)</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">visual line mode</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">enable visual line mode for text modes</span>
<span style="color: #707183;">(</span>add-hook 'text-mode-hook 'turn-on-visual-line-mode<span style="color: #707183;">)</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">auto-complete</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">auto-complete-config</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-to-list 'ac-dictionary-directories <span style="color: #8b2252;">"~/.emacs.d/ac-dict"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>ac-config-default<span style="color: #707183;">)</span>
<span style="color: #b22222;">; </span><span style="color: #b22222;">Use dictionaries by default</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq-default</span> ac-sources <span style="color: #707183;">(</span>add-to-list 'ac-sources 'ac-source-dictionary<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>global-auto-complete-mode t<span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> ac-auto-start 2<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">Start auto-completion after 2 characters of a word</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> ac-ignore-case nil<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">case sensitivity is important when finding matches</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">abbrev-mode</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> default-abbrev-mode t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> abbrev-file-name <span style="color: #8b2252;">"~/.emacs.d/abbrev_defs"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>read-abbrev-file <span style="color: #8b2252;">"~/.emacs.d/abbrev_defs"</span><span style="color: #707183;">)</span>       <span style="color: #b22222;">;; </span><span style="color: #b22222;">reads the abbreviations file on startup</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> save-abbrevs t<span style="color: #707183;">)</span>              <span style="color: #b22222;">;; </span><span style="color: #b22222;">save abbrevs when files are saved</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">you will be asked before the abbreviations are saved</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> save-abbrevs 'silently<span style="color: #707183;">)</span>              <span style="color: #b22222;">;; </span><span style="color: #b22222;">now I won't be asked</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">auto spell correct! much better!</span>
 <span style="color: #b22222;">;; </span><span style="color: #b22222;">this has been a godsend</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">endless/ispell-word-then-abbrev</span> <span style="color: #707183;">(</span>p<span style="color: #707183;">)</span>
    <span style="color: #8b2252;">"Call `</span><span style="color: #008b8b;">ispell-word</span><span style="color: #8b2252;">'. Then create an abbrev for the correction made.</span>
<span style="color: #8b2252;">  With prefix P, create local abbrev. Otherwise it will be global."</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"P"</span><span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>bef <span style="color: #707183;">(</span>downcase <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span>thing-at-point 'word<span style="color: #707183;">)</span> <span style="color: #8b2252;">""</span><span style="color: #707183;">)))</span> aft<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span>call-interactively 'ispell-word<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> aft <span style="color: #707183;">(</span>downcase <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span>thing-at-point 'word<span style="color: #707183;">)</span> <span style="color: #8b2252;">""</span><span style="color: #707183;">)))</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">unless</span> <span style="color: #707183;">(</span>string= aft bef<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"\"%s\" now expands to \"%s\" %sally"</span>
                 bef aft <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> p <span style="color: #8b2252;">"glob"</span> <span style="color: #8b2252;">"loc"</span> <span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>define-abbrev
          <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> p global-abbrev-table local-abbrev-table<span style="color: #707183;">)</span>
          bef aft<span style="color: #707183;">))))</span> 


<span style="color: #b22222;">;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;</span>
<span style="color: #b22222;">;;</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">word counts</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">word counts</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">wc</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> start end<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Prints number of lines, words and characters in region or whole buffer."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>n 0<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>start <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> mark-active <span style="color: #707183;">(</span>region-beginning<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>point-min<span style="color: #707183;">)))</span>
        <span style="color: #707183;">(</span>end <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> mark-active <span style="color: #707183;">(</span>region-end<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>point-max<span style="color: #707183;">))))</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
      <span style="color: #707183;">(</span>goto-char start<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">while</span> <span style="color: #707183;">(</span>&lt; <span style="color: #707183;">(</span>point<span style="color: #707183;">)</span> end<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>forward-word 1<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> n <span style="color: #707183;">(</span>1+ n<span style="color: #707183;">)))))</span>
    <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"%3d %3d %3d"</span> <span style="color: #707183;">(</span>count-lines start end<span style="color: #707183;">)</span> n <span style="color: #707183;">(</span>- end start<span style="color: #707183;">))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">unfilling paras</span>
<span style="color: #b22222;">;;; </span><span style="color: #b22222;">switching window configurations</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">toggle-windows-split</span><span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Switch back and forth between one window and whatever split of windows we might have in the frame. The idea is to maximize the current buffer, while being able to go back to the previous split of windows in the frame simply by calling this command again."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>not<span style="color: #707183;">(</span>window-minibuffer-p <span style="color: #707183;">(</span>selected-window<span style="color: #707183;">)))</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">progn</span>
        <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>&lt; 1 <span style="color: #707183;">(</span>count-windows<span style="color: #707183;">))</span>
            <span style="color: #707183;">(</span><span style="color: #a020f0;">progn</span>
              <span style="color: #707183;">(</span>window-configuration-to-register ?u<span style="color: #707183;">)</span>
              <span style="color: #707183;">(</span>delete-other-windows<span style="color: #707183;">))</span>
          <span style="color: #707183;">(</span>jump-to-register ?u<span style="color: #707183;">))))</span>
  <span style="color: #707183;">(</span>my-iswitchb-close<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Then, the convenient key binding:</span>
<span style="color: #707183;">(</span>define-key global-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-`"</span><span style="color: #707183;">)</span> 'toggle-windows-split<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>define-key global-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-~"</span><span style="color: #707183;">)</span> 'toggle-windows-split<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>define-key global-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-|"</span><span style="color: #707183;">)</span> 'toggle-windows-split<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">same key, on a spanish keyword mapping since I commute a lot between both</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">back-window</span> <span style="color: #707183;">()</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>other-window -1<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>define-key global-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-x O"</span><span style="color: #707183;">)</span> 'back-window<span style="color: #707183;">)</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">unfill paragraph</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Stefan Monnier &lt;foo at acm.org&gt;. It is the opposite of fill-paragraph</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">unfill-paragraph</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Takes a multi-line paragraph and makes it into a single line of text."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>fill-column <span style="color: #707183;">(</span>point-max<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span>fill-paragraph nil<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;;;</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">hippie expand</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">[</span>remap dabbrev-expand<span style="color: #707183;">]</span> 'hippie-expand<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">anzu -- pretty text replacement</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">anzu</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-anzu-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-%"</span><span style="color: #707183;">)</span> 'anzu-query-replace<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-M-%"</span><span style="color: #707183;">)</span> 'anzu-query-replace-regexp<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">appearance</span> <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this corrects an error I used to have. </span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> print-length 2000<span style="color: #707183;">)</span>
</pre>
      </div>
      
      <p>
        This bit here comes from <a href="http://timothypratley.blogspot.ca/2015/07/seven-specialty-emacs-settings-with-big.html">http://timothypratley.blogspot.ca/2015/07/seven-specialty-emacs-settings-with-big.html</a> Rainbow-delimiters for easily finding excess parens. Not sure I really need it.
      </p>
      
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span>add-hook 'prog-mode-hook 'rainbow-delimiters-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">rainbow-delimiters</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>set-face-attribute 'rainbow-delimiters-unmatched-face nil
                    <span style="color: #483d8b;">:foreground</span> 'unspecified
                    <span style="color: #483d8b;">:inherit</span> 'error<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline50" class="outline-2">
  <h2 id="orgheadline50">
    Organize my life
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline50">
  </div>
  
  <div id="outline-container-orgheadline20" class="outline-3">
    <h3 id="orgheadline20">
      Loading, hooks, minor modes
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline20">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">Org-mode hooks</span>
<span style="color: #707183;">(</span>add-hook 'org-mode-hook
  <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span><span style="color: #707183;">()</span>
    <span style="color: #707183;">(</span>flyspell-mode 1<span style="color: #707183;">)))</span>
<span style="color: #707183;">(</span>add-to-list 'auto-mode-alist '<span style="color: #707183;">(</span><span style="color: #8b2252;">"\\.org"</span> . org-mode<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">still need to load org2blog</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org2blog</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org2blog-autoloads</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this should turhn auto-fill off?</span>
<span style="color: #707183;">(</span>add-hook 'org-mode-hook 'turn-off-auto-fill<span style="color: #707183;">)</span>
<span style="color: #b22222;">;;</span><span style="color: #b22222;">org-mouse.el -- an extra</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-mouse</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">allow firefox integration via org=protocl</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-protocol</span><span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-odt</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-odt-styles-dir <span style="color: #8b2252;">"/home/matt/src/org-mode/etc/styles/"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-odt-styles-dir <span style="color: #8b2252;">"/home/matt/.emacs.d/Templates/"</span><span style="color: #707183;">)</span>

<span style="color: #b22222;">;;; </span><span style="color: #b22222;">Org HTML5 export Formats</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-deck</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-s5</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">markdown export</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-md</span> nil t<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline21" class="outline-3">
    <h3 id="orgheadline21">
      Keybindings
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline21">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;; </span><span style="color: #b22222;">Basic org-mode stuff</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #8b2252;">"\C-cl"</span> 'org-store-link<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #8b2252;">"\C-ca"</span> 'org-agenda<span style="color: #707183;">)</span>
<span style="color: #b22222;">;;; </span><span style="color: #b22222;">timesaving keybindings</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #8b2252;">"\C-c\M-0"</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)(</span>write-abbrev-file <span style="color: #8b2252;">"~/.emacs.d/abbrev_defs"</span><span style="color: #707183;">)))</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline22" class="outline-3">
    <h3 id="orgheadline22">
      Workflow, agenda, refiling, tasks
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline22">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;;; </span><span style="color: #b22222;">add some workflow states</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-todo-keywords
       '<span style="color: #707183;">((</span>sequence <span style="color: #8b2252;">"ACTION(a)"</span> <span style="color: #8b2252;">"WAITING(w)"</span> <span style="color: #8b2252;">"BLOCKED(b)"</span> <span style="color: #8b2252;">"|"</span> <span style="color: #8b2252;">"DONE(d)"</span> <span style="color: #8b2252;">"WON'T DO(o)"</span><span style="color: #707183;">)</span>
         <span style="color: #707183;">(</span>sequence <span style="color: #8b2252;">"PROJECT(p)"</span> <span style="color: #8b2252;">"SOMEDAY(s)"</span> <span style="color: #8b2252;">"MAYBE(m)"</span> <span style="color: #8b2252;">"|"</span> <span style="color: #8b2252;">"COMPLETE(c)"</span><span style="color: #707183;">)))</span>


<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">Capture Templates</span>
<span style="color: #707183;">(</span>define-key global-map <span style="color: #8b2252;">"\C-cc"</span> 'org-capture<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-capture-templates 
      '<span style="color: #707183;">(</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"t"</span> <span style="color: #8b2252;">"Todo Items"</span> <span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"p"</span> <span style="color: #8b2252;">"Password"</span> entry <span style="color: #707183;">(</span>file <span style="color: #8b2252;">"~/GTD/Keep-it-safe.org.gpg"</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"* %^{Description} \n SITE: %^{URL} \n USER:%^{USER} \n PASS:%^{PASS}\n%? \n"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"tt"</span> <span style="color: #8b2252;">"Teaching Todo with Sechedule & Tags set"</span> entry <span style="color: #707183;">(</span>file+olp <span style="color: #8b2252;">"~/Dropbox/GTD/gtd.org"</span> <span style="color: #8b2252;">"Tasks"</span> <span style="color: #8b2252;">"Teaching"</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"* ACTION %^{Description}  %^G:teaching:\nSCHEDULED:%(org-insert-time-stamp (org-read-date nil t \".+1d\"))%?"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"tx"</span> <span style="color: #8b2252;">"Other Todo entries"</span> entry <span style="color: #707183;">(</span>file+headline <span style="color: #8b2252;">"~/Dropbox/GTD/gtd.org"</span> <span style="color: #8b2252;">"Tasks"</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"* ACTION %^{Description}  %^G\nSCHEDULED:%(org-insert-time-stamp (org-read-date nil t \".+1d\"))%? \n %i \n %l"</span><span style="color: #707183;">)</span> 
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"th"</span> <span style="color: #8b2252;">"History"</span> entry <span style="color: #707183;">(</span>file+olp <span style="color: #8b2252;">"~/Dropbox/GTD/gtd.org"</span> <span style="color: #8b2252;">"Tasks"</span> <span style="color: #8b2252;">"History Dept"</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"* ACTION %^{Description}  %^G\nSCHEDULED:%(org-insert-time-stamp (org-read-date nil t \".+1d\"))%?"</span><span style="color: #707183;">)</span> 
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"b"</span> <span style="color: #8b2252;">"Bookmarks"</span> entry <span style="color: #707183;">(</span>file+olp <span style="color: #8b2252;">"~/Dropbox/GTD/Reference.org"</span> <span style="color: #8b2252;">"Links"</span> <span style="color: #707183;">)</span> <span style="color: #8b2252;">"*  %:description  %^G\nSOURCE: %u, %c\n\n  %i"</span><span style="color: #707183;">)</span> 

        <span style="color: #707183;">(</span><span style="color: #8b2252;">"j"</span> <span style="color: #8b2252;">"Journal"</span> entry <span style="color: #707183;">(</span>file+datetree <span style="color: #8b2252;">"~/Dropbox/GTD/Reference.org"</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"* %?</span>
<span style="color: #8b2252;">Entered on %U</span>
<span style="color: #8b2252;">  %i</span>
<span style="color: #8b2252;">  %a"</span><span style="color: #707183;">)</span> 
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"a"</span> <span style="color: #8b2252;">"Appointments"</span> entry <span style="color: #707183;">(</span>file+headline <span style="color: #8b2252;">"~/Dropbox/GTD/diary.org"</span> <span style="color: #8b2252;">"Appointments"</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"* Appointment: %^{Desciption} %^G\nSCHEULED:%t"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">))</span>

<span style="color: #b22222;">;;; </span><span style="color: #b22222;">REFILING</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Use IDO for target completion</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(setq org-completion-use-ido t)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Targets include this file and any file contributing to the agenda - up to 5 levels deep</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-refile-targets <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">((</span>org-agenda-files <span style="color: #483d8b;">:maxlevel</span> . 5<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>nil <span style="color: #483d8b;">:maxlevel</span> . 5<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #8b2252;">"/home/matt/org/.org2blog.el"</span> <span style="color: #483d8b;">:maxlevel</span> . 1<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #8b2252;">"/home/matt/Dropbox/Work/History/HackingHistory/Grades.org"</span> <span style="color: #483d8b;">:maxlevel</span> . 5<span style="color: #707183;">))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Targets start with the file name - allows creating level 1 tasks</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-refile-use-outline-path <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> file<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Targets complete in steps so we start with filename, TAB shows the next level of targets etc</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-outline-path-complete-in-steps t<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Allow refile to create parent tasks with confirmation</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-refile-allow-creating-parent-nodes <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> confirm<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">stuff from http://www.jboecker.de/2010/04/14/general-reference-filing-with-org-mode.html#sec-1 </span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">for org-mode remember integration</span>

<span style="color: #b22222;">;;; </span><span style="color: #b22222;">Agenda Commands</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">customized agenda commiands</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-agenda-custom-commands
      '<span style="color: #707183;">((</span><span style="color: #8b2252;">"g"</span> . <span style="color: #8b2252;">"GTD contexts"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"ge"</span> <span style="color: #8b2252;">"email"</span> tags-todo <span style="color: #8b2252;">"email/+ACTION"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"gc"</span> <span style="color: #8b2252;">"Computer"</span> tags-todo <span style="color: #8b2252;">"computer/+ACTION"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"go"</span> <span style="color: #8b2252;">"Office"</span> tags-todo <span style="color: #8b2252;">"office/+ACTION"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"gp"</span> <span style="color: #8b2252;">"Phone"</span> tags-todo <span style="color: #8b2252;">"phone/+ACTION"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"gh"</span> <span style="color: #8b2252;">"Home"</span> tags-todo <span style="color: #8b2252;">"home/+ACTION"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"gr"</span> <span style="color: #8b2252;">"Errands"</span> tags-todo <span style="color: #8b2252;">"errand/+ACTION"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"G"</span> <span style="color: #8b2252;">"GTD Block Agenda"</span>
         <span style="color: #707183;">((</span>tags-todo <span style="color: #8b2252;">"phone/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"office/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"email/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"computer/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"home/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"errand/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"-phone-office-email-computer-home-office/+ACTION"</span><span style="color: #707183;">))</span>
         nil                      <span style="color: #b22222;">;; </span><span style="color: #b22222;">i.e., no local settings</span>
         <span style="color: #707183;">(</span><span style="color: #8b2252;">"~/next-actions.html"</span><span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">exports block to this file with C-c a e</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"T"</span> <span style="color: #8b2252;">"Teaching Block Agenda"</span>
         <span style="color: #707183;">((</span>tags-todo <span style="color: #8b2252;">"phone+teaching/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"office+teaching/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"email+teaching/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"computer+teaching/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"home+teaching/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"errand+teaching/+ACTION"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"-phone-office-email-computer-home-office+teaching/+ACTION"</span><span style="color: #707183;">))</span>
         nil                      <span style="color: #b22222;">;; </span><span style="color: #b22222;">i.e., no local settings</span>
         <span style="color: #707183;">(</span><span style="color: #8b2252;">"~/next-actions.html"</span><span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">exports block to this file with C-c a e</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"H"</span> <span style="color: #8b2252;">"GTD Block Agenda"</span>
         <span style="color: #707183;">((</span>tags-todo <span style="color: #8b2252;">"+history+phone/+ACTION|BLOCKED"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"+history+office/+ACTION|BLOCKED"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"+history+email/+ACTION|BLOCKED"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"+history+computer/+ACTION|BLOCKED"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"+history+home/+ACTION+|BLOCKED"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"+history+errand/+ACTION|BLOCKED"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"+history-phone-office-email-computer-home-office/+ACTION|BLOCKED"</span><span style="color: #707183;">))</span>
         nil                      <span style="color: #b22222;">;; </span><span style="color: #b22222;">i.e., no local settings</span>
         <span style="color: #707183;">(</span><span style="color: #8b2252;">"~/history-next-actions.html"</span><span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">exports block to this file with C-c a e</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"W"</span> <span style="color: #8b2252;">"WAITING block Agenda"</span>
         <span style="color: #707183;">((</span>tags-todo <span style="color: #8b2252;">"phone/+WAITING"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"email/+WAITING"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"computer/+WAITING"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"office/+WAITING"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"home/+WAITING"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>tags-todo <span style="color: #8b2252;">"errand/+WAITING"</span><span style="color: #707183;">))</span>
         nil                      <span style="color: #b22222;">;; </span><span style="color: #b22222;">i.e., no local settings</span>
         <span style="color: #707183;">(</span><span style="color: #8b2252;">"~/next-actions.html"</span><span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">exports block to this file with C-c a e</span>
       <span style="color: #b22222;">;; </span><span style="color: #b22222;">..other commands here</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"t"</span> agenda <span style="color: #8b2252;">"Teaching Agenda"</span>
         <span style="color: #707183;">((</span> org-agenda-filter-preset '<span style="color: #707183;">(</span><span style="color: #8b2252;">"+teaching"</span><span style="color: #707183;">)</span> <span style="color: #707183;">)</span>  <span style="color: #707183;">)</span> <span style="color: #707183;">)</span>
         <span style="color: #707183;">(</span><span style="color: #8b2252;">"p"</span> <span style="color: #8b2252;">"Projects"</span> todo <span style="color: #8b2252;">"PROJECT"</span><span style="color: #707183;">)</span>
         <span style="color: #707183;">))</span>

<span style="color: #b22222;">;;; </span><span style="color: #b22222;">agenda diary stuff</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-agenda-diary-file <span style="color: #8b2252;">"~/Dropbox/GTD/diary.org"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-agenda-include-diary t<span style="color: #707183;">)</span>


<span style="color: #b22222;">;;; </span><span style="color: #b22222;">still more agenda</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defadvice</span> <span style="color: #0000ff;">org-agenda-add-entry-to-org-agenda-diary-file</span>
    <span style="color: #707183;">(</span>after add-to-google-calendar<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Add a new Google calendar entry that mirrors the diary entry just created by</span>
<span style="color: #8b2252;">org-mode."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>type <span style="color: #707183;">(</span>ad-get-arg 0<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>text <span style="color: #707183;">(</span>ad-get-arg 1<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>d1 <span style="color: #707183;">(</span>ad-get-arg 2<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>year1 <span style="color: #707183;">(</span>nth 2 d1<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>month1 <span style="color: #707183;">(</span>car d1<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>day1 <span style="color: #707183;">(</span>nth 1 d1<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>d2 <span style="color: #707183;">(</span>ad-get-arg 3<span style="color: #707183;">))</span>
        entry dates<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span>not <span style="color: #707183;">(</span>eq type 'block<span style="color: #707183;">))</span> <span style="color: #707183;">(</span>not d2<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> dates <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"%d-%02d-%02d"</span> year1 month1 day1<span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>year2 <span style="color: #707183;">(</span>nth 2 d2<span style="color: #707183;">))</span> <span style="color: #707183;">(</span>month2 <span style="color: #707183;">(</span>car d2<span style="color: #707183;">))</span> <span style="color: #707183;">(</span>day2 <span style="color: #707183;">(</span>nth 1 d2<span style="color: #707183;">))</span> <span style="color: #707183;">(</span>repeats <span style="color: #707183;">(</span>-
                                                                             <span style="color: #707183;">(</span>calendar-absolute-from-gregorian d1<span style="color: #707183;">)</span>

                                                                             <span style="color: #707183;">(</span>calendar-absolute-from-gregorian d2<span style="color: #707183;">))))</span>
        <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>&gt; repeats 0<span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> dates <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"%d-%02d-%02d every day for %d days"</span> year1
                                month1 day1 <span style="color: #707183;">(</span>abs repeats<span style="color: #707183;">)))</span>
          <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> dates <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"%d-%02d-%02d every day for %d days"</span> year1 month1
                              day1 <span style="color: #707183;">(</span>abs repeats<span style="color: #707183;">))))</span>
        <span style="color: #707183;">))</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> entry <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"/usr/bin/google calendar add --cal org \"%s on %s\""</span> text dates<span style="color: #707183;">))</span>
    <span style="color: #707183;">(</span>message entry<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>not <span style="color: #707183;">(</span>string= <span style="color: #8b2252;">"MYLAPTOPCOMPUTER"</span> mail-host-address<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>shell-command entry<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>offline <span style="color: #8b2252;">"~/tmp/org2google-offline-entries"</span><span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>find-file offline<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>goto-char <span style="color: #707183;">(</span>point-max<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>insert <span style="color: #707183;">(</span>concat entry <span style="color: #8b2252;">"\n"</span><span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>save-buffer<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>kill-buffer <span style="color: #707183;">(</span>current-buffer<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"Plain text written to %s"</span> offline<span style="color: #707183;">)))))</span>
<span style="color: #707183;">(</span>ad-activate 'org-agenda-add-entry-to-org-agenda-diary-file<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline23" class="outline-3">
    <h3 id="orgheadline23">
      Working with Macros
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline23">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;; </span><span style="color: #b22222;">macros</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">mwp/org-insert-example</span>
<span style="color: #707183;">(</span>fset 'mwp/org-insert-example
   <span style="color: #707183;">[</span>?# ?+ ?B ?E ?G ?I ?N ?_ ?E ?X ?A ?M ?P ?L ?E return return ?# ?+ ?E ?N ?D ?_ ?E ?X ?A ?M ?P ?L ?E up<span style="color: #707183;">])</span>

<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c M-2"</span><span style="color: #707183;">)</span> 'mwp/org-insert-example<span style="color: #707183;">)</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">mwp/org-insert-quote</span>
<span style="color: #707183;">(</span>fset 'mwp/org-insert-quote
   <span style="color: #707183;">[</span>?# ?+ ?B ?E ?G ?I ?N ?_ ?Q ?U ?O ?T ?E return return ?# ?+ ?E ?N ?D ?_ ?Q ?U ?O ?T ?E up<span style="color: #707183;">])</span>

<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c M-1"</span><span style="color: #707183;">)</span> 'mwp/org-insert-quote<span style="color: #707183;">)</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">mwp/org-insert-iframe</span>
<span style="color: #707183;">(</span>fset 'mwp/org-insert-iframe
   <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> arg<span style="color: #707183;">)</span> <span style="color: #8b2252;">"Keyboard macro."</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"p"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>kmacro-exec-ring-item <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">([</span>35 43 98 101 103 105 110 95 104 116 109 108 return 60 105 102 114 97 109 101 32 119 105 100 116 104 61 34 56 48 48 112 120 right 32 104 101 105 103 104 116 61 34 52 53 48 112 120 right 32 115 114 99 61 34 right 62 60 47 105 102 114 97 109 101 62 return 35 61 backspace 43 101 110 100 95 104 116 109 108 return up left left left left left left left left left left left left<span style="color: #707183;">]</span> 0 <span style="color: #8b2252;">"%d"</span><span style="color: #707183;">))</span> arg<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(global-set-key (kbd "C-c M-0") 'mwp/org-insert-iframe)</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">save a macro</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">keyboard macro function</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">save-macro</span> <span style="color: #707183;">(</span>name<span style="color: #707183;">)</span>                  
  <span style="color: #8b2252;">"save a macro. Take a name as argument</span>
<span style="color: #8b2252;">     and save the last defined macro under </span>
<span style="color: #8b2252;">     this name at the end of your .emacs"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"SName of the macro :"</span><span style="color: #707183;">)</span>  <span style="color: #b22222;">; </span><span style="color: #b22222;">ask for the name of the macro    </span>
  <span style="color: #707183;">(</span>kmacro-name-last-macro name<span style="color: #707183;">)</span>         <span style="color: #b22222;">; </span><span style="color: #b22222;">use this name for the macro    </span>
  <span style="color: #707183;">(</span>find-file <span style="color: #8b2252;">"/home/matt/.emacs.d/organize-my-life.el"</span><span style="color: #707183;">)</span>                   <span style="color: #b22222;">; </span><span style="color: #b22222;">open ~/.emacs or other user init file </span>
  <span style="color: #707183;">(</span>goto-char <span style="color: #707183;">(</span>point-max<span style="color: #707183;">))</span>               <span style="color: #b22222;">; </span><span style="color: #b22222;">go to the end of the .emacs</span>
  <span style="color: #707183;">(</span>newline<span style="color: #707183;">)</span>                             <span style="color: #b22222;">; </span><span style="color: #b22222;">insert a newline</span>
  <span style="color: #707183;">(</span>insert-kbd-macro name<span style="color: #707183;">)</span>               <span style="color: #b22222;">; </span><span style="color: #b22222;">copy the macro </span>
  <span style="color: #707183;">(</span>newline<span style="color: #707183;">)</span>                             <span style="color: #b22222;">; </span><span style="color: #b22222;">insert a newline</span>
  <span style="color: #707183;">(</span>switch-to-buffer nil<span style="color: #707183;">))</span>               <span style="color: #b22222;">; </span><span style="color: #b22222;">return to the initial buffer</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Keyboard macro to insert quotes. not bound yet to anything</span>
<span style="color: #707183;">(</span>fset 'insert_quote

      <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> arg<span style="color: #707183;">)</span> <span style="color: #8b2252;">"Keyboard macro."</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"p"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>kmacro-exec-ring-item <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">([</span>35 43 66 69 71 73 78 95 81 85 79 84 69 return return 35 43 69 78 68 95 81 85 79 84 69 up<span style="color: #707183;">]</span> 0 <span style="color: #8b2252;">"%d"</span><span style="color: #707183;">))</span> arg<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span>fset 'mwp/org-insert-js
   <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> arg<span style="color: #707183;">)</span> <span style="color: #8b2252;">"Keyboard macro."</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"p"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>kmacro-exec-ring-item <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">([</span>35 43 66 69 71 73 78 95 83 82 67 32 108 97 110 103 117 97 103 101 61 106 97 118 97 115 99 114 105 112 116 return return 35 43 69 78 68 95 83 82 67 up<span style="color: #707183;">]</span> 0 <span style="color: #8b2252;">"%d"</span><span style="color: #707183;">))</span> arg<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span>fset 'mwp/org-insert-html
   <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> arg<span style="color: #707183;">)</span> <span style="color: #8b2252;">"Keyboard macro."</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"p"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>kmacro-exec-ring-item <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">([</span>35 43 66 69 71 73 78 95 83 82 67 32 104 116 109 108 13 13 35 43 69 78 68 95 83 82 67 up<span style="color: #707183;">]</span> 0 <span style="color: #8b2252;">"%d"</span><span style="color: #707183;">))</span> arg<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;;; </span><span style="color: #b22222;">fix html export xml declaration so OOo can read it</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-export-html-xml-declaration
      '<span style="color: #707183;">((</span><span style="color: #8b2252;">"html"</span> . <span style="color: #8b2252;">""</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"php"</span> . <span style="color: #8b2252;">"&lt;?php echo \"&lt;?xml version=\\\"1.0\\\" encoding=\\\"%s\\\" ?&gt;\";</span>
<span style="color: #8b2252;">?&gt;"</span><span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span>fset 'marking-template
   <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> arg<span style="color: #707183;">)</span> <span style="color: #8b2252;">"Keyboard macro."</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"p"</span><span style="color: #707183;">)</span> <span style="color: #707183;">(</span>kmacro-exec-ring-item <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">([</span>42 42 32 return 124 32 79 114 103 97 110 105 122 97 116 105 111 110 32 124 32 124 32 backspace tab 67 108 97 114 105 116 121 32 111 102 32 84 104 101 115 105 115 tab tab 80 114 101 115 101 110 116 97 116 105 111 110 32 111 102 32 69 118 105 100 101 110 99 101 32 124 32 124 backspace backspace backspace backspace tab tab 71 114 97 109 109 97 114 32 97 110 100 32 83 112 101 108 108 105 110 103 tab tab 83 116 121 108 101 tab tab 67 105 116 97 116 105 111 110 115 tab tab 70 117 114 116 104 101 114 32 67 111 109 109 101 110 116 115 tab tab 71 114 97 100 101 tab up up up up up up up up 32<span style="color: #707183;">]</span> 0 <span style="color: #8b2252;">"%d"</span><span style="color: #707183;">))</span> arg<span style="color: #707183;">)))</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline24" class="outline-3">
    <h3 id="orgheadline24">
      {df}
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline24">
      <table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
        <colgroup> <col class="org-left" /> <col class="org-left" /> </colgroup> <tr>
          <td class="org-left">
            Organization
          </td>
          
          <td class="org-left">
            {d}
          </td>
        </tr>
        
        <tr>
          <td class="org-left">
            Clarity of thesis
          </td>
          
          <td class="org-left">
            {d}
          </td>
        </tr>
        
        <tr>
          <td class="org-left">
            Presentation of Evidence
          </td>
          
          <td class="org-left">
            {d}
          </td>
        </tr>
        
        <tr>
          <td class="org-left">
            Grammar and Spelling
          </td>
          
          <td class="org-left">
            {d}
          </td>
        </tr>
        
        <tr>
          <td class="org-left">
            Style
          </td>
          
          <td class="org-left">
            {d}
          </td>
        </tr>
        
        <tr>
          <td class="org-left">
            Citations
          </td>
          
          <td class="org-left">
            {d}
          </td>
        </tr>
        
        <tr>
          <td class="org-left">
            Further Comments
          </td>
          
          <td class="org-left">
            {d}
          </td>
        </tr>
        
        <tr>
          <td class="org-left">
            Grade:
          </td>
          
          <td class="org-left">
            {dfd}
          </td>
        </tr>
      </table>
    </div>
  </div>
  
  <div id="outline-container-orgheadline36" class="outline-3">
    <h3 id="orgheadline36">
      Exporting and Publishing
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline36">
    </div>
    
    <div id="outline-container-orgheadline25" class="outline-4">
      <h4 id="orgheadline25">
        General
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline25">
        <p>
          Some general options and fixes
        </p>
        
        <div class="org-src-container">
          <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;;; </span><span style="color: #b22222;">export options for org-mode</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-export-with-section-numbers nil
      org-export-with-toc nil
      org-export-preserve-breaks nil
      org-export-email-info nil
<span style="color: #707183;">)</span>
<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">Timestamps in Exports</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">removing annoying brackets from timestamp on html export</span>
<span style="color: #707183;">(</span>add-to-list 'org-export-filter-timestamp-functions 'matt-org-export-filter-timestamp-function<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">matt-org-export-filter-timestamp-function</span> <span style="color: #707183;">(</span>timestamp backend info<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"removes relevant brackets from a timestamp"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>org-export-derived-backend-p backend 'html<span style="color: #707183;">)</span> 
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">unfortunatley I can't make emacs regexps work yet.  sigh.  </span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">(replace-regexp-in-string "[][]" "" timestamp)</span>
    <span style="color: #707183;">(</span>replace-regexp-in-string <span style="color: #8b2252;">"&[lg]t;</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">[][]"</span> <span style="color: #8b2252;">""</span> timestamp<span style="color: #707183;">)</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">(replace-regexp-in-string "&lt;" "" timestamp)</span>
<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">removing annoying brackets from timestamp on html export</span>
<span style="color: #707183;">(</span>add-to-list 'org-export-filter-paragraph-functions 'matt-org-export-filter-paragraph-function<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">matt-org-export-filter-paragraph-function</span> <span style="color: #707183;">(</span>paragraph backend info<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"removes comments from export"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>org-export-derived-backend-p backend 'html<span style="color: #707183;">)</span> 
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">unfortunatley I can't make emacs regexps work yet.  sigh.  </span>
    <span style="color: #707183;">(</span>replace-regexp-in-string <span style="color: #8b2252;">"^#\+.*$"</span> <span style="color: #8b2252;">""</span> paragraph<span style="color: #707183;">)</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">(replace-regexp-in-string "&lt;" "" paragraph)</span>
<span style="color: #707183;">))</span>
</pre>
        </div>
      </div>
    </div>
    
    <div id="outline-container-orgheadline26" class="outline-4">
      <h4 id="orgheadline26">
        <a id="o2b:cef9afbb-5bec-4b01-a32a-783315ffc727"></a>Creating and Publishing Presentations with Org-reveal
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline26">
        <p>
          For several years, I&rsquo;ve been using Org-mode to compose slides for my lectures. This method is great, because I get to work in plain-text and focus on the content of my lectures rather than animations; but it&rsquo;s meant that when I want to share my presentations with others, there&rsquo;s a certain amount of work involved as I move from a local copy on my computer to a web-based version. (This has largely been an issue because I sometimes <i>compose</i> my lectures sitting in a café with lousy Internet, and I sometimes <i>give</i> my lectures in a horrible classroom at U of T with terrible Internet reception.) I&rsquo;ve now largely solved this problem, though there is hopefully an improvement coming down the pipe which will make it even easier.
        </p>
        
        <p>
          Org-mode has the capacity to export to a number of slide-like formats, including the <a href="http://orgmode.org/worg/exporters/beamer/beamer-dual-format.html">LaTeX-based Beamer format</a>, which also makes good PDF presentations, a couple of Emacs-based presentation tools, and a number of HTML5 formats. Since I teach about the web all the time, the HTML5 formats have always been the most appealing to me.
        </p>
      </div>
      
      <ul class="org-ul">
        <li>
          <a id="orgheadline27"></a>Org-Reveal Setup<br /> <div class="outline-text-5" id="text-orgheadline27">
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
<span style="color: #707183;">(</span>add-to-list 'load-path <span style="color: #8b2252;">"~/src/org-reveal"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-reveal</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">set local root</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-reveal-root <span style="color: #8b2252;">"file:///home/matt/src/reveal.js"</span><span style="color: #707183;">)</span>
</pre>
            </div>
            
            <p>
              That&rsquo;s all that&rsquo;s needed to get export working! I find it&rsquo;s really fast to prepare lectures.
            </p>
          </div>
        </li>
        
        <li>
          <a id="orgheadline28"></a>Publishing<br /> <div class="outline-text-5" id="text-orgheadline28">
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
              <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-publish-project-alist
      '<span style="color: #707183;">(</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"courses"</span>
         <span style="color: #483d8b;">:components</span> <span style="color: #707183;">(</span><span style="color: #8b2252;">"dh"</span> <span style="color: #8b2252;">"rlg231"</span><span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"rlg231"</span>
         <span style="color: #483d8b;">:components</span> <span style="color: #707183;">(</span><span style="color: #8b2252;">"rlg231-lecture-slides"</span> <span style="color: #8b2252;">"rlg231-lecture-source"</span><span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"dh"</span>
         <span style="color: #483d8b;">:components</span> <span style="color: #707183;">(</span><span style="color: #8b2252;">"digital-history-lecture-slides"</span> <span style="color: #8b2252;">"digital-history-lecture-source"</span><span style="color: #707183;">))</span>

        <span style="color: #707183;">(</span><span style="color: #8b2252;">"rlg231-lecture-slides"</span>
         <span style="color: #483d8b;">:base-directory</span> <span style="color: #8b2252;">"~/RLG231/Lectures/"</span>
         <span style="color: #483d8b;">:base-extension</span> <span style="color: #8b2252;">"org"</span>
         <span style="color: #483d8b;">:publishing-directory</span> <span style="color: #8b2252;">"/ssh:matt@shimano:/var/www/sandbox/RLG231/Lectures/Slides"</span>
         <span style="color: #483d8b;">:recursive</span> t
         <span style="color: #483d8b;">:publishing-function</span> mwp-org-reveal-publish-to-html
         <span style="color: #483d8b;">:headline-levels</span> 4             <span style="color: #b22222;">; </span><span style="color: #b22222;">Just the default for this project.</span>
         <span style="color: #483d8b;">:exclude</span> <span style="color: #8b2252;">"LectureOutlines.org"</span>
         <span style="color: #483d8b;">:exclude-tags</span> note noexport
         <span style="color: #483d8b;">:auto-preamble</span> t<span style="color: #707183;">)</span>

        <span style="color: #707183;">(</span><span style="color: #8b2252;">"rlg231-lecture-source"</span>
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
         <span style="color: #483d8b;">:auto-preamble</span> t<span style="color: #707183;">)</span>

        <span style="color: #707183;">(</span><span style="color: #8b2252;">"digital-history-lecture-source"</span>
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
         <span style="color: #483d8b;">:auto-preamble</span> t<span style="color: #707183;">)</span>

        <span style="color: #707183;">(</span><span style="color: #8b2252;">"digital-history-lecture-slides"</span>
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
         <span style="color: #483d8b;">:auto-preamble</span> t<span style="color: #707183;">)</span>

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

      <span style="color: #707183;">))</span>
</pre>
            </div>
            
            <p>
              Notice the publishing function, which is set to <code>mwp-org-deck-publish-to-html</code>. This is a simple function that resets the base url and <code>extra-css</code> values to web-based ones before publication, so that the presentations work when online. Notice I&rsquo;ve also reset the <code>deck.js</code> base url, in case I ever decide to change back to deck.
            </p>
            
            <div class="org-src-container">
              <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-reveal-publish-to-html</span> <span style="color: #707183;">(</span>plist filename pub-dir<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Publish an org file to reveal.js HTML Presentation.</span>
<span style="color: #8b2252;">FILENAME is the filename of the Org file to be published.  PLIST</span>
<span style="color: #8b2252;">is the property list for the given project.  PUB-DIR is the</span>
<span style="color: #8b2252;">publishing directory. Returns output file name."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>org-deck-base-url <span style="color: #8b2252;">"http://sandbox.hackinghistory.ca/Tools/deck.js/"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>org-reveal-root <span style="color: #8b2252;">"http://sandbox.hackinghistory.ca/Tools/reveal.js/"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>org-reveal-extra-css <span style="color: #8b2252;">"http://sandbox.hackinghistory.ca/Tools/reveal.js/css/local.css"</span><span style="color: #707183;">))</span>

    <span style="color: #707183;">(</span>org-publish-org-to 'reveal filename <span style="color: #8b2252;">".html"</span> plist pub-dir<span style="color: #707183;">))</span>
  <span style="color: #707183;">)</span>
</pre>
            </div>
            
            <p>
              And that&rsquo;s it, magic!
            </p>
          </div>
        </li>
        
        <li>
          <a id="orgheadline29"></a>Still to do<br /> <div class="outline-text-5" id="text-orgheadline29">
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
        </li>
        
        <li>
          <a id="orgheadline30"></a>See my slides<br /> <div class="outline-text-5" id="text-orgheadline30">
            <p>
              If you want to see some examples of the end product, <a href="http://sandbox.hackinghistory.ca/DigitalHistory/Lectures/">here is a link to my Digital History lecture archive</a> (still being built!). Many of my course materials are also <a href="https://github.com/titaniumbones?tab=repositories">online at Github</a>.
            </p>
          </div>
        </li>
      </ul>
    </div>
    
    <div id="outline-container-orgheadline31" class="outline-4">
      <h4 id="orgheadline31">
        Modifying Reveal
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline31">
        <p>
          This never actually worked, but here it is in case it&rsquo;s somehow doing something at hteo moment&#x2026;
        </p>
        
        <div class="org-src-container">
          <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-reveal-scripts</span> <span style="color: #707183;">(</span>info<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Return the necessary scripts for initializing reveal.js using</span>
<span style="color: #8b2252;">custom variable `</span><span style="color: #008b8b;">org-reveal-root</span><span style="color: #8b2252;">'."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>root-path <span style="color: #707183;">(</span>file-name-as-directory <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-root</span><span style="color: #707183;">))))</span>
    <span style="color: #707183;">(</span>concat
     <span style="color: #b22222;">;; </span><span style="color: #b22222;">reveal.js/lib/js/head.min.js</span>
     <span style="color: #b22222;">;; </span><span style="color: #b22222;">reveal.js/js/reveal.js</span>
     <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"</span>
<span style="color: #8b2252;">&lt;script src=\"%slib/js/head.min.js\"&gt;&lt;/script&gt;</span>
<span style="color: #8b2252;">&lt;script src=\"%sjs/reveal.js\"&gt;&lt;/script&gt;</span>
<span style="color: #8b2252;">"</span>
             root-path root-path<span style="color: #707183;">)</span>
     <span style="color: #b22222;">;; </span><span style="color: #b22222;">plugin headings</span>
     <span style="color: #8b2252;">"</span>
<span style="color: #8b2252;">&lt;script&gt;</span>
<span style="color: #8b2252;">// Full list of configuration options available here:</span>
<span style="color: #8b2252;">// https://github.com/hakimel/reveal.js#configuration</span>
<span style="color: #8b2252;">Reveal.initialize({</span>
<span style="color: #8b2252;">"</span>
     <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"</span>
<span style="color: #8b2252;">controls: %s,</span>
<span style="color: #8b2252;">progress: %s,</span>
<span style="color: #8b2252;">history: %s,</span>
<span style="color: #8b2252;">center: %s,</span>
<span style="color: #8b2252;">slideNumber: %s,</span>
<span style="color: #8b2252;">rollingLinks: %s,</span>
<span style="color: #8b2252;">keyboard: %s,</span>
<span style="color: #8b2252;">overview: %s,</span>
<span style="color: #8b2252;">"</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-control</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-progress</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-history</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-center</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-slide-number</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-rolling-links</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-keyboard</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-overview</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"true"</span> <span style="color: #8b2252;">"false"</span><span style="color: #707183;">))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">slide width</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>width <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-width</span><span style="color: #707183;">)))</span>
       <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>&gt; width 0<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"width: %d,\n"</span> width<span style="color: #707183;">)</span> <span style="color: #8b2252;">""</span><span style="color: #707183;">))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">slide height</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>height <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-height</span><span style="color: #707183;">)))</span>
       <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>&gt; height 0<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"height: %d,\n"</span> height<span style="color: #707183;">)</span> <span style="color: #8b2252;">""</span><span style="color: #707183;">))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">slide margin</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>margin <span style="color: #707183;">(</span>string-to-number <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-margin</span><span style="color: #707183;">))))</span>
       <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>&gt;= margin 0<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"margin: %.2f,\n"</span> margin<span style="color: #707183;">)</span> <span style="color: #8b2252;">""</span><span style="color: #707183;">))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">slide minimum scaling factor</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>min-scale <span style="color: #707183;">(</span>string-to-number <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-min-scale</span><span style="color: #707183;">))))</span>
       <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>&gt; min-scale 0<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"minScale: %.2f,\n"</span> min-scale<span style="color: #707183;">)</span> <span style="color: #8b2252;">""</span><span style="color: #707183;">))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">slide maximux scaling factor</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>max-scale <span style="color: #707183;">(</span>string-to-number <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-max-scale</span><span style="color: #707183;">))))</span>
       <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>&gt; max-scale 0<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"maxScale: %.2f,\n"</span> max-scale<span style="color: #707183;">)</span> <span style="color: #8b2252;">""</span><span style="color: #707183;">))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">thems and transitions</span>
     <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"</span>
<span style="color: #8b2252;">theme: Reveal.getQueryHash().theme, // available themes are in /css/theme</span>
<span style="color: #8b2252;">transition: Reveal.getQueryHash().transition || '%s', // default/cube/page/concave/zoom/linear/fade/none</span>
<span style="color: #8b2252;">transitionSpeed: '%s',\n"</span>
             <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-trans</span><span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-speed</span><span style="color: #707183;">))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">multiplexing - depends on defvar 'client-multiplex'</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-multiplex-id</span><span style="color: #707183;">)</span>
       <span style="color: #707183;">(</span>format
        <span style="color: #8b2252;">"multiplex: {</span>
<span style="color: #8b2252;">    secret: %s, // null if client</span>
<span style="color: #8b2252;">    id: '%s', // id, obtained from socket.io server</span>
<span style="color: #8b2252;">    url: '%s' // Location of socket.io server</span>
<span style="color: #8b2252;">},\n"</span>
        <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>eq client-multiplex nil<span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"'%s'"</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-multiplex-secret</span><span style="color: #707183;">))</span>
          <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"null"</span><span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-multiplex-id</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-multiplex-url</span><span style="color: #707183;">)))</span>

     <span style="color: #b22222;">;; </span><span style="color: #b22222;">optional JS library heading</span>
     <span style="color: #8b2252;">"</span>
<span style="color: #8b2252;">// Optional libraries used to extend on reveal.js</span>
<span style="color: #8b2252;">dependencies: [</span>
<span style="color: #8b2252;">"</span>
     <span style="color: #b22222;">;; </span><span style="color: #b22222;">JS libraries</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>builtins
             '<span style="color: #707183;">(</span>classList <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%slib/js/classList.js', condition: function() { return !document.body.classList; } }"</span> root-path<span style="color: #707183;">)</span>
                         markdown <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/markdown/marked.js', condition: function() { return !!document.querySelector( '[data-markdown]' ); } },</span>
<span style="color: #8b2252;"> { src: '%splugin/markdown/markdown.js', condition: function() { return !!document.querySelector( '[data-markdown]' ); } }"</span> root-path root-path<span style="color: #707183;">)</span>
                         highlight <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/highlight/highlight.js', async: true, callback: function() { hljs.initHighlightingOnLoad(); } }"</span> root-path<span style="color: #707183;">)</span>
                         zoom <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/zoom-js/zoom.js', async: true, condition: function() { return !!document.body.classList; } }"</span> root-path<span style="color: #707183;">)</span>
                         notes <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/notes/notes.js', async: true, condition: function() { return !!document.body.classList; } }"</span> root-path<span style="color: #707183;">)</span>
                         search <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/search/search.js', async: true, condition: function() { return !!document.body.classList; } }"</span> root-path<span style="color: #707183;">)</span>
                         remotes <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/remotes/remotes.js', async: true, condition: function() { return !!document.body.classList; } }"</span> root-path<span style="color: #707183;">)</span>
                         multiplex <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%s', async: true },\n%s"</span>
                                           <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-multiplex-socketio-url</span><span style="color: #707183;">)</span>
                                        <span style="color: #b22222;">; </span><span style="color: #b22222;">following ensures that either client.js or master.js is included depending on defva client-multiplex value state</span>
                                           <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>not client-multiplex<span style="color: #707183;">)</span>
                                               <span style="color: #707183;">(</span><span style="color: #a020f0;">progn</span>
                                                 <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-multiplex-secret</span><span style="color: #707183;">)</span>
                                                     <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> client-multiplex t<span style="color: #707183;">))</span>
                                                 <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/multiplex/master.js', async: true }"</span> root-path<span style="color: #707183;">))</span>

                                             <span style="color: #707183;">(</span>format <span style="color: #8b2252;">" { src: '%splugin/multiplex/client.js', async: true }"</span> root-path<span style="color: #707183;">)))))</span>
            <span style="color: #707183;">(</span>builtin-codes
             <span style="color: #707183;">(</span>mapcar
              <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>p<span style="color: #707183;">)</span>
                <span style="color: #707183;">(</span>eval <span style="color: #707183;">(</span>plist-get builtins p<span style="color: #707183;">)))</span>
              <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>buffer-plugins <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-plugins</span><span style="color: #707183;">)))</span>
                <span style="color: #707183;">(</span><span style="color: #a020f0;">cond</span>
                 <span style="color: #707183;">((</span>string= buffer-plugins <span style="color: #8b2252;">""</span><span style="color: #707183;">)</span> <span style="color: #707183;">())</span>
                 <span style="color: #707183;">(</span>buffer-plugins <span style="color: #707183;">(</span>car <span style="color: #707183;">(</span>read-from-string buffer-plugins<span style="color: #707183;">)))</span>
                 <span style="color: #707183;">(</span>t org-reveal-plugins<span style="color: #707183;">)))))</span>
            <span style="color: #707183;">(</span>extra-codes <span style="color: #707183;">(</span>plist-get info <span style="color: #483d8b;">:reveal-extra-js</span><span style="color: #707183;">))</span>
            <span style="color: #707183;">(</span>total-codes
             <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>string= <span style="color: #8b2252;">""</span> extra-codes<span style="color: #707183;">)</span> builtin-codes
               <span style="color: #707183;">(</span>append <span style="color: #707183;">(</span>list extra-codes<span style="color: #707183;">)</span> builtin-codes<span style="color: #707183;">))))</span>
       <span style="color: #707183;">(</span>mapconcat 'identity total-codes <span style="color: #8b2252;">",\n"</span><span style="color: #707183;">))</span>
     <span style="color: #8b2252;">"</span>
<span style="color: #8b2252;">]</span>
<span style="color: #8b2252;">});</span>
<span style="color: #8b2252;">&lt;/script&gt;\n"</span><span style="color: #707183;">)))</span>
</pre>
        </div>
      </div>
    </div>
    
    <div id="outline-container-orgheadline32" class="outline-4">
      <h4 id="orgheadline32">
        <a id="ID-3ac2a7c3-b4f5-4318-9477-5081fb955d21"></a>Org2Blog
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline32">
        <p>
          I have used org2blog for years, first to blog on my own, then when I stopped that, to post pretty much all my course content to websites for <a href="http://www.hackinghistory.ca/">Hacking History</a> and other courses such as <a href="http://digital.hackinghistory.ca/">Digital History</a>.
        </p>
        
        <p>
          Org2blog wil ltake a buffer or subtree and post it to any blog of your choosing, as long as the xmlrpc interface on that blog is open. You can start a new post with <code>M-x org2blog/wp-new-entry</code>, or call <code>org2blog/wp~post[buffer|subtree][-as-page][-and-publish]</code> from an existing buffer/subtree.
        </p>
        
        <p>
          This section defines my blogs (there are 4 right now, sometimes there are more, sometimes fewer), and has a couple of little defuns that make it easier to type some of the relevant functions. It also adds a hook to change some keybindings.
        </p>
        
        <p>
          There&rsquo;s way more info at <a href="https://github.com/punchagan/org2blog/">the Github repository page</a>.
        </p>
        
        <div class="org-src-container">
          <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;; </span><span style="color: #b22222;">Org2blog</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org2blog/wp-blog-alist
      '<span style="color: #707183;">((</span><span style="color: #8b2252;">"hh"</span>
         <span style="color: #483d8b;">:url</span> <span style="color: #8b2252;">"http://2014.hackinghistory.ca/xmlrpc.php"</span>
         <span style="color: #483d8b;">:username</span> <span style="color: #8b2252;">"matt"</span>
         <span style="color: #483d8b;">:default-title</span> <span style="color: #8b2252;">"Title"</span>
         <span style="color: #483d8b;">:default-categories</span> <span style="color: #707183;">(</span>nil<span style="color: #707183;">)</span>
         <span style="color: #483d8b;">:tags-as-categories</span> nil<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"rel"</span>
         <span style="color: #483d8b;">:url</span> <span style="color: #8b2252;">"http://relsci.hackinghistory.ca/xmlrpc.php"</span>
         <span style="color: #483d8b;">:username</span> <span style="color: #8b2252;">"matt"</span>
         <span style="color: #483d8b;">:default-title</span> <span style="color: #8b2252;">"Title"</span>
         <span style="color: #483d8b;">:default-categories</span> <span style="color: #707183;">(</span>nil<span style="color: #707183;">)</span>
         <span style="color: #483d8b;">:tags-as-categories</span> nil<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #8b2252;">"dig"</span>
         <span style="color: #483d8b;">:url</span> <span style="color: #8b2252;">"http://digital.hackinghistory.ca/xmlrpc.php"</span>
         <span style="color: #483d8b;">:username</span> <span style="color: #8b2252;">"matt"</span>
         <span style="color: #483d8b;">:default-title</span> <span style="color: #8b2252;">"Title"</span>
         <span style="color: #483d8b;">:default-categories</span> <span style="color: #707183;">(</span>nil<span style="color: #707183;">)</span>
         <span style="color: #483d8b;">:tags-as-categories</span> nil<span style="color: #707183;">)</span>
         <span style="color: #707183;">(</span><span style="color: #8b2252;">"matt"</span>
         <span style="color: #483d8b;">:url</span> <span style="color: #8b2252;">"http://matt.hackinghistory.ca/xmlrpc.php"</span>
         <span style="color: #483d8b;">:username</span> <span style="color: #8b2252;">"matt"</span>
         <span style="color: #483d8b;">:default-title</span> <span style="color: #8b2252;">""</span>
         <span style="color: #483d8b;">:default-categories</span> <span style="color: #707183;">(</span>nil<span style="color: #707183;">)</span>
         <span style="color: #483d8b;">:tags-as-categories</span> nil<span style="color: #707183;">)</span>
        <span style="color: #707183;">))</span>


<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">o2bnew</span> <span style="color: #707183;">()</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>org2blog/wp-new-entry<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">o2blin</span> <span style="color: #707183;">()</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>org2blog/wp-login<span style="color: #707183;">))</span>


<span style="color: #b22222;">;;; </span><span style="color: #b22222;">Org2blog</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">starting org2blog a little more easily</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">example of binding keys only when html-mode is active</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">O2B kebybindings</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">matt-org-mode-keys</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Modify keymaps used by `</span><span style="color: #008b8b;">org-mode</span><span style="color: #8b2252;">'."</span>
  <span style="color: #707183;">(</span>local-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c &lt;f1&gt;"</span><span style="color: #707183;">)</span> 'org2blog/wp-mode<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>local-set-key <span style="color: #8b2252;">"\C-c\C-r"</span> 'org-decrypt-entry<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>local-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"RET"</span><span style="color: #707183;">)</span> 'org-return-indent<span style="color: #707183;">)</span>
  <span style="color: #b22222;">;; </span><span style="color: #b22222;">insert a NOTES drawer with C-c C-x n</span>
<span style="color: #b22222;">;;  </span><span style="color: #b22222;">(local-set-key (kbd "C-c C-x n") (org-insert-drawer "NOTE"))</span>

  <span style="color: #b22222;">;; </span><span style="color: #b22222;">(local-set-key (kbd "C-c C-p") nil) ; remove a key</span>

  <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">add to org-mode-hook</span>
<span style="color: #707183;">(</span>add-hook 'org-mode-hook 'matt-org-mode-keys<span style="color: #707183;">)</span>
</pre>
        </div>
        
        <p>
          Here&rsquo;s a little treat from <a href="http://emacs.stackexchange.com/questions/2206/i-want-to-have-the-kbd-tags-for-my-blog-written-in-org-mode">Stack Exchange</a>. This function and accompanying keybinding let me insert a <kbd> tag very easily.
        </p>
        
        <div class="org-src-container">
          <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span>define-key org-mode-map <span style="color: #8b2252;">"\C-ck"</span> #'endless/insert-key<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">endless/insert-key</span> <span style="color: #707183;">(</span>key<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Ask for a key then insert its description.</span>
<span style="color: #8b2252;">Will work on both org-mode and any mode that accepts plain html."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"kType key sequence: "</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>is-org-mode <span style="color: #707183;">(</span>derived-mode-p 'org-mode<span style="color: #707183;">))</span>
         <span style="color: #707183;">(</span>tag <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> is-org-mode
                  <span style="color: #8b2252;">"@@html:&lt;kbd&gt;%s&lt;/kbd&gt;@@"</span>
                <span style="color: #8b2252;">"&lt;kbd&gt;%s&lt;/kbd&gt;"</span><span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>null <span style="color: #707183;">(</span>equal key <span style="color: #8b2252;">"\r"</span><span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>insert
         <span style="color: #707183;">(</span>format tag <span style="color: #707183;">(</span>help-key-description key nil<span style="color: #707183;">)))</span>
      <span style="color: #707183;">(</span>insert <span style="color: #707183;">(</span>format tag <span style="color: #8b2252;">""</span><span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span>forward-char <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> is-org-mode -8 -6<span style="color: #707183;">)))))</span>
</pre>
        </div>
      </div>
    </div>
    
    <div id="outline-container-orgheadline33" class="outline-4">
      <h4 id="orgheadline33">
        <a id="o2b:30e0ee49-9842-4cee-aff6-55283f9043c8"></a>Exporting org-files to a temporary location
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline33">
        <p>
          I have a private journal, which lives in an encrypted file in a Dropbox-backed-up directory. I use html export to examine the contents sometimes &#x2013; there are some big old tables that are hard to read in org-mode &#x2013; but I don&rsquo;t want the html file to end up in Dropbox.
        </p>
        
        <p>
          So I just copied the definition of org-export-html-as-html and made trivial modifications. There&rsquo;s probably a better way to do this.
        </p>
        
        <div class="org-src-container">
          <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">export html to tmp dir</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-html-to-tmp</span>
    <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> async subtreep visible-only body-only ext-plist<span style="color: #707183;">)</span>
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
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>extension <span style="color: #707183;">(</span>concat <span style="color: #8b2252;">"."</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span>plist-get ext-plist <span style="color: #483d8b;">:html-extension</span><span style="color: #707183;">)</span>
                                    org-html-extension
                                    <span style="color: #8b2252;">"html"</span><span style="color: #707183;">)))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this is the code I've changed from the original function. </span>
         <span style="color: #707183;">(</span>file <span style="color: #707183;">(</span>org-export-output-file-name extension subtreep <span style="color: #8b2252;">"/home/matt/tmp/"</span><span style="color: #707183;">))</span>

         <span style="color: #707183;">(</span>org-export-coding-system org-html-coding-system<span style="color: #707183;">))</span>
    <span style="color: #707183;">(</span>org-export-to-file 'html file
      async subtreep visible-only body-only ext-plist<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>org-open-file file<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span>org-defkey org-mode-map
            <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c 0"</span><span style="color: #707183;">)</span> 'mwp-org-html-to-tmp<span style="color: #707183;">)</span>
</pre>
        </div>
      </div>
    </div>
    
    <div id="outline-container-orgheadline34" class="outline-4">
      <h4 id="orgheadline34">
        Zotero!
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline34">
        <p>
          One of the few weaknesses that I see in Org compared to other platforms is citation handling, which until recently I really had no way of doing. But recently things have started to really get better, thanks to Erik Hetzner&rsquo;s <code>org-zotxt</code>. I&rsquo;ll blog more about this soon, because it&rsquo;s so awesome. Not quite finished yet though!
        </p>
        
        <div class="org-src-container">
          <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-zotxt</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">zotxt</span>
<span style="color: #707183;">(</span>org-add-link-type <span style="color: #8b2252;">"zotero"</span>
                   <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>rest<span style="color: #707183;">)</span>
                     <span style="color: #707183;">(</span>zotxt-select-key <span style="color: #707183;">(</span>substring rest 15<span style="color: #707183;">)))</span>
                   <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>path desc format<span style="color: #707183;">)</span>
                     <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>string-match <span style="color: #8b2252;">"^@</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">.*</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">$"</span> desc<span style="color: #707183;">)</span>
                         <span style="color: #707183;">(</span><span style="color: #a020f0;">cond</span> <span style="color: #707183;">((</span>eq format 'latex<span style="color: #707183;">)</span>
                                <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"\\cite{%s}"</span> <span style="color: #707183;">(</span>match-string 1 desc<span style="color: #707183;">)))</span>
                               <span style="color: #707183;">((</span>eq format 'md<span style="color: #707183;">)</span>
                                desc<span style="color: #707183;">)</span>
                               <span style="color: #707183;">((</span>eq format 'html<span style="color: #707183;">)</span>
                                <span style="color: #707183;">(</span><span style="color: #a020f0;">deferred:$</span>
                                  <span style="color: #707183;">(</span>zotxt-get-item-bibliography-deferred `<span style="color: #707183;">(</span><span style="color: #483d8b;">:key</span> , <span style="color: #707183;">(</span>substring path 15<span style="color: #707183;">)))</span>
                                  <span style="color: #707183;">(</span>deferred:nextc <span style="color: #a0522d;">it</span>
                                    <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>item<span style="color: #707183;">)</span>
                                      <span style="color: #707183;">(</span>plist-get item <span style="color: #483d8b;">:citation-html</span><span style="color: #707183;">)))</span>
                                  <span style="color: #707183;">(</span>deferred:sync! <span style="color: #a0522d;">it</span><span style="color: #707183;">)))</span>
                               <span style="color: #707183;">((</span>eq format 'odt<span style="color: #707183;">)</span>
                                <span style="color: #707183;">(</span>xml-escape-string <span style="color: #707183;">(</span><span style="color: #a020f0;">deferred:$</span>
                                                     <span style="color: #707183;">(</span>zotxt-get-item-deferred `<span style="color: #707183;">(</span><span style="color: #483d8b;">:key</span> , <span style="color: #707183;">(</span>substring path 15<span style="color: #707183;">))</span> <span style="color: #483d8b;">:248bebf1-46ab-4067-9f93-ec3d2960d0cd</span><span style="color: #707183;">)</span>
                                                     <span style="color: #707183;">(</span>deferred:nextc <span style="color: #a0522d;">it</span>
                                                       <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>item<span style="color: #707183;">)</span>
                                                         <span style="color: #707183;">(</span>plist-get item <span style="color: #483d8b;">:248bebf1-46ab-4067-9f93-ec3d2960d0cd</span><span style="color: #707183;">)))</span>
                                                     <span style="color: #707183;">(</span>deferred:sync! <span style="color: #a0522d;">it</span><span style="color: #707183;">))))</span>
                               <span style="color: #707183;">(</span>t nil<span style="color: #707183;">)</span>
                               nil<span style="color: #707183;">))))</span>



<span style="color: #b22222;">;; </span><span style="color: #b22222;">a helper function to parse html to org syntax:</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">pcase</span><span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-zotxt-parse-htmlstring</span> <span style="color: #707183;">(</span>html<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">with-temp-buffer</span>
    <span style="color: #707183;">(</span>insert html<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>libxml-parse-html-region <span style="color: #707183;">(</span>point-min<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>point-max<span style="color: #707183;">))))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-zotxt-htmlstring2org</span> <span style="color: #707183;">(</span>html<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>org-zotxt-htmltree2org <span style="color: #707183;">(</span>org-zotxt-parse-htmlstring html<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-zotxt-htmltree2org</span> <span style="color: #707183;">(</span>html<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">pcase</span> html
    <span style="color: #707183;">((</span>pred <span style="color: #707183;">(</span>stringp<span style="color: #707183;">))</span> html<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>`<span style="color: #707183;">(</span>a ,attrs . ,children<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"[[%s][%s]]"</span> <span style="color: #707183;">(</span>cdr <span style="color: #707183;">(</span>assq 'href attrs<span style="color: #707183;">))</span>
             <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span>`<span style="color: #707183;">(</span>i ,attrs . ,children<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"/%s/"</span> <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span>`<span style="color: #707183;">(</span>b ,attrs . ,children<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"*%s*"</span> <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span>`<span style="color: #707183;">(</span>p ,attrs . ,children<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"%s\n\n"</span> <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span>`<span style="color: #707183;">(</span>span ,attrs . ,children<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span><span style="color: #a020f0;">pcase</span> <span style="color: #707183;">(</span>cdr <span style="color: #707183;">(</span>assq 'style attrs<span style="color: #707183;">))</span>
       <span style="color: #707183;">(</span><span style="color: #8b2252;">"font-style:italic;"</span>
        <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"/%s/"</span> <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">)))</span>
       <span style="color: #707183;">(</span><span style="color: #8b2252;">"font-variant:small-caps;"</span>
        <span style="color: #b22222;">;; </span><span style="color: #b22222;">no way?</span>
        <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">))</span>
       <span style="color: #707183;">(</span>_ <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">))))</span>
    <span style="color: #707183;">((</span><span style="color: #a020f0;">or</span> `<span style="color: #707183;">(</span>html ,attrs . ,children<span style="color: #707183;">)</span>
         `<span style="color: #707183;">(</span>body ,attrs . ,children<span style="color: #707183;">))</span>
     <span style="color: #707183;">(</span>org-zotxt-htmltree2org children<span style="color: #707183;">))</span>
    <span style="color: #707183;">((</span>pred <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>h<span style="color: #707183;">)</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">and</span> <span style="color: #707183;">(</span>listp h<span style="color: #707183;">)</span>
                            <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span>stringp <span style="color: #707183;">(</span>car h<span style="color: #707183;">))</span>
                                <span style="color: #707183;">(</span><span style="color: #a020f0;">and</span> <span style="color: #707183;">(</span>listp <span style="color: #707183;">(</span>car h<span style="color: #707183;">))</span>
                                     <span style="color: #707183;">(</span>symbolp <span style="color: #707183;">(</span>car <span style="color: #707183;">(</span>car h<span style="color: #707183;">))))))))</span>
     <span style="color: #b22222;">;; </span><span style="color: #b22222;">list of strings or elements</span>
     <span style="color: #707183;">(</span>mapconcat #'org-zotxt-htmltree2org html <span style="color: #8b2252;">""</span><span style="color: #707183;">))))</span>
</pre>
        </div>
      </div>
    </div>
    
    <div id="outline-container-orgheadline35" class="outline-4">
      <h4 id="orgheadline35">
        Writer&rsquo;s Room
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline35">
        <p>
          This is an idea I&rsquo;ve had for a couple of years, which hasn&rsquo;t yet come to fruition. If I figure it out, I&rsquo;ll put something in this section!
        </p>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline37" class="outline-3">
    <h3 id="orgheadline37">
      First part
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline37">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">;;; Thisi s essential for code blocks, etc. </span>
 <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-use-speed-commands <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">and</span> <span style="color: #707183;">(</span>looking-at org-outline-regexp<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>looking-back <span style="color: #8b2252;">"^\**"</span><span style="color: #707183;">))))</span>

<span style="color: #b22222;">;;; </span><span style="color: #b22222;">Extract Links</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">my-org-extract-link</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Extract the link location at point and put it on the killring."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>org-in-regexp org-bracket-link-regexp 1<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>kill-new <span style="color: #707183;">(</span>org-link-unescape <span style="color: #707183;">(</span>org-match-string-no-properties 1<span style="color: #707183;">)))))</span>

<span style="color: #b22222;">;;; </span><span style="color: #b22222;">changing timestamps    </span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">update-org-days</span> <span style="color: #707183;">(</span>n<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Change all org-mode timestamps in the current buffer by N days."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"nChange days: "</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
    <span style="color: #707183;">(</span>goto-char <span style="color: #707183;">(</span>point-min<span style="color: #707183;">))</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">while</span> <span style="color: #707183;">(</span>re-search-forward <span style="color: #8b2252;">"[[&lt;]"</span> nil t<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>org-at-timestamp-p t<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>org-timestamp-change n 'day<span style="color: #707183;">)))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">incude htmlize.el</span>
<span style="color: #b22222;">;;; </span><span style="color: #b22222;">babel</span>
<span style="color: #707183;">(</span>org-babel-do-load-languages
 'org-babel-load-languages
  '<span style="color: #707183;">(</span> <span style="color: #707183;">(</span>perl . t<span style="color: #707183;">)</span>         
     <span style="color: #707183;">(</span>ruby . t<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span>sh . t<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span>python . t<span style="color: #707183;">)</span>
     <span style="color: #707183;">(</span>emacs-lisp . t<span style="color: #707183;">)</span>   
   <span style="color: #707183;">))</span>



<span style="color: #b22222;">;; </span><span style="color: #b22222;">yasnippet</span>
<span style="color: #707183;">(</span>add-hook 'org-mode-hook
          <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span>
            <span style="color: #707183;">(</span>org-set-local 'yas/trigger-key <span style="color: #707183;">[</span>tab<span style="color: #707183;">])</span>
            <span style="color: #707183;">(</span>define-key yas/keymap <span style="color: #707183;">[</span>tab<span style="color: #707183;">]</span> 'yas/next-field-or-maybe-expand<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">yas/org-very-safe-expand</span> <span style="color: #707183;">()</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>yas/fallback-behavior 'return-nil<span style="color: #707183;">))</span> <span style="color: #707183;">(</span>yas/expand<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span>add-hook 'org-mode-hook
          <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span>
            <span style="color: #707183;">(</span>make-variable-buffer-local 'yas/trigger-key<span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> yas/trigger-key <span style="color: #707183;">[</span>tab<span style="color: #707183;">])</span>
            <span style="color: #707183;">(</span>add-to-list 'org-tab-first-hook 'yas/org-very-safe-expand<span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>define-key yas/keymap <span style="color: #707183;">[</span>tab<span style="color: #707183;">]</span> 'yas/next-field<span style="color: #707183;">)))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">flymake-php</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-hook 'php-mode-hook 'flymake-php-load<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">some expand-region stuff</span>


<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">er/mark-org-heading</span> <span style="color: #707183;">(</span>level<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Marks a heading 0 or more levels up from current subheading"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"n"</span> <span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">while</span> <span style="color: #707183;">(</span>&gt; level 0<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>org-up-element<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> level <span style="color: #707183;">(</span>- level 1<span style="color: #707183;">))</span>
    <span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>org-mark-subtree<span style="color: #707183;">))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">er/mark-org-parent</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Marks a heading 1 level up from current subheading"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span>  <span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>org-up-element<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>org-mark-subtree<span style="color: #707183;">))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">er/mark-org-heading-2</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Marks a heading 0 or more levels up from current subheading"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"n"</span> <span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">(</span>level 2<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">while</span> <span style="color: #707183;">(</span>&gt; level 0<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span>org-up-element<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> level <span style="color: #707183;">(</span>- level 1<span style="color: #707183;">))</span>
      <span style="color: #707183;">))</span>
  <span style="color: #707183;">(</span>org-mark-subtree<span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun er/matt-add-org-mode-expansions ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Adds org-specific expansions for buffers in org-mode"</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(set (make-local-variable 'er/try-expand-list) (append</span>
<span style="color: #b22222;">;;                                                   </span><span style="color: #b22222;">er/try-expand-list</span>
<span style="color: #b22222;">;;                                                   </span><span style="color: #b22222;">'(org-mark-subtree</span>
<span style="color: #b22222;">;;                                                      </span><span style="color: #b22222;">er/mark-org-code-block</span>
<span style="color: #b22222;">;;                                                      </span><span style="color: #b22222;">er/mark-sentence</span>
<span style="color: #b22222;">;;                                                      </span><span style="color: #b22222;">er/mark-paragraph</span>
<span style="color: #b22222;">;;                                                      </span><span style="color: #b22222;">er/mark-org-heading-1</span>
<span style="color: #b22222;">;;                                                     </span><span style="color: #b22222;">er/mark-org-heading-2)</span>
<span style="color: #b22222;">;;                                                     </span><span style="color: #b22222;">)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(er/enable-mode-expansions 'org-mode 'er/matt-add-org-mode-expansions)</span>



<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-no-write</span> <span style="color: #707183;">()</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
    <span style="color: #707183;">(</span>beginning-of-line<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>looking-at org-property-re<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>myre <span style="color: #707183;">(</span>match-data<span style="color: #707183;">)</span> <span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>beg <span style="color: #707183;">(</span>match-beginning 1<span style="color: #707183;">))</span>
            <span style="color: #707183;">(</span>end <span style="color: #707183;">(</span>match-end 1<span style="color: #707183;">)))</span>
        <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"actually running"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>print myre<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>print beg<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>print end<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>put-text-property beg end 'read-only t<span style="color: #707183;">)</span> <span style="color: #707183;">))))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-write</span> <span style="color: #707183;">(</span> <span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
    <span style="color: #707183;">(</span>beginning-of-line<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>looking-at org-property-re<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>myre <span style="color: #707183;">(</span>match-data<span style="color: #707183;">)</span> <span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>beg <span style="color: #707183;">(</span>match-beginning 1<span style="color: #707183;">))</span>
            <span style="color: #707183;">(</span>end <span style="color: #707183;">(</span>match-end 1<span style="color: #707183;">))</span>
            <span style="color: #707183;">(</span>inhibit-read-only t<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"actually running"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>print myre<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>print beg<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>print end<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>remove-text-properties beg end '<span style="color: #707183;">(</span>read-only<span style="color: #707183;">))</span> <span style="color: #707183;">)))</span>
  <span style="color: #707183;">)</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">open things properly in org-mode</span>
<span style="color: #707183;">(</span>setcdr <span style="color: #707183;">(</span>assq 'system org-file-apps-defaults-gnu <span style="color: #707183;">)</span> <span style="color: #8b2252;">"xdg-open %s"</span><span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline38" class="outline-3">
    <h3 id="orgheadline38">
      Encryption
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline38">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">encryption with easypg</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">epa-file</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>epa-file-enable<span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-crypt</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>org-crypt-use-before-save-magic<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-tags-exclude-from-inheritance <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">(</span><span style="color: #8b2252;">"crypt"</span><span style="color: #707183;">)))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">GPG key to use for encryption</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Either the Key ID or set to nil to use symmetric encryption.</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-crypt-key nil<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline39" class="outline-3">
    <h3 id="orgheadline39">
      Org Function defuns
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline39">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-change-dates</span> <span style="color: #707183;">()</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">progn</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> ts-regex <span style="color: #8b2252;">"\\*\\*\\s-*&lt;</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">[0-9]\\{4\\}</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">-</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">[0-9]\\{2\\}</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">-</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">[0-9]\\{2\\}</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">\\s-</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">Mon</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">Tue</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">Wed</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">Thu</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">Fri</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">Sat</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">Sun</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">&gt;"</span><span style="color: #707183;">)</span>
      <span style="color: #b22222;">;; </span><span style="color: #b22222;">go to timestamp</span>
      <span style="color: #707183;">(</span>re-search-forward ts-regex<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> mwp-test-value <span style="color: #707183;">(</span>match-beginning 0<span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span>message <span style="color: #707183;">(</span>number-to-string  <span style="color: #707183;">(</span>match-beginning 0<span style="color: #707183;">)))</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>base-date <span style="color: #707183;">(</span>match-string 1<span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">this is the timestamp</span>
            <span style="color: #707183;">(</span>seven-days <span style="color: #707183;">(</span>seconds-to-time <span style="color: #707183;">(</span>* 7 24 60 60<span style="color: #707183;">)))</span>
            <span style="color: #707183;">(</span>new-ts<span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>week-num 0<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>message base-date<span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"in let!"</span><span style="color: #707183;">)</span>
        <span style="color: #707183;">(</span><span style="color: #a020f0;">while</span> <span style="color: #707183;">(</span>&lt; week-num 12<span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"in while!"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>add-value <span style="color: #707183;">(</span>seconds-to-time <span style="color: #707183;">(</span>* week-num 50 24 60 60<span style="color: #707183;">))))</span>
            <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"in second let"</span><span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> new-ts <span style="color: #707183;">(</span>format-time-string <span style="color: #8b2252;">"** &lt;%Y-%m-%d %a&gt;"</span>
                                             <span style="color: #707183;">(</span>time-add <span style="color: #707183;">(</span>date-to-time base-date<span style="color: #707183;">)</span> add-value<span style="color: #707183;">)))</span>
            <span style="color: #707183;">(</span>message new-ts<span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>re-search-forward ts-regex<span style="color: #707183;">)</span>

            <span style="color: #707183;">(</span>message <span style="color: #707183;">(</span>number-to-string  <span style="color: #707183;">(</span>match-beginning 0<span style="color: #707183;">)))</span>
            <span style="color: #707183;">(</span>message <span style="color: #707183;">(</span>number-to-string  <span style="color: #707183;">(</span>match-end 0<span style="color: #707183;">)))</span>
            <span style="color: #b22222;">;; </span><span style="color: #b22222;">now we kill the old time stamp, and insert the new one</span>
            <span style="color: #707183;">(</span>set-mark <span style="color: #707183;">(</span>match-beginning 0<span style="color: #707183;">))</span>

            <span style="color: #b22222;">;; </span><span style="color: #b22222;">(beginning-of-line)</span>
            <span style="color: #707183;">(</span>delete-region <span style="color: #707183;">(</span>match-beginning 0<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>match-end 0<span style="color: #707183;">))</span>
            <span style="color: #707183;">(</span>insert new-ts<span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> week-num <span style="color: #707183;">(</span>1+ week-num<span style="color: #707183;">))</span>
            <span style="color: #707183;">(</span>re-search-forward ts-regex<span style="color: #707183;">)))</span>
        <span style="color: #707183;">))))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">insert-ts+1w</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Insert a timestamp at point that is one week later than the</span>
<span style="color: #8b2252;">last timestamp found in the buffer."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>last-ts <span style="color: #707183;">(</span>car <span style="color: #707183;">(</span>last <span style="color: #707183;">(</span>org-element-map <span style="color: #707183;">(</span>org-element-parse-buffer<span style="color: #707183;">)</span> 'timestamp
                              <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>timestamp<span style="color: #707183;">)</span>
                                <span style="color: #707183;">(</span>org-element-property <span style="color: #483d8b;">:raw-value</span> timestamp<span style="color: #707183;">)))))))</span>
    <span style="color: #707183;">(</span>insert last-ts<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>backward-char 2<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>org-timestamp-change +7 'day<span style="color: #707183;">)</span>
    <span style="color: #707183;">))</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline40" class="outline-3">
    <h3 id="orgheadline40">
      Helm and Org
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline40">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">helm-bibtex</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> helm-bibtex-bibliography '<span style="color: #707183;">(</span><span style="color: #8b2252;">"/home/matt/.emacs.d/bibliography.bibtex/bibliography.bibtex.bib"</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">helm</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">helm-config</span><span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">The default "C-x c" is quite close to "C-x C-c", which quits Emacs.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Changed to "C-c h". Note: We must set "C-c h" globally, because we</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">cannot change `</span><span style="color: #008b8b;">helm-command-prefix-key</span><span style="color: #b22222;">' once `</span><span style="color: #008b8b;">helm-config</span><span style="color: #b22222;">' is loaded.</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c h"</span><span style="color: #707183;">)</span> 'helm-command-prefix<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-unset-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-x c"</span><span style="color: #707183;">))</span>


<span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>executable-find <span style="color: #8b2252;">"curl"</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> helm-google-suggest-use-curl-p t<span style="color: #707183;">))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> helm-split-window-in-side-p           t <span style="color: #b22222;">; </span><span style="color: #b22222;">open helm buffer inside current window, not occupy whole other window</span>
      helm-move-to-line-cycle-in-source     t <span style="color: #b22222;">; </span><span style="color: #b22222;">move to end or beginning of source when reaching top or bottom of source.</span>
      helm-ff-search-library-in-sexp        t <span style="color: #b22222;">; </span><span style="color: #b22222;">search for library in `</span><span style="color: #008b8b;">require</span><span style="color: #b22222;">' and `</span><span style="color: #008b8b;">declare-function</span><span style="color: #b22222;">' sexp.</span>
      helm-scroll-amount                    8 <span style="color: #b22222;">; </span><span style="color: #b22222;">scroll 8 lines other window using M-&lt;next&gt;/M-&lt;prior&gt;</span>
      helm-ff-file-name-history-use-recentf t<span style="color: #707183;">)</span>

<span style="color: #707183;">(</span>helm-mode 1<span style="color: #707183;">)</span>

<span style="color: #707183;">(</span>define-key helm-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"&lt;tab&gt;"</span><span style="color: #707183;">)</span> 'helm-execute-persistent-action<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">rebind tab to do persistent action</span>
<span style="color: #707183;">(</span>define-key helm-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-i"</span><span style="color: #707183;">)</span> 'helm-execute-persistent-action<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">make TAB works in terminal</span>
<span style="color: #707183;">(</span>define-key helm-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-z"</span><span style="color: #707183;">)</span>  'helm-select-action<span style="color: #707183;">)</span> <span style="color: #b22222;">; </span><span style="color: #b22222;">list actions using C-z</span>

<span style="color: #707183;">(</span>helm-autoresize-mode t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> helm-buffers-fuzzy-matching t
      helm-recentf-fuzzy-match    t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-x"</span><span style="color: #707183;">)</span> 'helm-M-x<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-y"</span><span style="color: #707183;">)</span> 'helm-show-kill-ring<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-x C-f"</span><span style="color: #707183;">)</span> 'helm-find-files<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-x b"</span><span style="color: #707183;">)</span> 'helm-mini<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>executable-find <span style="color: #8b2252;">"ack-grep"</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> helm-grep-default-command <span style="color: #8b2252;">"ack-grep -Hn --no-group --no-color %e %p %f"</span>
        helm-grep-default-recurse-command <span style="color: #8b2252;">"ack-grep -H --no-group --no-color %e %p %f"</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c h c"</span><span style="color: #707183;">)</span> 'helm-occur<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this binding breaks "Capital C"!  not sure why?</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">was missing 'kbd' sexp</span>
<span style="color: #707183;">(</span>define-key org-mode-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c h o"</span><span style="color: #707183;">)</span>  'helm-org-headlines<span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(global-set-key (kbd "C-c h o") 'helm-org-headlines)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-h SPC"</span><span style="color: #707183;">)</span> 'helm-all-mark-rings<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>global-set-key <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c h g"</span><span style="color: #707183;">)</span> 'helm-google-suggest<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-to-list 'helm-completing-read-handlers-alist '<span style="color: #707183;">(</span>zotxt-completing-read . helm-comp-read <span style="color: #707183;">))</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">helm-bibtex</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">helm-bibtex-format-pandoc-citation</span> <span style="color: #707183;">(</span>keys<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>concat <span style="color: #8b2252;">"["</span> <span style="color: #707183;">(</span>mapconcat <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>key<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>concat <span style="color: #8b2252;">"@"</span> key<span style="color: #707183;">))</span> keys <span style="color: #8b2252;">"; "</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"]"</span><span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">inform helm-bibtex how to format the citation in org-mode</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(setf (cdr (assoc 'org-mode helm-bibtex-format-citation-functions))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">'helm-bibtex-format-pandoc-citation)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setf</span> <span style="color: #707183;">(</span>cdr <span style="color: #707183;">(</span>assoc 'org-mode helm-bibtex-format-citation-functions<span style="color: #707183;">))</span>
      'helm-bibtex-format-citation-ebib<span style="color: #707183;">)</span>
</pre>
      </div>
    </div>
    
    <div id="outline-container-orgheadline41" class="outline-4">
      <h4 id="orgheadline41">
        Helm-swish-e
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline41">
        <p>
          An indexer for forg-mode files. from: <a href="http://kitchingroup.cheme.cmu.edu/blog/2015/06/25/Integrating-swish-e-and-Emacs/">http://kitchingroup.cheme.cmu.edu/blog/2015/06/25/Integrating-swish-e-and-Emacs/</a>
        </p>
        
        <p>
          A much cooler version is described here: <a href="http://kitchingroup.cheme.cmu.edu/blog/2015/07/06/Indexing-headlines-in-org-files-with-swish-e-with-laser-sharp-results/">http://kitchingroup.cheme.cmu.edu/blog/2015/07/06/Indexing-headlines-in-org-files-with-swish-e-with-laser-sharp-results/</a>
        </p>
        
        <p>
          But requires some setup first.
        </p>
        
        <div class="org-src-container">
          <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">helm-swish-e-candidates</span> <span style="color: #707183;">(</span>query<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Generate a list of cons cells (swish-e result . path)."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>result <span style="color: #707183;">(</span>shell-command-to-string
                  <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"swish-e -f ~/.swish-e/index.swish-e -x \"%%r\t%%p\n\" -w %s"</span>
                          <span style="color: #707183;">(</span>shell-quote-argument query<span style="color: #707183;">))))</span>
         <span style="color: #707183;">(</span>lines <span style="color: #707183;">(</span>s-split <span style="color: #8b2252;">"\n"</span> result t<span style="color: #707183;">))</span>
         <span style="color: #707183;">(</span>candidates '<span style="color: #707183;">()))</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">loop</span> for line in lines
          unless <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span>  <span style="color: #707183;">(</span>s-starts-with? <span style="color: #8b2252;">"#"</span> line<span style="color: #707183;">)</span>
                      <span style="color: #707183;">(</span>s-starts-with? <span style="color: #8b2252;">"."</span> line<span style="color: #707183;">))</span>
          collect <span style="color: #707183;">(</span>cons line <span style="color: #707183;">(</span>cdr <span style="color: #707183;">(</span>s-split <span style="color: #8b2252;">"\t"</span> line<span style="color: #707183;">))))))</span>


<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">helm-swish-e</span> <span style="color: #707183;">(</span>query<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Run a swish-e query and provide helm selection buffer of the results."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"sQuery: "</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>helm <span style="color: #483d8b;">:sources</span> `<span style="color: #707183;">(((</span>name . ,<span style="color: #707183;">(</span>format <span style="color: #8b2252;">"swish-e: %s"</span> query<span style="color: #707183;">))</span>
                    <span style="color: #707183;">(</span>candidates . ,<span style="color: #707183;">(</span>helm-swish-e-candidates query<span style="color: #707183;">))</span>
                    <span style="color: #707183;">(</span>action . <span style="color: #707183;">((</span><span style="color: #8b2252;">"open"</span> . <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>f<span style="color: #707183;">)</span>
                                           <span style="color: #707183;">(</span>find-file <span style="color: #707183;">(</span>car f<span style="color: #707183;">)))))))</span>
                   <span style="color: #707183;">((</span>name . <span style="color: #8b2252;">"New search"</span><span style="color: #707183;">)</span>
                    <span style="color: #707183;">(</span>dummy<span style="color: #707183;">)</span>
                    <span style="color: #707183;">(</span>action . <span style="color: #707183;">((</span><span style="color: #8b2252;">"search"</span> . <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>f<span style="color: #707183;">)</span>
                                             <span style="color: #707183;">(</span>helm-swish-e helm-pattern<span style="color: #707183;">)))))))))</span>
</pre>
        </div>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline42" class="outline-3">
    <h3 id="orgheadline42">
      More of the mixed up stuff
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline42">
      <div class="org-src-container">
        <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">ibuffer</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ibuffer</span><span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> ibuffer-saved-filter-groups
      <span style="color: #707183;">(</span><span style="color: #a020f0;">quote</span> <span style="color: #707183;">((</span><span style="color: #8b2252;">"default"</span>
               <span style="color: #707183;">(</span><span style="color: #8b2252;">"dired"</span> <span style="color: #707183;">(</span>mode . dired-mode<span style="color: #707183;">))</span>
               <span style="color: #707183;">(</span><span style="color: #8b2252;">"java"</span> <span style="color: #707183;">(</span>mode . java-mode<span style="color: #707183;">))</span>
               <span style="color: #707183;">(</span><span style="color: #8b2252;">"org"</span> <span style="color: #707183;">(</span>mode . org-mode<span style="color: #707183;">))</span>
               <span style="color: #707183;">(</span><span style="color: #8b2252;">"elisp"</span> <span style="color: #707183;">(</span>mode . elisp-mode<span style="color: #707183;">))</span>
               <span style="color: #707183;">(</span><span style="color: #8b2252;">"xml"</span> <span style="color: #707183;">(</span>mode . nxml-mode<span style="color: #707183;">))))))</span>    

<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> ibuffer-show-empty-filter-groups nil<span style="color: #707183;">)</span>

<span style="color: #707183;">(</span>add-hook 'ibuffer-mode-hook 
          <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span> 
            <span style="color: #707183;">(</span>ibuffer-switch-to-saved-filter-groups <span style="color: #8b2252;">"default"</span><span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>ibuffer-filter-by-filename <span style="color: #8b2252;">"."</span><span style="color: #707183;">)))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">to show only dired and files buffers</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">org-mobile</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-mobile-directory <span style="color: #8b2252;">"~/Dropbox/MobileOrg"</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">ace</span>
<span style="color: #b22222;">;; </span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">enable a more powerful jump back function from ace jump mode</span>
<span style="color: #b22222;">;;</span>
<span style="color: #707183;">(</span>define-key global-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-c C-SPC"</span><span style="color: #707183;">)</span> 'ace-jump-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>autoload
  'ace-jump-mode-pop-mark
  <span style="color: #8b2252;">"ace-jump-mode"</span>
  <span style="color: #8b2252;">"Ace jump back:-)"</span>
  t<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>eval-after-load <span style="color: #8b2252;">"ace-jump-mode"</span>
  '<span style="color: #707183;">(</span>ace-jump-mode-enable-mark-sync<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>define-key global-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"C-x C-SPC"</span><span style="color: #707183;">)</span> 'ace-jump-mode-pop-mark<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">which function mode</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">which-func</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>add-to-list 'which-func-modes 'org-mode<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>which-func-mode 1<span style="color: #707183;">)</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">fix open links</span>
<span style="color: #707183;">(</span>setcdr <span style="color: #707183;">(</span>assq 'system org-file-apps-defaults-gnu <span style="color: #707183;">)</span> '<span style="color: #707183;">(</span>call-process <span style="color: #8b2252;">"xdg-open"</span> nil 0 nil file<span style="color: #707183;">))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">ox-reveal, change initialization options</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">github (push  )</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">endless/visit-pull-request-url</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Visit the current branch's PR on Github."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>browse-url
   <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"https://github.com/%s/compare/%s"</span>
           <span style="color: #707183;">(</span>replace-regexp-in-string
            <span style="color: #8b2252;">"\\`.+github\\.com:</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">.+</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">\\.git\\'"</span> <span style="color: #8b2252;">"\\1"</span>
            <span style="color: #707183;">(</span>magit-get <span style="color: #8b2252;">"remote"</span>
                       <span style="color: #707183;">(</span>magit-get-current-remote<span style="color: #707183;">)</span>
                       <span style="color: #8b2252;">"url"</span><span style="color: #707183;">))</span>
           <span style="color: #707183;">(</span>magit-get-current-branch<span style="color: #707183;">))))</span>

<span style="color: #707183;">(</span>eval-after-load 'magit
  '<span style="color: #707183;">(</span>define-key magit-mode-map <span style="color: #8b2252;">"V"</span>
     #'endless/visit-pull-request-url<span style="color: #707183;">))</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">change default c-u values for C-c C-l</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-insert-link</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> complete-file link-location default-description<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"insert a link at location, but insert a letave link by default, and absolute one only by necessity."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">cond</span>
   <span style="color: #707183;">((</span>eq current-prefix-arg nil<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>current-prefix-arg 4<span style="color: #707183;">))</span> 
      <span style="color: #707183;">(</span>call-interactively 'org-insert-link<span style="color: #707183;">)))</span>
   <span style="color: #707183;">(</span>t
    <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>current-prefix-arg nil<span style="color: #707183;">))</span> 
      <span style="color: #707183;">(</span>call-interactively 'org-insert-link<span style="color: #707183;">)))</span>
   <span style="color: #707183;">(</span>t <span style="color: #707183;">(</span>call-interactively 'org-insert-link<span style="color: #707183;">))</span>
   <span style="color: #707183;">))</span>
</pre>
      </div>
    </div>
  </div>
  
  <div id="outline-container-orgheadline43" class="outline-3">
    <h3 id="orgheadline43">
      Mailing subtrees with Attachments
    </h3>
    
    <div class="outline-text-3" id="text-orgheadline43">
      <p>
        This is pretty exciting; I&rsquo;ve figured out how to quickly send org-mode subtrees as MIME-encoded emails. That means that, essentially, I can write in org, as plain text, and very quickly export to HTML, add attachments, and send. The exciting part about this for me is that it should streamline my communications with students, while also letting me stay in Org and keep my records in order. Let&rsquo;s walk through the process.
      </p>
    </div>
    
    <div id="outline-container-orgheadline44" class="outline-4">
      <h4 id="orgheadline44">
        Use-case
      </h4>
      
      <div class="outline-text-4" id="text-orgheadline44">
        <p>
          For the moment, I still use Thunderbird as my primary MUA. It&rsquo;s pretty easy to use, minimal configuration compared to all things Emacs, and if something goes wrong with it I don&rsquo;t have to quit Emacs (!), just Thunderbird.
        </p>
        
        <p>
          In some cases, though, Thunderbird makes for an awkward workflow. That&rsquo;s certainly the case for grading, whih has many, poorly-integrated elements. To mark an assignment I need to:
        </p>
        
        <ul class="org-ul">
          <li>
            log in to Blackboard (in Firefox; I wonder if I could do that in Emacs?)
          </li>
          <li>
            download the set of student papers (one at a time, 2 clicks per paper) to <code>Downlads</code>
          </li>
          <li>
            move papers to a directory (usually ~/COURSENAME/Grading/ASSIGNMENTNAME )
          </li>
          <li>
            Read papers in LigreOffice, comment inline
          </li>
          <li>
            record mark in Libreoffice Calc spreadsheet
          </li>
          <li>
            email paper back to student with comments
          </li>
          <li>
            upload marks back into Blackboard
          </li>
          <li>
            find a place to archive the student paper in case I need it later, e.g. for a contested grade.
          </li>
        </ul>
        
        <p>
          The while process basically sucks. I spend maybe 20% of my time fussing with paths and mouse clicks and email addresses. So I am experimenting with moving as much of this process into Emacs. So far, I don&rsquo;t think there&rsquo;s any way at all to bulk-download the papers &#x2013; that sucks, but I can live with it I guess (I have to!). So I start the optimization at the point where I have all my papers ready to go in a subdir.
        </p>
      </div>
      
      <ul class="org-ul">
        <li>
          <a id="orgheadline45"></a>Org-mime<br /> <div class="outline-text-5" id="text-orgheadline45">
            <p>
              Org-mime is the library that allows org buffers and other elements to be quickly converted to HTML and prepared for multi-part messaging. Load it and set it up (see <a href="http://orgmode.org/worg/org-contrib/org-mime.html">http://orgmode.org/worg/org-contrib/org-mime.html</a>):
            </p>
            
            <div class="org-src-container">
              <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">enable HTML email from org</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-mime</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">setup org-mime for wanderlust</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(setq org-mime-library 'semi)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">or for gnus/message-mode</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> org-mime-library 'mml<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">easy access to htmlize in message-mode</span>
<span style="color: #707183;">(</span>add-hook 'message-mode-hook
          <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span>
            <span style="color: #707183;">(</span>local-set-key <span style="color: #8b2252;">"\C-c\M-o"</span> 'org-mime-htmlize<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">uncomment this to use the org-mome native functions for htmlizing.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'org-mode-hook</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">(lambda ()</span>
<span style="color: #b22222;">;;             </span><span style="color: #b22222;">(local-set-key "\C-c\M-o" 'org-mime-org-buffer-htmlize)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">uncomment to displyay src blocks with a dark background</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'org-mime-html-hook</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">(lambda ()</span>
<span style="color: #b22222;">;;             </span><span style="color: #b22222;">(org-mime-change-element-style</span>
<span style="color: #b22222;">;;              </span><span style="color: #b22222;">"pre" (format "color: %s; background-color: %s; padding: 0.5em;"</span>
<span style="color: #b22222;">;;                            </span><span style="color: #b22222;">"#E6E1DC" "#232323"))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">pretty blockquotes</span>
<span style="color: #707183;">(</span>add-hook 'org-mime-html-hook
          <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span>
            <span style="color: #707183;">(</span>org-mime-change-element-style

             <span style="color: #8b2252;">"blockquote"</span> <span style="color: #8b2252;">"border-left: 2px solid gray; padding-left: 4px;"</span><span style="color: #707183;">)))</span>
</pre>
            </div>
          </div>
        </li>
        
        <li>
          <a id="orgheadline46"></a>Fix htmlization<br /> <div class="outline-text-5" id="text-orgheadline46">
            <p>
              Upstream org-mime-htmlize unfortunately can&rsquo;t be called uninteractively (bummer!), so we have to rewrite it to make programmatic calls work properly. I found the solution <a href="http://emacs.stackexchange.com/questions/13505/a-function-to-org-mime-subtree-then-org-mime-htmlize">Emacs Stackexchange</a>.
            </p>
            
            <div class="org-src-container">
              <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-mime-htmlize</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> arg<span style="color: #707183;">)</span>
<span style="color: #8b2252;">"Export a portion of an email body composed using `</span><span style="color: #008b8b;">mml-mode</span><span style="color: #8b2252;">' to</span>
<span style="color: #8b2252;">html using `</span><span style="color: #008b8b;">org-mode</span><span style="color: #8b2252;">'.  If called with an active region only</span>
<span style="color: #8b2252;">export that region, otherwise export the entire body."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"P"</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-org</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">ox-html</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>region-p <span style="color: #707183;">(</span>org-region-active-p<span style="color: #707183;">))</span>
         <span style="color: #707183;">(</span>html-start <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">and</span> region-p <span style="color: #707183;">(</span>region-beginning<span style="color: #707183;">))</span>
                         <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
                           <span style="color: #707183;">(</span>goto-char <span style="color: #707183;">(</span>point-min<span style="color: #707183;">))</span>
                           <span style="color: #707183;">(</span>search-forward mail-header-separator<span style="color: #707183;">)</span>
                           <span style="color: #707183;">(</span>+ <span style="color: #707183;">(</span>point<span style="color: #707183;">)</span> 1<span style="color: #707183;">))))</span>
         <span style="color: #707183;">(</span>html-end <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">and</span> region-p <span style="color: #707183;">(</span>region-end<span style="color: #707183;">))</span>
                       <span style="color: #b22222;">;; </span><span style="color: #b22222;">TODO: should catch signature...</span>
                       <span style="color: #707183;">(</span>point-max<span style="color: #707183;">)))</span>
         <span style="color: #707183;">(</span>raw-body <span style="color: #707183;">(</span>concat org-mime-default-header
                           <span style="color: #707183;">(</span>buffer-substring html-start html-end<span style="color: #707183;">)))</span>
         <span style="color: #707183;">(</span>tmp-file <span style="color: #707183;">(</span>make-temp-name <span style="color: #707183;">(</span>expand-file-name
                                    <span style="color: #8b2252;">"mail"</span> temporary-file-directory<span style="color: #707183;">)))</span>
         <span style="color: #707183;">(</span>body <span style="color: #707183;">(</span>org-export-string-as raw-body 'org t<span style="color: #707183;">))</span>
         <span style="color: #b22222;">;; </span><span style="color: #b22222;">because we probably don't want to export a huge style file</span>
         <span style="color: #707183;">(</span>org-export-htmlize-output-type 'inline-css<span style="color: #707183;">)</span>
         <span style="color: #b22222;">;; </span><span style="color: #b22222;">makes the replies with "&gt;"s look nicer</span>
         <span style="color: #707183;">(</span>org-export-preserve-breaks org-mime-preserve-breaks<span style="color: #707183;">)</span>
         <span style="color: #b22222;">;; </span><span style="color: #b22222;">dvipng for inline latex because MathJax doesn't work in mail</span>
         <span style="color: #707183;">(</span>org-html-with-latex 'dvipng<span style="color: #707183;">)</span>
         <span style="color: #b22222;">;; </span><span style="color: #b22222;">to hold attachments for inline html images</span>
         <span style="color: #707183;">(</span>html-and-images
          <span style="color: #707183;">(</span>org-mime-replace-images
           <span style="color: #707183;">(</span>org-export-string-as raw-body 'html t<span style="color: #707183;">)</span> tmp-file<span style="color: #707183;">))</span>
         <span style="color: #707183;">(</span>html-images <span style="color: #707183;">(</span><span style="color: #a020f0;">unless</span> arg <span style="color: #707183;">(</span>cdr html-and-images<span style="color: #707183;">)))</span>
         <span style="color: #707183;">(</span>html <span style="color: #707183;">(</span>org-mime-apply-html-hook
                <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> arg
                    <span style="color: #707183;">(</span>format org-mime-fixedwith-wrap body<span style="color: #707183;">)</span>
                  <span style="color: #707183;">(</span>car html-and-images<span style="color: #707183;">)))))</span>
    <span style="color: #707183;">(</span>delete-region html-start html-end<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
      <span style="color: #707183;">(</span>goto-char html-start<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span>insert <span style="color: #707183;">(</span>org-mime-multipart
               body html <span style="color: #707183;">(</span>mapconcat 'identity html-images <span style="color: #8b2252;">"\n"</span><span style="color: #707183;">))))))</span>
</pre>
            </div>
          </div>
        </li>
        
        <li>
          <a id="orgheadline47"></a>Actually perform the export!<br /> <div class="outline-text-5" id="text-orgheadline47">
            <p>
              These functions are crude helpers that gather extra information about the org subtree, of which org-mime is unaware.
            </p>
            
            <ul class="org-ul">
              <li>
                <code>mwp-org-get-parent-headline</code> traverses the tree to the ancestor headline, because that&rsquo;s what I want to set the subject to.
              </li>
              <li>
                <code>mwp-org-attachment-list</code> is stolen directly from the Gnorb package, which looks cool, awesome ,and kinda complex; it just iterates through a subtree&rsquo;s attachments and grabs URLs.
              </li>
              <li>
                <code>mwp-send-subtree-with-attachments</code> performs the export and is bound to <code>C-c M-o</code>
              </li>
            </ul>
            
            <p>
              So, if I want to mail a subtree, I just <code>C-c M-o</code> and I&rsquo;m almost done &#x2013; the html mail is ready to go, and all org attachments are also attached to the email.
            </p>
            
            <p>
              Note there are some real weaknesses here: <code>mwp-org-get-parent-headline</code> actually gets the top-level <i>ancestor</i> &#x2013; which only happens to be what I want right now. Better would be to use org-element to locate the parent (and other headline attributes) directly, but I&rsquo;m not sure how to do that.
            </p>
            
            <p>
              Similarly, the initial greeting is generated from the current headline value &#x2013; so this only works because I name my subtrees after the addressee (which I only do because of my use case).
            </p>
            
            <div class="org-src-container">
              <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-get-parent-headline</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Acquire the parent headline & return."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
    <span style="color: #707183;">(</span>org-mark-subtree<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>re-search-backward  <span style="color: #8b2252;">"^\\* "</span><span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>nth 4 <span style="color: #707183;">(</span>org-heading-components<span style="color: #707183;">))))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-send-subtree-with-attachments</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"org-mime-subtree and HTMLize"</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>org-mark-subtree<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>attachments <span style="color: #707183;">(</span>mwp-org-attachment-list<span style="color: #707183;">))</span>
        <span style="color: #707183;">(</span>subject  <span style="color: #707183;">(</span>mwp-org-get-parent-headline<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span>insert <span style="color: #8b2252;">"Hello "</span> <span style="color: #707183;">(</span>nth 4 org-heading-components<span style="color: #707183;">)</span> <span style="color: #8b2252;">",\n"</span><span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>org-mime-subtree<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>insert <span style="color: #8b2252;">"\nBest,\nMP.\n"</span><span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"subject is"</span> <span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>message subject<span style="color: #707183;">)</span>
    <span style="color: #b22222;">;;</span><span style="color: #b22222;">(message-to)</span>
    <span style="color: #707183;">(</span>org-mime-htmlize<span style="color: #707183;">)</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">this comes from gnorb</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">I will reintroduce it if I want to reinstate questions.</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">(map-y-or-n-p</span>
    <span style="color: #b22222;">;;  </span><span style="color: #b22222;">;; (lambda (a) (format "Attach %s to outgoing message? "</span>
    <span style="color: #b22222;">;;  </span><span style="color: #b22222;">;;                    (file-name-nondirectory a)))</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">(lambda (a)</span>
    <span style="color: #b22222;">;;   </span><span style="color: #b22222;">(mml-attach-file a (mm-default-file-encoding a)</span>
    <span style="color: #b22222;">;;                    </span><span style="color: #b22222;">nil "attachment"))</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">attachments</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">'("file" "files" "attach"))</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">(message "Attachments: %s" attachments)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">dolist</span> <span style="color: #707183;">(</span>a attachments<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"Attachment: %s"</span> a<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>mml-attach-file a <span style="color: #707183;">(</span>mm-default-file-encoding a<span style="color: #707183;">)</span> nil <span style="color: #8b2252;">"attachment"</span><span style="color: #707183;">))</span>
    <span style="color: #707183;">(</span>message-goto-to<span style="color: #707183;">)</span>
    <span style="color: #707183;">))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">add a keybinding for org-mode</span>
<span style="color: #707183;">(</span>add-hook 'org-mode-hook
          <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">()</span>
            <span style="color: #707183;">(</span>local-set-key <span style="color: #8b2252;">"\C-c\M-o"</span> 'mwp-send-subtree-with-attachments<span style="color: #707183;">)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">stolen from gnorb; finds attachments in subtree</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-attachment-list</span> <span style="color: #707183;">(</span><span style="color: #228b22;">&optional</span> id<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Get a list of files (absolute filenames) attached to the</span>
<span style="color: #8b2252;">current heading, or the heading indicated by optional argument ID."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">featurep</span> '<span style="color: #008b8b;">org-attach</span><span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>attach-dir <span style="color: #707183;">(</span><span style="color: #a020f0;">save-excursion</span>
                         <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> id
                           <span style="color: #707183;">(</span>org-id-goto id<span style="color: #707183;">))</span>
                         <span style="color: #707183;">(</span>org-attach-dir t<span style="color: #707183;">)))</span>
           <span style="color: #707183;">(</span>files
            <span style="color: #707183;">(</span>mapcar
             <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>f<span style="color: #707183;">)</span>
               <span style="color: #707183;">(</span>expand-file-name f attach-dir<span style="color: #707183;">))</span>
             <span style="color: #707183;">(</span>org-attach-file-list attach-dir<span style="color: #707183;">))))</span>
      files<span style="color: #707183;">)))</span>
</pre>
            </div>
          </div>
        </li>
        
        <li>
          <a id="orgheadline48"></a>Contacts<br /> <div class="outline-text-5" id="text-orgheadline48">
            <p>
              That&rsquo;s a good start, but there are still some steps to make this truly convenient. For instance, I certainly don&rsquo;t want to type in students&rsquo; email addresses by hand. So I imported my contacts from thunderbird to org-contacts. This was a pain &#x2013; the process was Thunderbird &rarr; Gmail (via gsync plugin) &rarr; vcard (via gmail export) &rarr; org-contacts (via <a href="https://gist.github.com/tmalsburg/9747104">Titus&#8217;s python importer</a>). I wish there was a CSV importer for org-contacts; probably this would be easy to write but I&rsquo;m so slooooowwww at coding. My org contacts live in <code>GTD/Contacts.org</code>, which is set in <code>Customize</code>, and org reads them on startup with this line
            </p>
            
            <div class="org-src-container">
              <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-contacts</span><span style="color: #707183;">)</span>
</pre>
            </div>
            
            <p>
              With this single line, org-contacts now provides <kbd>TAB</kbd> completion
            </p>
          </div>
        </li>
        
        <li>
          <a id="orgheadline49"></a><a id="ID-7c9d8afa-0b2d-4be6-94b4-b586b4ea3bd7"></a>Attachments and Links<br /> <div class="outline-text-5" id="text-orgheadline49">
            <p>
              Start by loading org-download, which downloads dragged images as attachments and inserts a link. (yay). THen a modification which fixes handling of file links (oops! lost this!!) allowing me to drag-n-drop files links onto org as attachments. Never did work quite right, actually.
            </p>
            
            <div class="org-src-container">
              <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-download</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-attach</span><span style="color: #707183;">)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">extending the dnd functionality</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">but doesn't actually work... </span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-file-link-dnd</span> <span style="color: #707183;">(</span>uri action<span style="color: #707183;">)</span>
    <span style="color: #8b2252;">"When in `</span><span style="color: #008b8b;">org-mode</span><span style="color: #8b2252;">' and URI points to local file, </span>
<span style="color: #8b2252;">  add as attachment and also add a link. Otherwise, </span>
<span style="color: #8b2252;">  pass URI and Action back to dnd dispatch"</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">cond</span> <span style="color: #707183;">((</span>eq major-mode 'org-mode<span style="color: #707183;">)</span>
           <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"Hi! uri: %s "</span> <span style="color: #707183;">(</span>file-relative-name uri<span style="color: #707183;">))</span>
           <span style="color: #707183;">(</span>org-attach-attach uri nil <span style="color: #8b2252;">"lns"</span><span style="color: #707183;">)</span>
           <span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>t
           <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>dnd-protocol-alist
                  <span style="color: #707183;">(</span>rassq-delete-all
                   'mwp-org-file-link-dnd
                   <span style="color: #707183;">(</span>copy-alist dnd-protocol-alist<span style="color: #707183;">))))</span>
             <span style="color: #707183;">(</span>dnd-handle-one-url nil action uri<span style="color: #707183;">)))))</span>

  <span style="color: #b22222;">;; </span><span style="color: #b22222;">add a new function that DOESN'T open the attachment!</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-attach-new-dont-open</span> <span style="color: #707183;">(</span>file<span style="color: #707183;">)</span>
    <span style="color: #8b2252;">"Create a new attachment FILE for the current task.</span>
<span style="color: #8b2252;">  The attachment is created as an Emacs buffer."</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"sCreate attachment named: "</span><span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span><span style="color: #a020f0;">and</span> org-attach-file-list-property <span style="color: #707183;">(</span>not org-attach-inherited<span style="color: #707183;">))</span>
      <span style="color: #707183;">(</span>org-entry-add-to-multivalued-property
       <span style="color: #707183;">(</span>point<span style="color: #707183;">)</span> org-attach-file-list-property file<span style="color: #707183;">))</span>
    <span style="color: #707183;">)</span>

  <span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-file-link-enable</span> <span style="color: #707183;">()</span>
    <span style="color: #8b2252;">"Enable file drag and drop attachments."</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">unless</span> <span style="color: #707183;">(</span>eq <span style="color: #707183;">(</span>cdr <span style="color: #707183;">(</span>assoc <span style="color: #8b2252;">"^</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">file</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">://"</span> dnd-protocol-alist<span style="color: #707183;">))</span>
                'mwp-org-file-link-dnd<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> dnd-protocol-alist
            `<span style="color: #707183;">((</span><span style="color: #8b2252;">"^</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">file</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">://"</span> . mwp-org-file-link-dnd<span style="color: #707183;">)</span> ,@dnd-protocol-alist<span style="color: #707183;">))))</span>

  <span style="color: #b22222;">;;</span><span style="color: #b22222;">(mwp-org-file-link-enable)</span>
</pre>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline51" class="outline-2">
  <h2 id="orgheadline51">
    Keybindings
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline51">
    <p>
      really this should go at the end of the file so it doesn&rsquo;t get overwritten
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">abbrevs</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">improved autocorrect, with a decent keybinding</span>
<span style="color: #707183;">(</span>define-key ctl-x-map <span style="color: #8b2252;">"\C-i"</span> 'endless/ispell-word-then-abbrev<span style="color: #707183;">)</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline52" class="outline-2">
  <h2 id="orgheadline52">
    Desktop
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline52">
    <p>
      Wait to the end to start up desktop, so by the time files appear, emacs actually works.
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;;; </span><span style="color: #b22222;">session management with desktop</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">desktop</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> desktop-dirname <span style="color: #8b2252;">"/home/matt/.emacs.d/desktop-sessions/"</span><span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>desktop-save-mode 1<span style="color: #707183;">)</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline53" class="outline-2">
  <h2 id="orgheadline53">
    Unused
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline53">
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">This is how Magnar manages his packages.  I'm not there yet.  soon I hope.  </span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Setup packages</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'setup-package)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Install extensions if they're missing</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun init--install-packages ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(packages-install</span>
<span style="color: #b22222;">;;    </span><span style="color: #b22222;">'(magit</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">paredit</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">move-text</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">gist</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">htmlize</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">visual-regexp</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">flycheck</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">flx</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">flx-ido</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">css-eldoc</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">yasnippet</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">smartparens</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">ido-vertical-mode</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">ido-at-point</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">simple-httpd</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">guide-key</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">nodejs-repl</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">restclient</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">highlight-escape-sequences</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">whitespace-cleanup-mode</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">elisp-slime-nav</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">git-commit-mode</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">gitconfig-mode</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">gitignore-mode</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">clojure-mode</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">groovy-mode</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">prodigy</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">cider</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">cider-tracing)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(condition-case nil</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(init--install-packages)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(error</span>
<span style="color: #b22222;">;;    </span><span style="color: #b22222;">(package-refresh-contents)</span>
<span style="color: #b22222;">;;    </span><span style="color: #b22222;">(init--install-packages)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this is the main structure of magnar's .emacs; it's a little complex for me</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Setup extensions</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(eval-after-load 'ido '(require 'setup-ido))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(eval-after-load 'org '(require 'setup-org))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(eval-after-load 'dired '(require 'setup-dired))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(eval-after-load 'magit '(require 'setup-magit))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(eval-after-load 'grep '(require 'setup-rgrep))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(eval-after-load 'shell '(require 'setup-shell))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'setup-hippie)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'setup-yasnippet)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'setup-perspective)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'setup-ffip)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'setup-html-mode)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'setup-paredit)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">;; Elisp go-to-definition with M-. and back again with M-,</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(autoload 'elisp-slime-nav-mode "elisp-slime-nav")</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'emacs-lisp-mode-hook (lambda () (elisp-slime-nav-mode t) (eldoc-mode 1)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">;; Run at full power please</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(put 'downcase-region 'disabled nil)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(put 'upcase-region 'disabled nil)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(put 'narrow-to-region 'disabled nil)</span>
<span style="color: #707183;">(</span>put 'upcase-region 'disabled nil<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">;; Conclude init by setting up specifics for the current user</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(when (file-exists-p user-settings-dir)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(mapc 'load (directory-files user-settings-dir nil "^[</span><span style="color: #b22222;">^</span><span style="color: #b22222;">#].*el$")))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">setup autopair</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(autopair-global-mode) ;; enable autopair in all buffers</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">;; add opened files to gnome recent-files list</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun fd-add-file-to-recent ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(when buffer-file-name</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(start-process "addtorecent" nil "addtorecent"</span>
<span style="color: #b22222;">;;                    </span><span style="color: #b22222;">(concat "file://" buffer-file-name)</span>
<span style="color: #b22222;">;;                    </span><span style="color: #b22222;">"text/plain"</span>
<span style="color: #b22222;">;;                    </span><span style="color: #b22222;">"Emacs"</span>
<span style="color: #b22222;">;;                    </span><span style="color: #b22222;">"emacsclient %F")))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'find-file-hook 'fd-add-file-to-recent)</span>
<span style="color: #b22222;">;;; </span><span style="color: #b22222;">Speedbar and Imenu</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">This adds support for speedbar and Imenu.  Right now I'm not actually using either though.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'org-mode-hook</span>
<span style="color: #b22222;">;;                     </span><span style="color: #b22222;">(lambda () (imenu-add-to-menubar "Imenu")))</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(require 'speedbar)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(speedbar-add-supported-extension ".org")</span>

<span style="color: #b22222;">;;;; </span><span style="color: #b22222;">changing languages for e.g. spellcheck.  I don't use this much right now</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(let ((langs '("canadian" "francais")))</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(setq lang-ring (make-ring (length langs)))</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(dolist (elem langs) (ring-insert lang-ring elem)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun cycle-ispell-languages ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(interactive)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(let ((lang (ring-ref lang-ring -1)))</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(ring-insert lang-ring lang)</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(ispell-change-dictionary lang)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(global-set-key [f6] 'cycle-ispell-languages)</span>
<span style="color: #b22222;">;;; </span><span style="color: #b22222;">Worg and Wanderlust</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">also some org-wl interaction</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">stolen from worg: http://orgmode.org/worg/org-hacks.html</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun dmj/wl-send-html-message ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Send message as html message.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">Convert body of message to html using</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">`</span><span style="color: #008b8b;">org-export-region-as-html</span><span style="color: #b22222;">'."</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(require 'org)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(save-excursion</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(let (beg end html text)</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(goto-char (point-min))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(re-search-forward "^--text follows this line--$")</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;; move to beginning of next line</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(beginning-of-line 2)</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(setq beg (point))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(if (not (re-search-forward "^--\\[\\[" nil t))</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">(setq end (point-max))</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">;; line up</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(end-of-line 0)</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(setq end (point)))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;; grab body</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(setq text (buffer-substring-no-properties beg end))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;; convert to html</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(with-temp-buffer</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(org-mode)</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(insert text)</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">;; handle signature</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(when (re-search-backward "^-- \n" nil t)</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">;; preserve link breaks in signature</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">(insert "\n#+BEGIN_VERSE\n")</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">(goto-char (point-max))</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">(insert "\n#+END_VERSE\n")</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">;; grab html</span>
<span style="color: #b22222;">;;           </span><span style="color: #b22222;">(setq html (org-export-region-as-html</span>
<span style="color: #b22222;">;;                       </span><span style="color: #b22222;">(point-min) (point-max) t 'string))))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(delete-region beg end)</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(insert</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(concat</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">"--" "&lt;&lt;alternative&gt;&gt;-{\n"</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">"--" "[text/plain]\n" text</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">"--" "[text/html]\n"  html</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">"--" "}-&lt;&lt;alternative&gt;&gt;\n")))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun dmj/wl-send-html-message-toggle ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Toggle sending of html message."</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(interactive)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(setq dmj/wl-send-html-message-toggled-p</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(if dmj/wl-send-html-message-toggled-p</span>
<span style="color: #b22222;">;;             </span><span style="color: #b22222;">nil "HTML"))</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(message "Sending html message toggled %s"</span>
<span style="color: #b22222;">;;            </span><span style="color: #b22222;">(if dmj/wl-send-html-message-toggled-p</span>
<span style="color: #b22222;">;;                </span><span style="color: #b22222;">"on" "off")))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun dmj/wl-send-html-message-draft-init ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Create buffer local settings for maybe sending html message."</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(unless (boundp 'dmj/wl-send-html-message-toggled-p)</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(setq dmj/wl-send-html-message-toggled-p nil))</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(make-variable-buffer-local 'dmj/wl-send-html-message-toggled-p)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(add-to-list 'global-mode-string</span>
<span style="color: #b22222;">;;                </span><span style="color: #b22222;">'(:eval (if (eq major-mode 'wl-draft-mode)</span>
<span style="color: #b22222;">;;                            </span><span style="color: #b22222;">dmj/wl-send-html-message-toggled-p))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun dmj/wl-send-html-message-maybe ()</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Maybe send this message as html message.</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">If buffer local variable `</span><span style="color: #008b8b;">dmj/wl-send-html-message-toggled-p</span><span style="color: #b22222;">' is</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">non-nil, add `</span><span style="color: #008b8b;">dmj/wl-send-html-message</span><span style="color: #b22222;">' to</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">`</span><span style="color: #008b8b;">mime-edit-translate-hook</span><span style="color: #b22222;">'."</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(if dmj/wl-send-html-message-toggled-p</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(add-hook 'mime-edit-translate-hook 'dmj/wl-send-html-message)</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(remove-hook 'mime-edit-translate-hook 'dmj/wl-send-html-message)))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'wl-draft-reedit-hook 'dmj/wl-send-html-message-draft-init)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'wl-mail-setup-hook 'dmj/wl-send-html-message-draft-init)</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(add-hook 'wl-draft-send-hook 'dmj/wl-send-html-message-maybe) </span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun dmj/org-export-region-as-html-attachment (beg end arg)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Export region between BEG and END as html attachment.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">If BEG and END are not set, use current subtree.  Region or</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">subtree is exported to html without header and footer, prefixed</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">with a mime entity string and pushed to clipboard and killring.</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">When called with prefix, mime entity is not marked as</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">attachment."</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(interactive "r\nP")</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(save-excursion</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(let* ((beg (if (region-active-p) (region-beginning)</span>
<span style="color: #b22222;">;;                   </span><span style="color: #b22222;">(progn</span>
<span style="color: #b22222;">;;                     </span><span style="color: #b22222;">(org-back-to-heading)</span>
<span style="color: #b22222;">;;                     </span><span style="color: #b22222;">(point))))</span>
<span style="color: #b22222;">;;            </span><span style="color: #b22222;">(end (if (region-active-p) (region-end)</span>
<span style="color: #b22222;">;;                   </span><span style="color: #b22222;">(progn</span>
<span style="color: #b22222;">;;                     </span><span style="color: #b22222;">(org-end-of-subtree)</span>
<span style="color: #b22222;">;;                     </span><span style="color: #b22222;">(point))))</span>
<span style="color: #b22222;">;;            </span><span style="color: #b22222;">(html (concat "--[text/html"</span>
<span style="color: #b22222;">;;                          </span><span style="color: #b22222;">(if arg "" "\nContent-Disposition: attachment")</span>
<span style="color: #b22222;">;;                          </span><span style="color: #b22222;">"]\n"</span>
<span style="color: #b22222;">;;                          </span><span style="color: #b22222;">(org-export-region-as-html beg end t 'string))))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(when (fboundp 'x-set-selection)</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(ignore-errors (x-set-selection 'PRIMARY html))</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(ignore-errors (x-set-selection 'CLIPBOARD html)))</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">(message "html export done, pushed to kill ring and clipboard"))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">temp fix for heckboxes in orgh-html export</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">disabling cause it messes up my export</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun org-html-checkbox (checkbox)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Format CHECKBOX into HTML."</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(case checkbox (on "&lt;input type=\"checkbox\" checked /&gt;")</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(off "&lt;input type=\"checkbox\" /&gt;")</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(trans "&lt;code&gt;[-]&lt;/code&gt;")</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(t "")))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(defun org-html-format-list-item (contents type checkbox info</span>
<span style="color: #b22222;">;;                                         </span><span style="color: #b22222;">&optional term-counter-id</span>
<span style="color: #b22222;">;;                                         </span><span style="color: #b22222;">headline)</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">"Format a list item into HTML."</span>
<span style="color: #b22222;">;;   </span><span style="color: #b22222;">(let ((checkbox (concat (org-html-checkbox checkbox) (and checkbox " ")))</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(br (org-html-close-tag "br" nil info)))</span>
<span style="color: #b22222;">;;     </span><span style="color: #b22222;">(concat</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(case type</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(ordered</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(let* ((counter term-counter-id)</span>
<span style="color: #b22222;">;;             </span><span style="color: #b22222;">(extra (if counter (format " value=\"%s\"" counter) "")))</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(concat</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(format "&lt;li%s&gt;" extra)</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(when headline (concat headline br)))))</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(unordered</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(let* ((id term-counter-id)</span>
<span style="color: #b22222;">;;             </span><span style="color: #b22222;">(extra (if id (format " id=\"%s\"" id) ""))</span>
<span style="color: #b22222;">;;             </span><span style="color: #b22222;">(chkclass (if checkbox (format " class=\"checkbox\"") "")))</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(concat</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(format "&lt;li%s%s&gt;" extra chkclass)</span>
<span style="color: #b22222;">;;         </span><span style="color: #b22222;">(when headline (concat headline br)))))</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(descriptive</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(let* ((term term-counter-id))</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(setq term (or term "(no term)"))</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">;; Check-boxes in descriptive lists are associated to tag.</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(concat (format "&lt;dt&gt; %s &lt;/dt&gt;"</span>
<span style="color: #b22222;">;;                        </span><span style="color: #b22222;">(concat checkbox term))</span>
<span style="color: #b22222;">;;                </span><span style="color: #b22222;">"&lt;dd&gt;"))))</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(unless (eq type 'descriptive) checkbox)</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">contents</span>
<span style="color: #b22222;">;;      </span><span style="color: #b22222;">(case type</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(ordered "&lt;/li&gt;")</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(unordered "&lt;/li&gt;")</span>
<span style="color: #b22222;">;;        </span><span style="color: #b22222;">(descriptive "&lt;/dd&gt;")))))</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">wl-message-id-domain "smtp.gmail.com")</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">(setq wl-from "Matt Price <a href="mailto:matt.price%40utoronto.ca">&lt;matt.price@utoronto.ca&gt;</a>"</span>

<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;;all system folders (draft, trash, spam, etc) are placed in the</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;;[Gmail]-folder, except inbox. "%" means it's an IMAP-folder</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">wl-default-folder "%inbox"</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">wl-draft-folder   "%[Gmail]/Drafts"</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">wl-trash-folder   "%[Gmail]/Trash"</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">wl-fcc            "%[Gmail]/Sent"</span>

<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;; mark sent messages as read (sent messages get sent back to you and</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;; placed in the folder specified by wl-fcc)</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">wl-fcc-force-as-read    t</span>

<span style="color: #b22222;">;;       </span><span style="color: #b22222;">;;for when auto-compleating foldernames</span>
<span style="color: #b22222;">;;       </span><span style="color: #b22222;">wl-default-spec "%")</span>
</pre>
    </div>
  </div>
</div>