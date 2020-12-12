---
title: Mailing subtrees with Attachments
author: matt
type: post
date: 2015-07-15T19:17:00+00:00
url: /2015/07/15/mailing-subtrees-with-attachments/
dsq_thread_id:
  - "4317252438"
categories:
  - emacs

---
This is pretty exciting; I&rsquo;ve figured out how to quickly send org-mode subtrees as MIME-encoded emails. That means that, essentially, I can write in org, as plain text, and very quickly export to HTML, add attachments, and send. The exciting part about this for me is that it should streamline my communications with students, while also letting me stay in Org and keep my records in order. Let&rsquo;s walk through the process. 

<div id="outline-container-orgheadline1" class="outline-2">
  <h2 id="orgheadline1">
    Use-case
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline1">
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
</div>

<div id="outline-container-orgheadline2" class="outline-2">
  <h2 id="orgheadline2">
    Org-mime
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline2">
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
</div>

<div id="outline-container-orgheadline3" class="outline-2">
  <h2 id="orgheadline3">
    Fix htmlization
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline3">
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
</div>

<div id="outline-container-orgheadline4" class="outline-2">
  <h2 id="orgheadline4">
    Actually perform the export!
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline4">
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
</div>

<div id="outline-container-orgheadline5" class="outline-2">
  <h2 id="orgheadline5">
    Contacts
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline5">
    <p>
      That&rsquo;s a good start, but there are still some steps to make this truly convenient. For instance, I certainly don&rsquo;t want to type in students&rsquo; email addresses by hand. So I imported my contacts from thunderbird to org-contacts. This was a pain &#x2013; the process was Thunderbird &rarr; Gmail (via gsync plugin) &rarr; vcard (via gmail export) &rarr; org-contacts (via <a href="https://gist.github.com/tmalsburg/9747104">Titus&#8217;s python importer</a>). I wish there was a CSV importer for org-contacts; probably this would be easy to write but I&rsquo;m so slooooowwww at coding. My org contacts live in <code>GTD/Contacts.org</code>, which is set in <code>Customize</code>, and org reads them on startup with this line
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-contacts</span><span style="color: #707183;">)</span>
</pre>
    </div>
    
    <p>
      With this single line, org-contacts now provides <kbd>TAB</kbd> completion in message-mode to headers. It&rsquo;s very fast, so feels more convenient than Thunderbird.
    </p>
    
    <dl class="org-dl">
      <dt>
        Making it better
      </dt>
      
      <dd>
        I wish I could get org-contacts to provide tab completion in my subtrees (see below). I would need to access the completion function directly and somehow set the binding for <kbd>TAB</kbd> to that completion function.
      </dd>
    </dl>
  </div>
</div>

<div id="outline-container-orgheadline6" class="outline-2">
  <h2 id="orgheadline6">
    <a id="ID-7c9d8afa-0b2d-4be6-94b4-b586b4ea3bd7"></a>Adding Attachments with Drag & Drop
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline6">
    <p>
      After I make inline comments, I fill out a grading template and attach the paper to the resultant subtree (<kbd>C-c C-a a PATH</kbd>). This is OK, but sometimes it would nice to be able to drag and drom the files, so I am working on these functions.
    </p>
    
    <dl class="org-dl">
      <dt>
        Even better
      </dt>
      
      <dd>
        an even better solution be to add the attachments programmatically. The studnet papes follow a strict naming convention, so I should be able to crawl the directory and find the most recent paper with the student&rsquo;s name in it&#x2026; I&rsquo;m worried it wil lbe too error prone though.
      </dd>
    </dl>
    
    <p>
      Anyway: unfortunately the following code doesn&rsquo;t work right, so <b>don&#8217;t just cut and paste this code!). I *ought</b> to be able to bind the drag and drop action to a function &#x2013; even several functions &#x2013; and, if conditions are right, attach the dragged file to the current org header. John Kitchin describes this method <a href="http://kitchingroup.cheme.cmu.edu/blog/2015/07/10/Drag-images-and-files-onto-org-mode-and-insert-a-link-to-them/">here</a>. But I do the following instead, which is also broken right now:
    </p>
    
    <p>
      Start by loading org-download, which downloads dragged images as attachments and inserts a link. (yay). THen a modification which fixes handling of file links allowing me to drag-n-drop files links onto org as attachments. Unfortunately, I can&rsquo;t get org-attach to process the URI&rsquo;s properly. Darn it.
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
    <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>img-regexp <span style="color: #8b2252;">"</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">png$</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">|</span><span style="color: #8b2252;">jp[e]?g$</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">"</span><span style="color: #707183;">)</span>
          <span style="color: #707183;">(</span>newuri <span style="color: #707183;">(</span>replace-regexp-in-string <span style="color: #8b2252;">"file:///"</span> <span style="color: #8b2252;">"/"</span> uri<span style="color: #707183;">)))</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">cond</span> <span style="color: #707183;">((</span>eq major-mode 'org-mode<span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span>message <span style="color: #8b2252;">"Hi! newuri: %s "</span> <span style="color: #707183;">(</span>file-relative-name newuri<span style="color: #707183;">))</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">cond</span> <span style="color: #707183;">((</span>string-match img-regexp newuri<span style="color: #707183;">)</span>
                    <span style="color: #707183;">(</span>insert <span style="color: #8b2252;">"#+ATTR_ORG: :width 300\n"</span><span style="color: #707183;">)</span>
                    <span style="color: #707183;">(</span>insert <span style="color: #707183;">(</span>concat  <span style="color: #8b2252;">"#+CAPTION: "</span> <span style="color: #707183;">(</span>read-input <span style="color: #8b2252;">"Caption: "</span><span style="color: #707183;">)</span> <span style="color: #8b2252;">"\n"</span><span style="color: #707183;">))</span>
                    <span style="color: #707183;">(</span>insert <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"[[%s]]"</span> uri<span style="color: #707183;">))</span>
                    <span style="color: #707183;">(</span>org-display-inline-images t t<span style="color: #707183;">))</span>
                   <span style="color: #707183;">(</span>t 
                    <span style="color: #707183;">(</span>org-attach-new newuri<span style="color: #707183;">)</span>
                    <span style="color: #707183;">(</span>insert <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"[[%s]]"</span> uri<span style="color: #707183;">))))</span>
             <span style="color: #707183;">)</span>
            <span style="color: #707183;">(</span>t
             <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>dnd-protocol-alist
                    <span style="color: #707183;">(</span>rassq-delete-all
                     'mwp-org-file-link-dnd
                     <span style="color: #707183;">(</span>copy-alist dnd-protocol-alist<span style="color: #707183;">))))</span>
               <span style="color: #707183;">(</span>dnd-handle-one-url nil action uri<span style="color: #707183;">)))</span>
            <span style="color: #707183;">)))</span>

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

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp-org-file-link-disable</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Enable file drag and drop attachments."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>eq <span style="color: #707183;">(</span>cdr <span style="color: #707183;">(</span>assoc <span style="color: #8b2252;">"^</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">file</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">://"</span> dnd-protocol-alist<span style="color: #707183;">))</span>
              'mwp-org-file-link-dnd<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span>rassq-delete-all
       'mwp-org-file-link-dnd
       dnd-protocol-alist<span style="color: #707183;">)</span>

    <span style="color: #707183;">))</span>

<span style="color: #707183;">(</span>mwp-org-file-link-enable<span style="color: #707183;">)</span>
</pre>
    </div>
  </div>
</div>