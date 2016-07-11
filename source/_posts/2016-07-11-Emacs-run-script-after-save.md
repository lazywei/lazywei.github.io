title: Emacs run script after save
tags:
  - productivity
  - emacs
  - tools
date: 2016-07-11 09:02:37
---


I used Spacemacs, a popular Emacs distribution, for daily work. Recently, I faced an annoying situation. I got some projects that can only be compiled and executed on a remote server. Naturally, I think of TRAMP mode. However, the `ssh` latency to this remote server is so high that every operation in TRAMP mode tooks me about 20+ seconds on average. That is not acceptable.

Therefore, I decided to write the code on my local laptop and then use `rsync` to upload only updated files to remote. It somehow solved my problem, only thing bothered me is I have to run `rsync` every time after modified some code. But hey, I'm a programmer and I'm using Emacs, so it should be quite easy to figure out a solution to this bothersome routine.

Clarify my expectation:
1. Being able to run a script after saving files.
2. Only run this script in certain project.

After doing some search, I found it is possible to
1. Hook a elisp function after save.
2. Utilize the projectile's defined function for locating script.

The solution I come up with is creating a `run_after_save` executable in the root of a project (the definition of a project is defined by projectile), and hook a function that checks for this file and executes it if exists on saving. Here is the resulted snippet:


```lisp
  ;; === After save hook function for projectile ===
  (defun pj-run-after-save ()
    "Run script to sync on save"
    (if (and (projectile-project-p)
             (file-executable-p (projectile-file-truename "run_after_save")))
        (shell-command (projectile-file-truename "run_after_save"))
      )
    )

  (add-hook 'after-save-hook 'pj-run-after-save)
  ;; ===============================================
```

Done!
