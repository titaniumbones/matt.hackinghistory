---
title: Sending HTML Mail with mu4e
author: matt
type: post
date: 2016-11-19T03:19:00+00:00
url: /2016/11/18/sending-html-mail-with-mu4e/
dsq_thread_id:
  - "5315506155"
categories:
  - emacs

---
John Kitchin [has a terrific post][1] detailing some configuration/improvements to mu4e that make it easier to send html mail. This original post is really great, but I ran into quite a bit of trouble following this advice. 

He uses a cool feature of mu4e, `org-mu4e-compose-org-mode`, which toggles the major mode of the message buffer between org-mode (when you&rsquo;re in the message body) and mu4e-compose-mode (when you&rsquo;re in the headers area). With a couple of custom functions, it&rsquo;s easy to convert the org text to html and send a mime-multipart email from Emacs, which is quite convenient. If you add `org-mu4e-compose-org-mode` as a hook to `mu4e-compose-mode`, you can compose in html by default, which is great. 

Unfortunately, `org-mu4e-compose-org-mode` is **deprecated** on account of its instability, and while John doesn&rsquo;t have any problems with it, for me it was unworkable. It turns out that this &ldquo;mode&rdquo; isn&rsquo;t really a standard emacs mode at all &#x2013; instead, it&rsquo;s a sly workaround that trickily adds an internal function to the &rsquo;post-command-hook in the draft buffer and switches major modes based on position. This is a neat hack, but since the function _invokes the major modes directly_, setting thefunction as a hook to `mu4e-compose-mode` leads to some funky, inadvertent looping effects. On my machine, for some reason, those effects send the underlying `message-send` function crazy, and instead of sending directly, I get this amazingly annoying question: 

> Already sent message via mail; resend? (y or n) y 

On its own, that is already annoying; but worse, the sent message doesn&rsquo;t get saved to my Sent folder. Instead, it&rsquo;s lost completely. 

Anyway, after fruitless hours of paying around with this, I realized that the problem could be fixed by adding a new hook to the mu4e compose functions (rather than to the compose _mode_). I&rsquo;ve [submitted those changes as a pull request][2] and hopefully they will be accepted; if not, though, feel free to navigate back to [my branch][3] and pull/install mu from there. With those small changes, I now have frictionless html email working very quickly within emacs, using this small bit of code. It is at least 90% stolen: 

<div class="org-src-container">
  <pre class="src src-emacs-lisp"><span style="color: #b22222;">;; </span><span style="color: #b22222;">this is stolen from John but it didn't work for me until I</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">made those changes to mu4e-compose.el</span>
<span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">htmlize-and-send</span> <span style="color: #707183;">()</span>
  <span style="color: #8b2252;">"When in an org-mu4e-compose-org-mode message, htmlize and send it."</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">interactive</span><span style="color: #707183;">)</span>
  <span style="color: #707183;">(</span><span style="color: #a020f0;">when</span> <span style="color: #707183;">(</span>member 'org~mu4e-mime-switch-headers-or-body post-command-hook<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>org-mime-htmlize<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>org-mu4e-compose-org-mode<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>mu4e-compose-mode<span style="color: #707183;">)</span>
    <span style="color: #707183;">(</span>message-send-and-exit<span style="color: #707183;">)))</span>


<span style="color: #b22222;">;; </span><span style="color: #b22222;">This overloads the amazing C-c C-c commands in org-mode with one more function</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">namely the htmlize-and-send, above.</span>
<span style="color: #707183;">(</span>add-hook 'org-ctrl-c-ctrl-c-hook 'htmlize-and-send t<span style="color: #707183;">)</span>

<span style="color: #b22222;">;; </span><span style="color: #b22222;">Originally, I set the `</span><span style="color: #008b8b;">mu4e-compose-mode-hook</span><span style="color: #b22222;">' here, but</span>
<span style="color: #b22222;">;; </span><span style="color: #b22222;">this new hook works much, much better for me.  </span>
<span style="color: #707183;">(</span>add-hook 'mu4e-compose-post-hook
          <span style="color: #707183;">(</span><span style="color: #a020f0;">defun</span> <span style="color: #0000ff;">do-compose-stuff</span> <span style="color: #707183;">()</span>
            <span style="color: #8b2252;">"My settings for message composition."</span>
            <span style="color: #707183;">(</span>org-mu4e-compose-org-mode<span style="color: #707183;">)))</span>
</pre>
</div>

It feels great to have gotten this far. There are still some small things I&rsquo;d like to be able to improve; and I think I would like to add a few wrapper functions and keybindings to my setup, but for now I&rsquo;m pretty efficient. 

Still missing: 

<ul class="org-ul">
  <li>
    a better HTML viewing interface! right now, html mails render as mostly text &#x2013; it would be nice to have a rendered html message by default in emacs. This is an issue several times a day when I get promotional emails from organizations I work with &#x2013; usually the html part is really important. I can access these in the browser but it&rsquo;s comparatively awkward.
  </li>
  <li>
    a way to forward these html emails to someone intact &#x2013; right now, the html parts are discarded. No idea how hard it would be to do this.
  </li>
</ul>

Thanks John and Dirk-Jan for these great tools!

 [1]: http://kitchingroup.cheme.cmu.edu/blog/2016/10/29/Sending-html-emails-from-org-mode-with-org-mime/
 [2]: https://github.com/djcb/mu/pull/952
 [3]: https://github.com/titaniumbones/mu/tree/org-mu4e-compose-stabilization