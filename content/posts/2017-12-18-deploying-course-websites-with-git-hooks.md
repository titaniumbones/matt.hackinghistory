---
title: Deploying Course Websites with git hooks.
author: matt
type: post
date: 2017-12-18T22:17:00+00:00
url: /2017/12/18/deploying-course-websites-with-git-hooks/
dsq_thread_id:
  - "6357398391"
categories:
  - General

---
<nav id="table-of-contents"> 

## Table of Contents

<div id="text-table-of-contents">
  <ul>
    <li>
      <a href="#org1796560">Repository setup</a>
    </li>
    <li>
      <a href="#orgabfdcd7">Workflow</a>
    </li>
  </ul>
</div></nav> 

Now that I&rsquo;ve moved my course websites to Hugo via ox-hugo, life is **definitely** easier than when I used WordPress, and my teaching is unquestionably crawling a little bit closer to actual reproducibility. It&rsquo;s been non-trivial to figure out a good deployment process, though. My course materials live in a slightly broader context than my websites: I have my readings, my private notes, my grades, and a few other documents like teaching contracts, etc., in the main course directory. Some but not all of these materials can be committed to Git, so it makes intuitive sense to me to have a course repository which exists independently of the website. If I didn&rsquo;t use [ox-hugo][1] to manage the org-to-hugo process &#x2013; if, say, I used the somewhat more limited [googrgeous][2] support which is built into Hugo &#x2013; I might try to build the website directly from the course materials. Instead, I&rsquo;ve come up with a somewhat more convoluted process. It&rsquo;s by no means perfect, and there are probably better solutions , but for now this process is working pretty well for me. Documenting here in case it&rsquo;s useful to anyone else, and also in case I forget what I&rsquo;ve done! 

For this post I&rsquo;ll take my [Digital History][3] course as an example, but I&rsquo;m hoping to move all my courses to the same system, semester by semester. 

<div id="outline-container-org1796560" class="outline-2">
  <h2 id="org1796560">
    Repository setup
  </h2>
  
  <div class="outline-text-2" id="text-org1796560">
    <p>
      Each course has two repositories:
    </p>
    
    <ul class="org-ul">
      <li>
        <a href="https://github.com/titaniumbones/Digital-History">a main repo</a>, mostly for my own use, but potentially of use to colleagues. These sites are small, with one file per content type &#x2013; usually one each for syllabus, assignments, lectures, and some kind of &ldquo;other&rdquo; category.
      </li>
      <li>
        <a href="https://github.com/DigitalHistory/dh-website">a website repo</a>, containing one file per webpage &#x2013; so instead of a single <code>Assignments.org</code>, an entry for each individual assignment. These are markdown files created by <code>org-hugo-export-subtree</code>.
      </li>
    </ul>
    
    <p>
      The website repo has two relevant branches: a <code>master</code> branch that contains the uncompiled source files, and a <code>gh-pages</code> branch that holds the Hugo-generated files that are actually served on the server. The gh-pages branch is checked out as a git worktree <a href="https://gohugo.io/hosting-and-deployment/hosting-on-github/">as described in the Hugo docs</a>. It took me a little while to figure this out, but once you understand the concept of git worktrees it&rsquo;s kind of fun to do things this way.
    </p>
    
    <p>
      For Digital History, I&rsquo;m using the excellent <a href="http://docdock.netlify.com/">docdock theme</a>, and as they suggest, <a href="http://docdock.netlify.com/getting-start/installation/#1-install-docdock-as-git-submodule">I&rsquo;ve installed the theme as a git submodule</a>.
    </p>
  </div>
</div>

<div id="outline-container-orgabfdcd7" class="outline-2">
  <h2 id="orgabfdcd7">
    Workflow
  </h2>
  
  <div class="outline-text-2" id="text-orgabfdcd7">
    <p>
      I do all my editing inside Emacs, in the original org-mode files that live in the &ldquo;main&rdquo; repo. Usually changes belong to one of two categories:
    </p>
    
    <ul class="org-ul">
      <li>
        &ldquo;features&rdquo; &#x2013; planned updates to assignments, course materials, due dates, etc.
      </li>
      <li>
        &ldquo;hotfixes&rdquo; &#x2013; corrections to errors, omissions, duplications in the course materials. I have to do these more often than I&rsquo;d like to admit!
      </li>
    </ul>
    
    <p>
      In either case, the workflow <b>should</b> look like this:
    </p>
    
    <ul class="org-ul">
      <li>
        make the change in source (.org) text
      </li>
      <li>
        export change to Hugo
      </li>
      <li>
        inspect changes on local server
      </li>
      <li>
        if changes look good, commit them to master in the main site
      </li>
      <li>
        commit those changes to the web master
      </li>
      <li>
        run <code>hugo -s</code> in the website master dir; commit changes to <code>gh-pages</code> branch; push <code>gh-pages</code> to upstream.
      </li>
    </ul>
    
    <p>
      There are a lot of steps there! Fortunately we can automate or semi-automate them.
    </p>
    
    <p>
      I&rsquo;ve left the first two as manual stages, because it&rsquo;s smart to check changes before committing. However, I now make it a little easier by <a href="https://github.com/kaushalmodi/ox-hugo/issues/109#issuecomment-352510810">following Kaushal&rsquo;s advice</a> to run hugo server with the <code>-p</code> and <code>--navigateToChanged</code> switches. I now keep a pinned tab up in Firefox at <code>localhost:xxxx</code> (choose your own port with <code>-p</code>), and as soon as I export a page, that tab will update to the appropriate page. Fantastic! Makes checking my work much quicker. As for the rest, those steps are all done via 2 git <code>post-commit</code> hooks, one for each repo:
    </p>
    
    <ul class="org-ul">
      <li>
        in the main repo, I have a post-commit hook that checks if the commit was made on the master branch, and if so, then grabs the commit message & commit hash, then cd&rsquo;s to the website directory & creates a new commit on master with references to the original commit.
      </li>
      <li>
        in the web repo, I have a second post-commit hook, which again checks if we&rsquo;re on master, and if so, generates the website in the <code>public</code> directory (where we have the gh-pages site checked out as a git subtree), cd&rsquo;s there, and commits to the gh-pages brnach. Then both branches are pushed to gh-pages
      </li>
      <li>
        I&rsquo;m in a transitional period where I&rsquo;m moving from self-hosted sites to gh-pages where possible. So I actually regenerate the site in another directory using <code>hugo server -s -d myserver</code>; I also have to update <code>config.toml</code> before doing this to fix a couple of paths. This is all pretty easy to do.
      </li>
    </ul>
    
    <p>
      Here&rsquo;s the first, somewhat simpler script from the main repo:
    </p>
    
    <div class="org-src-container">
      <pre class="src src-sh"><span style="color: #a0522d;">push_branch</span>=master
<span style="color: #a0522d;">cur_branch</span>=$<span style="color: #707183;">(</span>git rev-parse --abbrev-ref HEAD<span style="color: #707183;">)</span>
<span style="color: #a0522d;">cur_hash</span>=$<span style="color: #707183;">(</span>git log -1 --pretty=%h<span style="color: #707183;">)</span>
<span style="color: #a0522d;">cur_msg</span>=$<span style="color: #707183;">(</span>git log -1 --pretty=%B<span style="color: #707183;">)</span>
<span style="color: #a0522d;">web_dir</span>=dh-website
<span style="color: #a0522d;">cur_dir</span>=$<span style="color: #a0522d;">PWD</span>

<span style="color: #b22222;"># </span><span style="color: #b22222;">only run if commit is to master</span>

<span style="color: #a020f0;">if</span> <span style="color: #707183;">[</span> <span style="color: #8b2252;">"$cur_branch"</span> ==  <span style="color: #8b2252;">"$push_branch"</span> <span style="color: #707183;">]</span>; <span style="color: #a020f0;">then</span>
    <span style="color: #b22222;"># </span><span style="color: #b22222;">if [ "$(git status -uno --porcelain)" ]; then</span>
    <span style="color: #b22222;">#   </span><span style="color: #b22222;">echo "uncommitted  changes, can't push";</span>
  <span style="color: #b22222;"># </span><span style="color: #b22222;">else</span>
  <span style="color: #b22222;"># </span><span style="color: #b22222;">cd to website</span>
  <span style="color: #483d8b;">cd</span> $<span style="color: #a0522d;">web_dir</span>
  <span style="color: #b22222;"># </span><span style="color: #b22222;">commit hcanges</span>
  <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"committing changes to  website master branch\n\n"</span>
  git commit -a -m <span style="color: #8b2252;">"push changes to website from titaniumbones/Digital-History#$cur_hash</span>

<span style="color: #8b2252;">$cur_msg"</span> 
  <span style="color: #483d8b;">cd</span> $<span style="color: #a0522d;">cur_dir</span>
  <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"pushing master to origin"</span>
  git push -f origin master
<span style="color: #a020f0;">fi</span>

</pre>
    </div>
    
    <p>
      And the slightly more baroque version from the website repo, which pushes to both gh-pages and to hackinghistory (<code>shimano</code> is just an alias for hackinghistory.ca).
    </p>
    
    <p>
      Note that I had to set up the hackinghistory repo as a non-bare repo with <code>receive.denyCurrentBranch</code> set to <code>updateInstead</code>. <a href="https://stackoverflow.com/a/28381311/1220983">Thank you, stackoverflow!</a>.
    </p>
    
    <div class="org-src-container">
      <pre class="src src-sh"><span style="color: #a0522d;">push_branch</span>=master
<span style="color: #a0522d;">cur_branch</span>=$<span style="color: #707183;">(</span>git rev-parse --abbrev-ref HEAD<span style="color: #707183;">)</span>
<span style="color: #a0522d;">cur_hash</span>=$<span style="color: #707183;">(</span>git log -1 --pretty=%h<span style="color: #707183;">)</span>
<span style="color: #a0522d;">cur_msg</span>=$<span style="color: #707183;">(</span>git log -1 --pretty=%B<span style="color: #707183;">)</span>



<span style="color: #b22222;"># </span><span style="color: #b22222;">only run if commit is to master</span>

<span style="color: #a020f0;">if</span> <span style="color: #707183;">[</span> <span style="color: #8b2252;">"$cur_branch"</span> ==  <span style="color: #8b2252;">"$push_branch"</span> <span style="color: #707183;">]</span>; <span style="color: #a020f0;">then</span>
    <span style="color: #a020f0;">if</span> <span style="color: #707183;">[</span> <span style="color: #8b2252;">"$(git status -uno --porcelain)"</span> <span style="color: #707183;">]</span>; <span style="color: #a020f0;">then</span>
      <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"uncommitted  changes, can't push"</span>;
    <span style="color: #a020f0;">else</span>
      <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"pushing master to origin"</span>
      git push -f origin master
      <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"Rebuilding Site"</span>
      hugo -s ./
      <span style="color: #483d8b;">cd</span> public &&
        <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"changing env variables"</span> &&
        <span style="color: #a0522d;">GIT_WORK_TREE</span>=<span style="color: #8b2252;">"../.git/worktrees/public"</span> &&
        <span style="color: #a0522d;">GIT_INDEX_FILE</span>=<span style="color: #8b2252;">"../.git/worktrees/public/index"</span>&&
        <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"adding all files\n\n"</span>  &&
        git add --all
      <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"commitchanges to  gh-pages\n\n"</span>
        git commit -a -m <span style="color: #8b2252;">"push changes to gh-pages from $cur_hash</span>

<span style="color: #8b2252;">$cur_msg"</span> 
        <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"pushing gh-pages to origin"</span>
        git push -f origin gh-pages && <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"successfully pushed gh-pages"</span>
      <span style="color: #483d8b;">cd</span> ..

      sed -i.gh <span style="color: #8b2252;">'s/baseURL = \"https:\/\/digitalhistory.github.io\/dh-website\/\"/baseURL = \"https:\/\/digital.hackinghistory.ca\/\"/'</span> config.toml
      hugo -s ~/DH/dh-website/ -d ~/DH/dh-website/shimano/

      <span style="color: #b22222;"># </span><span style="color: #b22222;">now shimano dir, shimano-deploy branch, shimano remote</span>
      <span style="color: #483d8b;">cd</span> shimano &&
        <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"changing env variables"</span> &&
        <span style="color: #a0522d;">GIT_WORK_TREE</span>=<span style="color: #8b2252;">"../.git/worktrees/shimano"</span> &&
        <span style="color: #a0522d;">GIT_INDEX_FILE</span>=<span style="color: #8b2252;">"../.git/worktrees/shimano/index"</span>&&
        <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"adding all files\n\n"</span>  &&
        git add --all
      <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"commit changes to shimano\n\n"</span>
      git commit -a -m <span style="color: #8b2252;">"push changes to shimano from $cur_hash</span>

<span style="color: #8b2252;">$cur_msg"</span> 
      <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"pushing shimano-deploy to shimano"</span>
      git push -f shimano shimano-deploy && <span style="color: #483d8b;">echo</span> <span style="color: #8b2252;">"successfully pushed shimano"</span>
      <span style="color: #483d8b;">cd</span> ..

      <span style="color: #b22222;"># </span><span style="color: #b22222;">rsync -azvbP ~/DH/dh-website/shimano/ shimano:/var/www/digital.hackinghistory.ca/</span>
      mv config.toml.gh config.toml
    <span style="color: #a020f0;">fi</span>    
<span style="color: #a020f0;">fi</span>
</pre>
    </div>
  </div>
</div>

 [1]: https://github.com/kaushalmodi/ox-hugo/
 [2]: https://github.com/chaseadamsio/goorgeous
 [3]: https://digital.hackinghistory.ca