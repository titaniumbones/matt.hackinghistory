---
title: It’s Time to Finally Take Encryption Seriously!
author: matt
type: post
date: 2016-11-12T04:00:00+00:00
url: /2016/11/11/its-time-to-finally-take-encryption-seriously/
dsq_thread_id:
  - "5297667398"
categories:
  - General

---
<nav id="table-of-contents"> 

## Table of Contents

<div id="text-table-of-contents">
  <ul>
    <li>
      <a href="#orgd215cc1">Encryption Review</a>
    </li>
    <li>
      <a href="#orgb192000">Messaging</a>
    </li>
    <li>
      <a href="#org25f12fd">Email</a>
    </li>
  </ul>
</div></nav> 

We have known for a long time that the American intelligence agencies don&rsquo;t flinch at violating our privacy. Edward Snowden&rsquo;s revelations have forced some small reforms at the NSA and elsewhere; but those small improvements will doubtless be rolled back, and [X][1]Keyscore, [PRISM][2] and, [ECHELON][3] will be replaced by even more invasive and dangerous technologies. They will be controlled by a government with strong ties to White Nationalists, militias, and various hate groups. In other times and places, governments have circulated death lists of dissenters to paramilitary organizations. I don&rsquo;t think that&rsquo;s likely in the USA; but I also think that as citizens, we need to act to protect ourselves from the abuse of power. 

_The only effective tool against government surveillance is end-to-end encryption of communication._ Even encryption is likely to be inadequate against a targeted attack by the NSA&rsquo;s supercomputers and tactical operations; but when it comes to the mass collection of data, properly-practiced encryption protects you from being targeted for more detailed analysis. As a by-product, encrypting your communications makes it a little bit harder (but not much!) for corporations to track your preferences, and also adds a little bit of protection from other malicious agents. 

The basic building blocks of encrypted communication have been available for a long time, but until recently encryption was clumsy and error-prone. Encryption of email is still somewhat challenging. In the last year, though, two excellent solutions, and one adequate one, have emerged for messaging. I&rsquo;ll talk about all these in a minute, but first let&rsquo;s quickly review how encryption works. 

<div id="outline-container-orgd215cc1" class="outline-2">
  <h2 id="orgd215cc1">
    Encryption Review
  </h2>
  
  <div class="outline-text-2" id="text-orgd215cc1">
    <p>
      Almost all contemporary encryption is &ldquo;dual-key&rdquo; &#x2013; each person in a conversation has both a private and a public &ldquo;key&rdquo;. If I want to send you a message, I encrypt it with <i>your</i> public key. After that, even I can&rsquo;t read my message &#x2013; only you can, with your <i>private</i> key, which you don&rsquo;t show to anyone.
    </p>
    
    <p>
      This sounds simple, but it is actually quite difficult to implement encrypted communication in a simple, easy-to-understand interface. Software developers have struggled with this for a long time. It&rsquo;s worth reading a little more about this:
    </p>
    
    <ul class="org-ul">
      <li>
        <a href="https://ssd.eff.org/">The EFF&rsquo;s Surveillance Self-Defense Guide</a> is a comprehensive introduction to encryption practice. You should read it.
      </li>
      <li>
        <a href="https://emailselfdefense.fsf.org/en/">The FSF also has an Email Self-Defense Guide</a>. Specific to email, but still helpful, and quite a bit shorter.
      </li>
      <li>
        The EFF used to have a <a href="https://www.eff.org/secure-messaging-scorecard">Secure Messaging Scorecard</a>. The first version has been deprecated, but a new one will be launched soon; read it when it comes out. It will likely supersede much of this information.
      </li>
    </ul>
  </div>
</div>

<div id="outline-container-orgb192000" class="outline-2">
  <h2 id="orgb192000">
    Messaging
  </h2>
  
  <div class="outline-text-2" id="text-orgb192000">
    <p>
      The messaging landscape has been changing rapidly. After a long delay, there is now really good encryption available, but some of the interfaces are a little confusing or misleading.
    </p>
    
    <ul class="org-ul">
      <li>
        <b><a href="https://ssd.eff.org/en/module/how-use-signal-android">Signal</a></b> allows you to send text messages and make phone calls with full, end-to-end, dual-key encryption. You should switch to it immediately. You won&rsquo;t be able to make encrypted phone calls over the regular phone network &#x2013; you will need to use data instead.
      </li>
      <li>
        As of April, <b><a href="https://ssd.eff.org/en/module/how-use-whatsapp-android">WhatsApp</a></b> now offers dual-key encryption by default, including a key-verification interface. Among the mainstram apps, WhatsApp is probably the best option, but <a href="https://www.eff.org/deeplinks/2016/10/where-whatsapp-went-wrong-effs-four-biggest-security-concerns">it&rsquo;s not perfect</a>, and Signal is better if you can use it.
      </li>
      <li>
        In June, <b><a href="https://www.wired.com/2016/10/facebook-completely-encrypted-messenger-update-now/">Facebook Messenger</a></b> enabled end-to-end encryption. Encryption is turned off by default and has to be turned on for every conversation, so this is <b>not</b> as good as WhatsApp, but if your social media life mostly moves through Facebook, you <b>can</b> enable encryption, and it&rsquo;s worth doing.
      </li>
      <li>
        Google&rsquo;s new <a href="https://www.eff.org/deeplinks/2016/09/googles-allo-sends-wrong-message-about-encryption">Allo</a> messenging app also permits end-to-end encryption, but it&rsquo;s not on by default, and it&rsquo;s a bit confusing to use. Signal is still ldefinitely preferable.
      </li>
    </ul>
  </div>
</div>

<div id="outline-container-org25f12fd" class="outline-2">
  <h2 id="org25f12fd">
    Email
  </h2>
  
  <div class="outline-text-2" id="text-org25f12fd">
    <p>
      Email is tricky. For most people, <a href="https://enigmail.wiki/Quick_start">Thunderbird plus EnigMail</a> is probably the best option, but it is confusing to use and it also &ldquo;leaks&rdquo; information (as will always be the case for email sent through traditional channels). Various projects to fix email were announced in the wake of the Snowden revelations, but the onse I know about have mostly stopped development. It&rsquo;s a hard nut to crack.
    </p>
    
    <p>
      Still: you should aim to encrypt a relatively high percentage of your emails if you can. It&rsquo;s not good enough to just encrypt a few; the important ones need to be masked by the trivial ones. And encrypting your emails also helps to protect the journalists, activists, ando thers who need to encrypt their communication <i>for their own protection</i>. We encrypt not just for ourselves, but to preserve essential freedoms for others.
    </p>
    
    <p>
      Over the next few months I hope to transition to encryption in most of my messaging; that means that, if you want to be in touch with me online, you&rsquo;ll need to start figuring this stuff out too. Let me know what I can do to help!
    </p>
  </div>
</div>

 [1]: https://en.wikipedia.org/wiki/XKeyscore
 [2]: https://en.wikipedia.org/wiki/PRISM_(surveillance_program)
 [3]: https://en.wikipedia.org/wiki/ECHELON