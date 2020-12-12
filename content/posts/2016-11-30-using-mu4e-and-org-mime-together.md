---
title: Using mu4e and org-mime together
author: matt
type: post
date: 2016-11-30T20:45:00+00:00
url: /2016/11/30/using-mu4e-and-org-mime-together/
dsq_thread_id:
  - "5344671858"
categories:
  - emacs

---
I use org-mime in my [grading][1] [system][2], to email my comments on student papers. One frustrating element has always been that messages sent by org-mime were never saved to my sent-mail folder. I realized just recently that this was because I had failed to set emacs&rsquo;s mail-user-agent, which I now do in my initial mu4e setup: 

`(setq mail-user-agent 'mu4e-user-agent)` 

Now org-mime attempts to send messages using mu4e&rsquo;s internal compose functions. Unfortunately some of the information passed by org-mime is in a format that `mu4e~compose-mail` doesn&rsquo;t like, so I had to make some very slight changes to that function: 

<div class="org-src-container">
  <pre class="src src-diff"><span style="color: #333333;">diff --git a/mu4e/mu4e-compose.el b/mu4e/mu4e-compose.el</span>
<span style="color: #333333;">index a24e74a..0c7ec3c 100644</span>
<span style="background-color: #cccccc;">--- </span><span style="background-color: #b3b3b3; font-weight: bold;">a/mu4e/mu4e-compose.el</span>
<span style="background-color: #cccccc;">+++ </span><span style="background-color: #b3b3b3; font-weight: bold;">b/mu4e/mu4e-compose.el</span>
<span style="background-color: #cccccc;">@@ -780,7 +780,14 @@</span><span style="background-color: #cccccc;"> draft message."</span>

<span style="color: #333333;">   ;; add any other headers specified</span>
<span style="color: #333333;">   (when other-headers</span>
<span style="background-color: #ffdddd;">-</span><span style="background-color: #ffdddd;">    (message-add-header other-headers))</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">    (dolist (h other-headers other-headers)</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">      (if (symbolp (car h)) (setcar h (symbol-name (car h))))</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">      (message-add-header (concat (capitalize (car h)) ": " (cdr h) "\n"  ))</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">      )</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">    ;; (dolist (h other-headers)</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">    ;;  (message-add-header h) )</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">    ;;(message-add-header other-headers)</span>
<span style="background-color: #ddffdd;">+</span><span style="background-color: #ddffdd;">    )</span>

<span style="color: #333333;">   ;; yank message</span>
<span style="color: #333333;">   (if (bufferp yank-action)</span>
</pre>
</div>

The commit is in its [my org-mu4e-compose branch, contianing a couple of other fixes][3], and in [its own branch][4] on github, if prefer to pull from there. A patch has been submitted, we&rsquo;ll see what dcjb thinks of it. 

With these changes, org-mime now works perfectly for me.

 [1]: http://matt.hackinghistory.ca/2015/07/15/mailing-subtrees-with-attachments/
 [2]: https://github.com/titaniumbones/Org-Marking-Mode
 [3]: https://github.com/titaniumbones/mu/tree/org-mu4e-compose-stabilization
 [4]: https://github.com/titaniumbones/mu/tree/fix-other-headers