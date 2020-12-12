---
title: Note Taking with PDF Tools
author: matt
type: post
date: 2015-11-11T20:11:00+00:00
url: /2015/11/11/note-taking-with-pdf-tools/
dsq_thread_id:
  - "4316961962"
categories:
  - emacs

---
**NOTE:** This post has been modified as of <span class="timestamp-wrapper"><span class="timestamp">2015-11-22 Sun </span></span> &#x2013; the new code is a little cleaner, and I think the discussion a little fuller. 

Almost all of my job-related reading is now done on a screen. There are still disadvantages &#x2013; I find it much harder to concentrate when reading online &#x2013; but in other ways it is markedly more convenient. 

In particular, it is now much easier to assemble quotations from sources; and now that I&rsquo;ve found [PDF Tools][1], it has become even easier. I&rsquo;ve just started to use it to extract annotations from my PDF&rsquo;s, and it works much better than the lousy command-line hack I was using previously. 

As we&rsquo;re mid-semester, most of my reading is for classes I teach. My current workflow is as follows: 

<ul class="org-ul">
  <li>
    Assemble the relevant readings in a Dropbox-synced directory (ClassName/Readings)
  </li>
  <li>
    Using <a href="http://m.cerience.com/reader/">Repligo Reader</a> (apparently no longer available in the app store?), highlight the passages I&rsquo;m interested in.
  </li>
  <li>
    execute code block (see below) to insert org headings with all highlights from one or more pdfs
  </li>
  <li>
    Assemble reveal.js lecture presentation around those highlights, using <a href="https://github.com/yjwen/org-reveal">org-reveal</a> or Pandoc.
  </li>
</ul>

<div id="outline-container-orgheadline1" class="outline-2">
  <h2 id="orgheadline1">
    Activating PDF Tools
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline1">
    <p>
      Begin by installing pdf-tools and org-pdfview from ELPA with <code>package-list~packages</code> or <code>package-install</code>.
    </p>
    
    <p>
      Then make sure they are activated by adding these lines in your init file:
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span>pdf-tools-install<span style="color: #707183;">)</span>
<span style="color: #707183;">(</span>eval-after-load 'org '<span style="color: #707183;">(</span><span style="color: #a020f0;">require</span> '<span style="color: #008b8b;">org-pdfview</span><span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>add-to-list 'org-file-apps '<span style="color: #707183;">(</span><span style="color: #8b2252;">"\\.pdf\\'"</span> . org-pdfview-open<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>add-to-list 'org-file-apps '<span style="color: #707183;">(</span><span style="color: #8b2252;">"\\.pdf::</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">[[:digit:]]+</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">\\'"</span> . org-pdfview-open<span style="color: #707183;">))</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline2" class="outline-2">
  <h2 id="orgheadline2">
    Switching to PDF Tools for annotating and extracting PDF&rsquo;s
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline2">
    <p>
      Last month Penguim proposed some changes in a <a href="https://github.com/politza/pdf-tools/pull/133">pull request</a>, that export annotations as a set of org headlines. It&rsquo;s potentially very interesting but not quite what I want to do, so I modified this code. <code>pdf-annot-markups-as-org-text</code> extracts the text of an annotation (stored as the <code>subject</code> attribute in an alist), and also generates a link back to the page in the pdf. <code>mwp/pdf-multi-extract</code> is just a helper function that makes it easier to construct elisp source blocks the way I&rsquo;m used to doing:
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">modified from https://github.com/politza/pdf-tools/pull/133 </span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">mwp/pdf-multi-extract</span> <span style="color: #707183;">(</span>sources<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Helper function to print highlighted text from a list of pdf's, with one org header per pdf, </span>
<span style="color: #8b2252;">and links back to page of highlight."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">(</span>
        <span style="color: #707183;">(</span>output <span style="color: #8b2252;">""</span><span style="color: #707183;">))</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">dolist</span> <span style="color: #707183;">(</span>thispdf sources<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> output <span style="color: #707183;">(</span>concat output <span style="color: #707183;">(</span>pdf-annot-markups-as-org-text thispdf nil level <span style="color: #707183;">))))</span>
    <span style="color: #707183;">(</span>princ output<span style="color: #707183;">))</span>
  <span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">this is stolen from https://github.com/pinguim06/pdf-tools/commit/22629c746878f4e554d4e530306f3433d594a654</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">pdf-annot-edges-to-region</span> <span style="color: #707183;">(</span>edges<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Attempt to get 4-entry region \(LEFT TOP RIGHT BOTTOM\) from several edges.</span>
<span style="color: #8b2252;">We need this to import annotations and to get marked-up text, because annotations</span>
<span style="color: #8b2252;">are referenced by its edges, but functions for these tasks need region."</span>

  <span style="color: #707183;">(</span><span style="color: #a020f0;">let</span> <span style="color: #707183;">((</span>left0 <span style="color: #707183;">(</span>nth 0 <span style="color: #707183;">(</span>car edges<span style="color: #707183;">)))</span>
        <span style="color: #707183;">(</span>top0 <span style="color: #707183;">(</span>nth 1 <span style="color: #707183;">(</span>car edges<span style="color: #707183;">)))</span>
        <span style="color: #707183;">(</span>bottom0 <span style="color: #707183;">(</span>nth 3 <span style="color: #707183;">(</span>car edges<span style="color: #707183;">)))</span>
        <span style="color: #707183;">(</span>top1 <span style="color: #707183;">(</span>nth 1 <span style="color: #707183;">(</span>car <span style="color: #707183;">(</span>last edges<span style="color: #707183;">))))</span>
        <span style="color: #707183;">(</span>right1 <span style="color: #707183;">(</span>nth 2 <span style="color: #707183;">(</span>car <span style="color: #707183;">(</span>last edges<span style="color: #707183;">))))</span>
        <span style="color: #707183;">(</span>bottom1 <span style="color: #707183;">(</span>nth 3 <span style="color: #707183;">(</span>car <span style="color: #707183;">(</span>last edges<span style="color: #707183;">))))</span>
        <span style="color: #707183;">(</span>n <span style="color: #707183;">(</span>safe-length edges<span style="color: #707183;">)))</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">we try to guess the line height to move</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">the region away from the boundary and</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">avoid double lines</span>
    <span style="color: #707183;">(</span>list left0
          <span style="color: #707183;">(</span>+ top0 <span style="color: #707183;">(</span>/ <span style="color: #707183;">(</span>- bottom0 top0<span style="color: #707183;">)</span> 2<span style="color: #707183;">))</span>
          right1
          <span style="color: #707183;">(</span>- bottom1 <span style="color: #707183;">(</span>/ <span style="color: #707183;">(</span>- bottom1 top1<span style="color: #707183;">)</span> 2 <span style="color: #707183;">)))))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">pdf-annot-markups-as-org-text</span> <span style="color: #707183;">(</span>pdfpath <span style="color: #228b22;">&optional</span> title level<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Acquire highligh annotations as text, and return as org-heading"</span>

  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span> <span style="color: #8b2252;">"fPath to PDF: "</span><span style="color: #707183;">)</span>  
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>outputstring <span style="color: #8b2252;">""</span><span style="color: #707183;">)</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">the text to be returned</span>
         <span style="color: #707183;">(</span>title <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> title <span style="color: #707183;">(</span>replace-regexp-in-string <span style="color: #8b2252;">"-"</span> <span style="color: #8b2252;">" "</span> <span style="color: #707183;">(</span>file-name-base pdfpath <span style="color: #707183;">))))</span>
         <span style="color: #707183;">(</span>level <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> level <span style="color: #707183;">(</span>1+ <span style="color: #707183;">(</span>org-current-level<span style="color: #707183;">))))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">I guess if we're not in an org-buffer this will fail</span>
         <span style="color: #707183;">(</span>levelstring <span style="color: #707183;">(</span>make-string level ?*<span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">set headline to proper level</span>
         <span style="color: #707183;">(</span>annots <span style="color: #707183;">(</span>sort <span style="color: #707183;">(</span>pdf-info-getannots nil pdfpath<span style="color: #707183;">)</span>  <span style="color: #b22222;">;; </span><span style="color: #b22222;">get and sort all annots</span>
                       'pdf-annot-compare-annotations<span style="color: #707183;">))</span>
         <span style="color: #707183;">)</span>
    <span style="color: #b22222;">;; </span><span style="color: #b22222;">create the header</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> outputstring <span style="color: #707183;">(</span>concat levelstring <span style="color: #8b2252;">" Quotes From "</span> title <span style="color: #8b2252;">"\n\n"</span><span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">create heading</span>

    <span style="color: #b22222;">;; </span><span style="color: #b22222;">extract text</span>
    <span style="color: #707183;">(</span>mapc
     <span style="color: #707183;">(</span><span style="color: #a020f0;">lambda</span> <span style="color: #707183;">(</span>annot<span style="color: #707183;">)</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">traverse all annotations</span>
       <span style="color: #707183;">(</span><span style="color: #a020f0;">if</span> <span style="color: #707183;">(</span>eq 'highlight <span style="color: #707183;">(</span>assoc-default 'type annot<span style="color: #707183;">))</span>
           <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>page <span style="color: #707183;">(</span>assoc-default 'page annot<span style="color: #707183;">))</span>
                  <span style="color: #b22222;">;; </span><span style="color: #b22222;">use pdf-annot-edges-to-region to get correct boundaries of highlight</span>
                  <span style="color: #707183;">(</span>real-edges <span style="color: #707183;">(</span>pdf-annot-edges-to-region
                               <span style="color: #707183;">(</span>pdf-annot-get annot 'markup-edges<span style="color: #707183;">)))</span>
                  <span style="color: #707183;">(</span>text <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> <span style="color: #707183;">(</span>assoc-default 'subject annot<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>assoc-default 'content annot<span style="color: #707183;">)</span>
                            <span style="color: #707183;">(</span>replace-regexp-in-string <span style="color: #8b2252;">"\n"</span> <span style="color: #8b2252;">" "</span> <span style="color: #707183;">(</span>pdf-info-gettext page real-edges nil pdfpath<span style="color: #707183;">)</span>
                                                      <span style="color: #707183;">)</span> <span style="color: #707183;">))</span>

                  <span style="color: #707183;">(</span>height <span style="color: #707183;">(</span>nth 1 real-edges<span style="color: #707183;">))</span> <span style="color: #b22222;">;; </span><span style="color: #b22222;">distance down the page</span>
                  <span style="color: #b22222;">;; </span><span style="color: #b22222;">use pdfview link directly to page number</span>
                  <span style="color: #707183;">(</span>linktext <span style="color: #707183;">(</span>concat <span style="color: #8b2252;">"[[pdfview:"</span> pdfpath <span style="color: #8b2252;">"::"</span> <span style="color: #707183;">(</span>number-to-string page<span style="color: #707183;">)</span> 
                                    <span style="color: #8b2252;">"++"</span> <span style="color: #707183;">(</span>number-to-string height<span style="color: #707183;">)</span> <span style="color: #8b2252;">"]["</span> title  <span style="color: #8b2252;">"]]"</span> <span style="color: #707183;">))</span>
                  <span style="color: #707183;">)</span>
             <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> outputstring <span style="color: #707183;">(</span>concat outputstring text <span style="color: #8b2252;">" ("</span>
                                        linktext <span style="color: #8b2252;">", "</span> <span style="color: #707183;">(</span>number-to-string page<span style="color: #707183;">)</span> <span style="color: #8b2252;">")\n\n"</span><span style="color: #707183;">))</span>
             <span style="color: #707183;">)))</span>
     annots<span style="color: #707183;">)</span>
    outputstring <span style="color: #b22222;">;; </span><span style="color: #b22222;">return the header</span>
    <span style="color: #707183;">)</span>
  <span style="color: #707183;">)</span>
</pre>
    </div>
  </div>
</div>

<div id="outline-container-orgheadline3" class="outline-2">
  <h2 id="orgheadline3">
    Using in Org with a Source Block
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline3">
    <p>
      Now it&rsquo;s more or less trivial to quickly generate the org headers using a source block:
    </p>
    
    <pre class="example">
#+BEGIN_SRC elisp :results output raw :var level=(1+ (org-current-level))
(mwp/pdf-multi-extract '(
                   "/home/matt/HackingHistory/readings/Troper-becoming-immigrant-city.pdf"  "/home/matt/HackingHistory/readings/historical-authority-hampton.pdf"))

#+END_SRC
</pre>
    
    <p>
      And the output gives something like
    </p>
    
    <p>
      #+BEGIN_EXAMPLE
    </p>
  </div>
</div>

<div id="outline-container-orgheadline4" class="outline-2">
  <h2 id="orgheadline4">
    Quotes From Troper becoming immigrant city
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline4">
    <p>
      Included in the Greater Toronto Area multiethnic mix are an estimated 450,000 Chinese, 400,000 Italians, and 250,000 African Canadians, the largest component of which are ofCar- ibbean background, although a separate and distinct infusion of Soma- lis, Ethiopians, and other Africans is currently taking place. (<a href="http://matt.hackinghistory.ca/wp-content/uploads/2015/11/wpid-Troper-becoming-immigrant-city.pdf">Troper becoming immigrant city</a>, 3)
    </p>
    
    <p>
      Although Toronto is Canada&rsquo;s leading immigrant-receiving centre, city officials have neither a hands-on role in immigrant selection nor an official voice in deciding immigration policy. In Canada, immigration policy and administration is a constitutional responsibility of the fed- eral government, worked out in consultation with the provinces. (<a href="http://matt.hackinghistory.ca/wp-content/uploads/2015/11/wpid-Troper-becoming-immigrant-city.pdf">Troper becoming immigrant city</a>, 4)
    </p>
    
    <p>
      #+END_EXAMPLE
    </p>
  </div>
</div>

<div id="outline-container-orgheadline5" class="outline-2">
  <h2 id="orgheadline5">
    Alternative: Temporary buffer with custom link type
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline5">
    <p>
      An alternative workflow would be to pop to a second, temporary buffer and insert the annotations there; one could do this with a custom link type. PDF-Tools already has a mechanism for listing annotations in a separate buffer, but it&rsquo;s not designed for quick access to all annotations at once. Anyway, here&rsquo;s one way to do this; I&rsquo;m not really using it at the moment.
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span>org-add-link-type <span style="color: #8b2252;">"pdfquote"</span> 'org-pdfquote-open 'org-pdfquote-export<span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-pdfquote-open</span> <span style="color: #707183;">(</span>link<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Open a new buffer with all markup annotations in an org headline."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>pop-to-buffer
   <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"*Quotes from %s*"</span>
           <span style="color: #707183;">(</span>file-name-base link<span style="color: #707183;">)))</span>
  <span style="color: #707183;">(</span>org-mode<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>erase-buffer<span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span>insert <span style="color: #707183;">(</span>pdf-annot-markups-as-org-text link nil 1<span style="color: #707183;">))</span>
  <span style="color: #707183;">(</span>goto-char 0<span style="color: #707183;">)</span>
  <span style="color: #707183;">)</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-pdfquote-export</span> <span style="color: #707183;">(</span>link description format<span style="color: #707183;">)</span>
  <span style="color: #8b2252;">"Export the pdfview LINK with DESCRIPTION for FORMAT from Org files."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">let*</span> <span style="color: #707183;">((</span>path <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>string-match <span style="color: #8b2252;">"</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">(</span><span style="color: #8b2252;">.+</span><span style="color: #8b2252; font-weight: bold;">\\</span><span style="color: #8b2252; font-weight: bold;">)</span><span style="color: #8b2252;">::.+"</span> link<span style="color: #707183;">)</span>
                 <span style="color: #707183;">(</span>match-string 1 link<span style="color: #707183;">)))</span>
         <span style="color: #707183;">(</span>desc <span style="color: #707183;">(</span><span style="color: #a020f0;">or</span> description link<span style="color: #707183;">)))</span>
    <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>stringp path<span style="color: #707183;">)</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">setq</span> path <span style="color: #707183;">(</span>org-link-escape <span style="color: #707183;">(</span>expand-file-name path<span style="color: #707183;">)))</span>
      <span style="color: #707183;">(</span><span style="color: #a020f0;">cond</span>
       <span style="color: #707183;">((</span>eq format 'html<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"&lt;a href=\"%s\"&gt;%s&lt;/a&gt;"</span> path desc<span style="color: #707183;">))</span>
       <span style="color: #707183;">((</span>eq format 'latex<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"\href{%s}{%s}"</span> path desc<span style="color: #707183;">))</span>
       <span style="color: #707183;">((</span>eq format 'ascii<span style="color: #707183;">)</span> <span style="color: #707183;">(</span>format <span style="color: #8b2252;">"%s (%s)"</span> desc path<span style="color: #707183;">))</span>
       <span style="color: #707183;">(</span>t path<span style="color: #707183;">)))))</span>

<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">org-pdfquote-complete-link</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"Use the existing file name completion for file.</span>
<span style="color: #8b2252;">Links to get the file name, then ask the user for the page number</span>
<span style="color: #8b2252;">and append it."</span>                                  

  <span style="color: #707183;">(</span>replace-regexp-in-string <span style="color: #8b2252;">"^file:"</span> <span style="color: #8b2252;">"pdfquote:"</span> <span style="color: #707183;">(</span>org-file-complete-link<span style="color: #707183;">)))</span>
</pre>
    </div>
    
    <p>
      I&rsquo;ve also added two bindings to make highlighting easier from the PDF buffer:
    </p>
    
    <div class="org-src-container">
      <pre class="src src-emacs-lisp"><span style="color: #707183;">(</span>eval-after-load 'pdf-view 
                    '<span style="color: #707183;">(</span>define-key pdf-view-mode-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"M-h"</span><span style="color: #707183;">)</span> 'pdf-annot-add-highlight-markup-annotation<span style="color: #707183;">))</span>
<span style="color: #707183;">(</span>eval-after-load 'pdf-view 
                    '<span style="color: #707183;">(</span>define-key pdf-view-mode-map <span style="color: #707183;">(</span>kbd <span style="color: #8b2252;">"&lt;tab&gt;"</span><span style="color: #707183;">)</span> 'pdf-annot-add-highlight-markup-annotation<span style="color: #707183;">))</span>
</pre>
    </div>
    
    <p>
      All of this is getting me very close to using Emacs for all my PDF work. I am doing maybe 50% of my PDF work in Emacs instead of on my tablet. It&rsquo;s incredibly convenient, although I still find it a little harder to concentrate on my laptop than on the tablet (for reasons ergonomic, optical, and psychological). Here are the remaining papercuts from my perspective:
    </p>
    
    <ul class="org-ul">
      <li>
        Highlighting text with the mouse is more awkward and less intimate than using my fingertip on a laptop. I often find mouse movement a little awkward in Emacs, but pdf-view purposely relies on the mouse for movement (for good reasons).
      </li>
      <li>
        Scrolling in pdf-view is also a bit awkward, and there&rsquo;s no &ldquo;continuous&rdquo; mode as one might find in Evince or acroread. Again, I often find scrolling an issue in Emacs, so this might not be so easy to fix.
      </li>
      <li>
        Finally, the laptop screen is just harder on my eyes than my high-res tablet. pdf-view hasa &ldquo;midnight mode&rdquo; which makes it a little easier to read, but it&rsquo;s not quite enough.
      </li>
    </ul>
    
    <p>
      So, for the time being I will probably do much of my reading on the tablet. But for short pieces and for review (e.g., papers that I&rsquo;m reading for the third year in a row in a graduate seminar&#x2026;) PDF Tools is now my main interface. Which is pretty sweet.
    </p>
  </div>
</div>

<div id="outline-container-orgheadline6" class="outline-2">
  <h2 id="orgheadline6">
    Todo
  </h2>
  
  <div class="outline-text-2" id="text-orgheadline6">
    <p>
      <b>UPDATE:</b> <del>I would like to extend the pdfview link type (in org-pdfview) to permit me to specify the precise location of an annotation, so I can jump precisely to that part of the page.</del> This has now been <a href="https://github.com/markus1189/org-pdfview/pull/7">done</a> and the code above has been updated to reflect the new syntax.
    </p>
    
    <p>
      <b>UPDATE:</b> <del>Also, now that I think about it, it might be interesting to just have a link type that pops up a temporary buffer with all of the annotations; I could then cut and paste the annotations into the master document. This might be even more convenient.</del> OK, I&rsquo;ve implemented this, see above!
    </p>
    
    <p>
      <b>Update <span class="timestamp-wrapper"><span class="timestamp">2015-11-22 Sun</span></span>:</b> I&rsquo;ve cleaned up some of the code, and added a bit more commentary at the end.
    </p>
  </div>
</div>

 [1]: https://github.com/politza/pdf-tools