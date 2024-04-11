---
title:  "Emacs Config"
date:   2024-01-07
url: 'emacs-config'
---

My ~~not so~~ **E**xtremely **M**inimal **A**lt + **C**trl + **S**hift.

**Moved to `Modular Structure` from `Literate Org-Mode` config**

This post doesn't strictly follow the most recent version(*Updated: 2024-04-05*).\
That's available on [Github](https://github.com/ptrtoj/Jemacs.git).

*(Prefer `Built-in` packages over `3rd Party` ones.)*

# JEMACS

> Emacs really does stand for “Escape-Meta-Alt-Control-Shift”,
> and not “Editing Macros”, as you may have heard from
> other disreputable sources (like the Emacs author).
>
> &#x2014; [The Gnus Newsreader Manual > 12.7.1 Keystrokes](https://www.gnu.org/software/emacs/manual/html_node/gnus/Keystrokes.html)

BTW, I prefer all headings fall nicely under the main & top node, so the `H1`.


## Table of Contents     :TOC_5_org:

-   [JEMACS](#orga83489f)
	-   [NOTES](#orgfcc26d9)
		-   [Aren't You Said **Extremely Minimal**?](#org62da705)
		-   [Conventions](#org940b596)
		-   [Comments](#org27c9c66)
		-   [Cross Reference](#org293711b)
		-   [Preview `ORG` file](#orgf8940ac)
		-   [Acknowledge](#org05c5f49)
	-   [INSTALL](#org7ab494b)
		-   [Dependency](#orgf42062f)
		-   [By cloning the repo itself as `.emacs.d`](#orgf17d6d3)
		-   [With symlink](#orgbd3ee05)
	-   [`early-init.el` Part](#org31728c4)
		-   [`package-enable-at-startup`](#orgaaa30da)
		-   [`LSP_USE_PLISTS`](#orgc235d0b)
	-   [`init.el` Part](#orgdcd6f5d)
	-   [Use-Package](#orge19dbd6)
		-   [Initiate](#orgffcafc7)
		-   [Auto Update](#org0093f06)
	-   [Emacs Defaults](#org0cc4cfd)
		-   [`gc-cons-threshold` is too low](#org7703cd9)
		-   [`read-process-output-max` is also too low](#orge8ceacb)
	-   [Fix MacOS Path Issue](#org7c56ce1)
	-   [Personal Functions](#orgf809eec)
	-   [Editing](#org80e4940)
		-   [Revert Automatically](#orgb4a28f2)
		-   [Key Bindings](#org729d73c)
		-   [Disable `custom.el`](#org454430d)
		-   [Abbreviation](#org1f05ee7)
		-   [Delete Selection as I Type](#org28ac0d6)
		-   [Eldoc](#org0b0b3ba)
		-   [Pair Parenthesis Automatically](#org0344c6f)
		-   [Files](#org6dbfb0a)
		-   [Spell Check](#orga62db39)
		-   [`OBSOLETE Show Address as Links`](#org3f510f5)
		-   [Helpful](#org3859d53)
		-   [Recent Files](#orga9f33da)
		-   [Save Command History](#org29e9871)
		-   [Save Last Cursor Position](#orgdf31692)
		-   [Treat `some-long-named-var` as one word](#orgcfef653)
		-   [Show Keys](#org44ef22e)
		-   [Clean White Spaces](#org82751a0)
		-   [Move Windows Easily](#org1c3384f)
	-   [UI](#orgf8314b8)
		-   [Column Indicator](#orgf7465e3)
		-   [Hide Minor Mode](#org9c2deac)
		-   [Show Line Numbers](#org4cd2ed9)
		-   [Frame](#orgbbad639)
		-   [Highlight Cursor Line](#orge5589b1)
		-   [Highlight Indent Level](#org0d3e1fa)
		-   [Show Parenthesis Level](#org782f9c9)
		-   [Remove Scroll Bar](#orgca46437)
		-   [Simple](#orgc5ad1bf)
		-   [Theme](#org5792b7a)
		-   [Fonts](#org579069e)
		-   [Remove Tool Bar](#org2ef2ed0)
		-   [`OBSOLETE Remove Menu Bar`](#org0eb372f)
		-   [`OBSOLETE Remove Tooltips`](#org18ce6bc)
	-   [Dashboard](#org248413f)
		-   [Page Break Lines](#org3706814)
		-   [Nerd Icons](#orgb1c5529)
		-   [Dashboard](#org248413f)
	-   [File Tree](#orgdf1d65d)
		-   [Treemacs](#org280c172)
		-   [Treemacs with Projectile](#orgf59cfad)
		-   [Treemacs with Magit](#orgfe46880)
		-   [Treemacs with LSP](#org1df400a)
	-   [Modeline](#orgc3b99a5)
		-   [Doom Modeline](#orgaa036a8)
		-   [Show Time on Modeline](#org8b711f4)
	-   [Git](#org149f3e8)
		-   [Magit](#org868f7fc)
	-   [Minibuffer Set - SMOCE](#org6a7dc95)
		-   [Vertico](#org75fafea)
		-   [Marginalia](#orgbb0d1f5)
		-   [Orderless](#org26a9e32)
		-   [Embark](#orgf70459d)
		-   [Embark Consult](#org0fed5c0)
		-   [Consult](#org03cdade)
		-   [`OBSOLETE Helm`](#orgfd9241b)
		-   [`OBSOLETE Helm-LSP`](#org902dc3b)
	-   [Completion at Point](#orge372532)
		-   [Company](#org7200577)
		-   [`OBSOLETE Corfu`](#orga0ff011)
	-   [Org Mode](#orga655cc9)
		-   [Org Preview](#orgd19975a)
			-   [ISSUE-240410#preview: unable to resolve link](#org63cb79f)
		-   [Org Bullets](#orgf474bce)
		-   [Org ToC](#orgd0cef78)
			-   [FIX: ISSUE-240410#preview](#org25c8835)
		-   [Org visual fill column](#orga51cd3b)
	-   [Project Manager](#org7bfedcd)
		-   [ag](#org7e799b6)
		-   [rg](#org066301f)
		-   [Projectile](#org4bd026e)
	-   [Snippet](#orge2f4b9b)
		-   [Yasnippet](#org031f0a8)
		-   [Snippets](#org1fbfef2)
	-   [LSP](#orge7a3977)
		-   [LSP-Mode](#org05160ee)
		-   [LSP-UI](#orgea2eddd)
		-   [`OBSOLETE eglot`](#org2d670b7)
	-   [Debug](#org3edd7bf)
		-   [Dap-mode](#org4245b27)
	-   [Language Specifics](#org9a042cf)
		-   [Ada](#org5f668d0)
		-   [C](#org22fd897)
		-   [Markdown](#org87137f3)


## NOTES


### Aren't You Said **Extremely Minimal**?

Trying hard to keep this minimal, but keeps growing :/


### Conventions

Followed "[D.1 Emacs Lisp Coding Conventions](https://www.gnu.org/software/emacs/manual/html_node/elisp/Coding-Conventions.html)" in *Emacs Manual* when using modular structured configuration.


### Comments

Followed "[D.7 Tips on Writing Comments](https://www.gnu.org/software/emacs/manual/html_node/elisp/Comment-Tips.html)" in *Emacs Manual* when using modular structured configuration.

> Three semicolons are used for top-level sections,
> four for sub-sections, five for sub-sub-sections and so on.

Then, you can enable `outline-mode`, and use `outline-show-only-headings`.


### Cross Reference

Follows "[5 Cross-references](https://www.gnu.org/software/texinfo/manual/texinfo/html_node/Cross-References.html)" in *GNU Texinfo Manual*.


### Preview `ORG` file

Preview this `org` file in browser by `C-c`, `C-e`, `h`, `o`, or see [Org Preview](#orgd19975a).


### Acknowledge

Heavily influenced by [purcell/emasc.d](https://github.com/purcell/emacs.d), [bbatsov/prelude](https://github.com/bbatsov/prelude) and [daviwil/emacs-from-scratch](https://github.com/daviwil/emacs-from-scratch).


## INSTALL

In any case, investigate your git repo and fix `.gitignore` to not push
`elpa`, etc. unnecessary files.


### Dependency

-   git
-   hunspell and dictionary files (for 'flyspell')
-   Nerd patched font (for 'nerd-icons')
	-   I'm using manually patched 'BerkeleyMono Nerd Fonts'
-   [fd](https://github.com/sharkdp/fd), [ag](https://github.com/ggreer/the_silver_searcher), [rg](https://github.com/BurntSushi/ripgrep) in System, **AND ALSO** on Emacs (for 'projectile')


### By cloning the repo itself as `.emacs.d`

	$git clone git@github.com:ptrtoj/Jemacs.git ~/.emacs.d


### With symlink

	$cd YOUR/GIT/REPO_DIR
	$git clone git@github.com:ptrtoj/Jemacs.git
	$ln -sv YOUR/GIT/REPO_DIR ~/.emacs.d


## `early-init.el` Part

Quote from '50.4.6 The Early Init File' (from [Emacs Manual](https://www.gnu.org/software/emacs/manual/html_node/emacs/Early-Init-File))

> This file is loaded before the package system and GUI is initialized,
> so in it you can customize variables that affect the package initialization process,
> such as 'package-enable-at-startup', 'package-load-list', and 'package-user-dir'.


### `package-enable-at-startup`

Purcell sets 'package-enable-at-startup' to 'nil' (see [purcell/emacs.d/early-init.el](https://github.com/purcell/emacs.d/blob/master/early-init.el)).

However, Protesilaos disagrees (see <https://github.com/protesilaos/dotfiles/blob/master/emacs/.emacs.d/early-init.el>, Line 77)(
Couldn't feel noticable difference, yet

	(setq package-enable-at-startup nil)


### `LSP_USE_PLISTS`

Removed `(setenv "LSP_USE_PLISTS" "true")`.
Rather added to system 'env' var, because of 'exec-path-from-shell'


## `init.el` Part

Implementation of JEMACS.


## Use-Package


### Initiate

	(require 'use-package)
	(use-package package
	  :bind
	  ("C-c u p" . package-refresh-contents)
	  :config
	  (add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/"))
	  (package-initialize)
	  (unless package-archive-contents
		(package-refresh-contents)))


### Auto Update

Automatically update packages. But can be updated manually by `C-c`, `u`, `g`.

	(use-package auto-package-update
	  :ensure t
	  :bind
	  ("C-c u g" . auto-package-update-now)
	  :config
	  (setq auto-package-update-prompt-before-update t)
	  (setq auto-package-update-delete-old-versions t))


## Emacs Defaults

	(use-package emacs
	  :custom
	  (user-full-name "WooHyung Jeon")
	  (user-mail-address "jeon@ptrtoj.com")
	  ;;(visible-bell t)

	  ;; Obsoleted by using 'dashboard'
	  ;;(inhibit-startup-screen t)
	  ;;(inhibit-startup-message t)
	  ;;(inhibit-startup-echo-area-message t)
	  ;;(initial-major-mode 'fundamental-mode)

	  ;; old way to change to 'y-or-n-p'
	  ;;(defalias 'yes-or-no-p 'y-or-n-p)

	  ;; new and correct way to set 'y-or-n-p'
	  (setopt use-short-answers t)

	  ;; Look two consecutive headings below
	  (gc-cons-threshold (* 4 1024 1024))
	  (read-process-output-max (* 1024 1024)))


### `gc-cons-threshold` is too low

is claimed by `lsp-mode` in this page's [performance section](https://emacs-lsp.github.io/lsp-mode/page/performance/).

About `Garbage Colleciton` see [E.3 Garbage Collection](https://www.gnu.org/software/emacs/manual/html_node/elisp/Garbage-Collection.html) in `Elisp Reference Manual`.


### `read-process-output-max` is also too low

is again claimed by `lsp-mode` in the same  [page](https://emacs-lsp.github.io/lsp-mode/page/performance/).


## Fix MacOS Path Issue

	(use-package exec-path-from-shell
	  :ensure t
	  :config
	  (dolist (var '("LSP_USE_PLISTS"))
		(add-to-list 'exec-path-from-shell-variables var))
	  (when (memq window-system '(mac ns x))
		(exec-path-from-shell-initialize)))


## Personal Functions

Load personal functions from `libj` directory. (see: [Emacs Manual](https://www.gnu.org/software/emacs/manual/html_node/eintr/Loading-Files.html))

	(add-to-list 'load-path "~/.emacs.d/libj")
	(load "utils")


## Editing


### Revert Automatically

See [Emacs Manual](https://www.gnu.org/software/emacs/manual/html_node/emacs/Auto-Revert.html).

	(use-package autorevert
	  :diminish (auto-revert-mode)
	  :custom
	  (global-auto-revert-non-file-buffers t))


### Key Bindings

Personal key bindings, which aren't written directly into the 'use-package' part.
New bindings are left, and replacement or 3rd party package bindings are moved to each package's 'use-package' file.

Considering `General.el`.

	(use-package bind-keys
	  :bind
	  ;; Personal Additions
	  ("C-c k" . describe-personal-keybindings)) ; Show personal keybindings


### Disable `custom.el`

Re-route 'custom' settings to 'custom-file'
Prevent the custom section written in this file.

	(setq custom-file (concat user-emacs-directory "custom.el"))
	(when (file-exists-p custom-file)
	  (load custom-file))


### Abbreviation

Complete and expand abbreviation

	(use-package dabbrev
	  :diminish (abbrev-mode)
	  :bind
	  (("M-/"   . dabbrev-completion)
	   ("C-M-/" . dabbrev-expand))
	  :config
	  (add-to-list 'dabbrev-ignored-buffer-modes 'doc-view-mode)
	  (add-to-list 'dabbrev-ignored-buffer-modes 'pdf-view-mode))


### Delete Selection as I Type

	(use-package delsel
	  :config
	  (delete-selection-mode 1))


### Eldoc

A buffer-local minor mode that helps with looking up documentation of symbols (functions, methods, classes, variables, etc.) in your program

(Seems it automatically turns on 'elisp' files, but 'C' file complains, if LSP such as 'eglot' isn't enabled)

	(use-package eldoc
	  :diminish
	  :hook
	  ;; (emacs-lisp-mode . turn-on-eldoc-mode))
	  (prog-mode . turn-on-eldoc-mode))


### Pair Parenthesis Automatically

	(use-package elec-pair
	  :hook
	  (prog-mode . electric-pair-mode)
	  (org-mode . electric-pair-mode))


### Files

Do not make `*~` backup files

	(use-package files
	  :custom
	  (make-backup-files nil))


### Spell Check

Dependency:

-   'hunspell'
-   'Spelling Files' such as \`\*.aff\` & \`\*.dic\`

	(use-package flyspell
	  :diminish
	  :custom
	  (ispell-local-dictionary "en_US")
	  :hook
	  (text-mode . flyspell-mode)
	  (prog-mode . flyspell-prog-mode))

	;; Telling ispell-mode to use hunspell. Already handled by 'ispell.el'.
	;;(setq ispell-program-name "/opt/homebrew/bin/hunspell")


### `OBSOLETE Show Address as Links`

Show URL as clickable links.

Using, `org-mode` instead of link in `el` file.

	(use-package goto-addr
	  :config
	  (global-goto-address-mode))


### Helpful

	(use-package helpful
	  :ensure ;TODO:
	  :bind
	  ([remap describe-function] . helpful-callable)
	  ([remap describe-variable] . helpful-variable)
	  ([remap describe-key] . helpful-key)
	  ([remap describe-command] . helpful-command)
	  ("C-h j" . helpful-at-point))


### Recent Files

	(use-package recentf
	  :bind
	  ("C-x C-r" . recentf-open-files)
	  :config
	  (recentf-mode 1))


### Save Command History

	(use-package savehist
	  :config
	  (savehist-mode 1))


### Save Last Cursor Position

	(use-package saveplace
	  :config
	  (save-place-mode 1))


### Treat `some-long-named-var` as one word

	(use-package subword
	  :diminish (superword-mode)
	  :config
	  (global-superword-mode 1))


### Show Keys

	(use-package which-key
	  :ensure t
	  :diminish
	  :config
	  (which-key-mode 1))


### Clean White Spaces

	(use-package whitespace
	  :hook
	  (before-save . whitespace-cleanup))


### Move Windows Easily

	(use-package windmove
	  :bind
	  ("C-c w j" . windmove-down)
	  ("C-c w k" . windmove-up)
	  ("C-c w h" . windmove-left)
	  ("C-c w l" . windmove-right))


## UI


### Column Indicator

Show 80-char width

	(use-package display-fill-column-indicator
	  :config
	  ;;(set-face-background 'fill-column-indicator "#ffff00")	; Yellow BG
	  (global-display-fill-column-indicator-mode 1))


### Hide Minor Mode

	(use-package diminish
	  :ensure t)


### Show Line Numbers

	(use-package display-line-numbers
	  :custom
	  (display-line-numbers-type 'relative)
	  (display-line-numbers-width-start t)		; prevent width expand near the three digit
	  :config
	  (global-display-line-numbers-mode 1)
	  (dolist (mode '(term-mode-hook		; don't show in terminals
					  shell-mode-hook
					  eshell-mode-hook
					  treemacs-mode-hook))		; also in filetree
		(add-hook mode (lambda () (display-line-numbers-mode -1)))))


### Frame

	(use-package frame
	  :custom
	  (initial-frame-alist (quote ((fullscreen . maximized)))))


### Highlight Cursor Line

	(use-package hl-line
	  :config
	  ;; if !using-colortheme,
	  ;; then background = lightgray
	  (set-face-background hl-line-face "lightgray")
	  (global-hl-line-mode 1))


### Highlight Indent Level

	(use-package indent-guide
	  :ensure t
	  :diminish
	  :custom
	  (indent-guide-char " ")
	  ;;(indent-guide-delay 0.5)
	  :config
	  (set-face-background 'indent-guide-face "gray90")
	  :hook
	  (prog-mode . indent-guide-mode))


### Show Parenthesis Level

	(use-package rainbow-delimiters
	  :ensure t
	  :hook
	  (prog-mode . rainbow-delimiters-mode))


### Remove Scroll Bar

	(use-package scroll-bar
	  :init
	  (scroll-bar-mode 1))


### Simple

	(use-package simple
	  :config
	  (column-number-mode 1))


### Theme

'Emacs', the only true & correct editor defaults to light-theme, but if it's boring, use below

	;;;; Built-in 'modus' themes
	;; (use-package custom
	;;   :config
	;;   (load-theme 'modus-operandi t))
	  ;;(load-theme 'modus-vivendi t)

	;;;; Nord
	;; (use-package nord-theme
	;;   :ensure t
	;;   :config
	;;   (load-theme 'nord t))

	;;;; Catppuccin
	(use-package catppuccin-theme
	  :ensure t
	  :config
	  (load-theme 'catppuccin t)
	  (setq catppuccin-flavor 'latte)
	  (catppuccin-reload))


### Fonts

Should come after 'theme', see 'Commentary - [FIXED] ISSUE: 2024-04-09'

[FIXED] ISSUE: 2024-04-09
'Theme' resets 'font-lock-keyword-face' and 'font-lock-comment-face'.
So these should be placed under the 'theme.el'.

	(use-package faces
	  :config
	  (set-face-attribute 'font-lock-keyword-face nil :weight 'bold)
	  (set-face-attribute 'font-lock-comment-face nil :slant 'italic)
	  (add-to-list 'default-frame-alist '(font . "BerkeleyMono Nerd Font")))


### Remove Tool Bar

	(use-package tool-bar
	  :config
	  (tool-bar-mode -1))


### `OBSOLETE Remove Menu Bar`

On MacOS, menu-bar doesn't bother me

	(use-package menu-bar
	  :config
	  (menu-bar-mode -1))


### `OBSOLETE Remove Tooltips`

Tooltips are actually quite helpful

	(use-package tooltip
	  :config
	  (tooltip-mode -1))


## Dashboard


### Page Break Lines

	(use-package page-break-lines
	  :ensure t)


### Nerd Icons

	(use-package nerd-icons
	  :ensure t
	  :custom
	  (nerd-icons-font-family "BerkeleyMono Nerd Font"))


### Dashboard

ISSUE: 2024-04-06
Can't find 'octicon', maybe caused by/with 'all-the-icons'
→ even same with 'nerd-icons'

Dependency:

-   page-break-lines
-   nerd-icons

	(use-package dashboard
	  :ensure t
	  :config
	  (setq dashboard-center-content t)
	  (setq dashboard-vertically-center-content t)
	  (setq dashboard-items '((recents . 20)))
	  (setq dashboard-item-shortcuts '((recents . "r")))
	  (setq dashboard-display-icons-p t)
	  (setq dashboard-icon-type 'nerd-icons)
	  ;;(setq dashboard-set-heading-icons t)
	  (setq dashboard-set-file-icons t)
	  (dashboard-setup-startup-hook))


## File Tree


### Treemacs

	(use-package treemacs
	  :ensure t
	  :defer t
	  :bind
	  ("C-c t" . treemacs)
	  :config
	  (progn (setq treemacs-width 30))
	  (treemacs-resize-icons 16))


### Treemacs with Projectile

	(use-package treemacs-projectile
	  :ensure t
	  :after (treemacs projectile))


### Treemacs with Magit

	(use-package treemacs-magit
	  :ensure t
	  :after (treemacs magit))


### Treemacs with LSP

	(use-package lsp-treemacs
	  :ensure t
	  :commands
	  lsp-treemacs-errors-list)


## Modeline


### Doom Modeline

	(use-package doom-modeline
	  :ensure t
	  :config
	  (setq doom-modeline-icon t)
	  (setq doom-modeline-minor-modes t)
	  (setq doom-modeline-enable-word-count t)
	  (setq doom-modeline-total-line-number t)
	  (setq find-file-visit-truename t)		; To show 'symlinked' file path  correctly
	  :init
	  (doom-modeline-mode 1))


### Show Time on Modeline

	(use-package time
	  :init
	  (display-time-mode t))


## Git


### Magit

[Webpage](https://magit.vc)

	(use-package magit
	  :ensure t
	  :custom
	  (magit-display-buffer-function #'magit-display-buffer-same-window-except-diff-v1))


## Minibuffer Set - SMOCE

"Selectrum&#x2026; is replaced" with 'vertico'(see: <https://github.com/radian-software/selectrum>).


### Vertico

[Github](https://github.com/minad/vertico)

	(use-package vertico
	  :ensure t
	  :init
	  (vertico-mode))


### Marginalia

[Github](https://github.com/minad/marginalia)

	(use-package marginalia
	  :after vertico
	  :ensure t
	  :config
	  (marginalia-mode))


### Orderless

[Github](https://github.com/oantolin/orderless)

	(use-package orderless
	  :ensure t
	  :init
	  (setq completion-styles '(orderless basic)
			completion-category-defaults nil
			completion-category-overrides '((file (styles partial-completion)))))


### Embark

[Github](https://github.com/oantolin/embark)

	(use-package embark
	  :ensure t)


### Embark Consult

	(use-package embark-consult
	  :after embark
	  :ensure t)


### Consult

[Github](https://github.com/minad/consult)

	(use-package consult
	  :after  embark-consult
	  :ensure t)


### `OBSOLETE Helm`

Using `SMOCE` instead of `helm`

	(use-package helm
	  :ensure t
	  :diminish
	  :bind
	  ("M-x" . 'helm-M-x)
	  ("C-x C-f" . 'helm-find-files)
	  :config
	  (setq helm-display-header-line nil)
	  (set-face-attribute 'helm-source-header nil :height 0.1)
	  (setq helm-split-window-inside-p t)
	  (helm-mode 1))


### `OBSOLETE Helm-LSP`

	(use-package helm-lsp
	  :ensure t
	  :commands
	  helm-lsp-workspace-symbol)


## Completion at Point


### Company

	(use-package company
	  :ensure t
	  :diminish
	  :custom
	  (company-minimum-prefix-length 1)
	  (company-idle-delay 0.0)
	  :init
	  (global-company-mode)
	  :config
	  (setq company-backends (mapcar #'jeon/company-add-yas-backend company-backends)))	; add yasnippet to all backends


### `OBSOLETE Corfu`

[Github](https://github.com/minad/corfu)

Using `Company` instead of `Corfu`

Depedency

-   emacs
-   dabbrev
-   savehist

	(use-package corfu
	  :ensure t
	  :custom
	  (corfu-auto t)
	  (corfu-quit-no-match 'separator)
	  :bind
	  (:map corfu-map
			("RET" . nil))
	  :init
	  (global-corfu-mode))

	;; 'corfu' manual also recommends below
	(use-package emacs
	  :init
	  (setq tab-always-indent 'complete)
	  (setq text-mode-ispell-word-completion nil)
	  (setq read-extended-command-predicate #'command-completion-default-include-p))

	;;; Extensions:
	;; corfu-history
	(corfu-history-mode 1)
	(add-to-list 'savehist-additional-variables 'corfu-history)

	;; corfu-indexed
	(corfu-indexed-mode 1)

	;; corfu-popupinfo
	(corfu-popupinfo-mode 1)

	(use-package nerd-icons-corfu
	  :ensure t)
	(add-to-list 'corfu-margin-formatters #'nerd-icons-corfu-formatter)


## Org Mode

	(use-package org
	  :custom
	  (org-ellipsis " ▾"))
	  ;;:config
	  ;;(add-to-list 'org-structure-template-alist '("el" . "src emacs-lisp")))


### Org Preview

Github: [jakebox/org-preview-html](https://github.com/jakebox/org-preview-html)

	(use-package org-preview-html
	  :ensure t)

1.  ISSUE-240410#preview: unable to resolve link

	See: [1.18.3.1](#org25c8835)


### Org Bullets

	(use-package org-bullets
	  :ensure t
	  :custom
	  (org-bullets-bullet-list '("◉" "○" "●" "○" "●" "○" "●"))
	  :hook
	  (org-mode . org-bullets-mode))


### Org ToC

Github: [snosov1/toc-org](https://github.com/snosov1/toc-org)

	(use-package toc-org
	  :ensure t
	  :hook
	  (org-mode . toc-org-mode))

Make a heading for table of contents, and add tag `:TOC:` by `C-c~~C-q`.

1.  FIX: ISSUE-240410#preview

	****Add `noexport` tag with `:TOC:`, so it becomes `:TOC:noexport:`**** (see: [Github's Exclude headings part](https://github.com/snosov1/toc-org#exclude-headings), [ISSUE-BUG:35](https://github.com/snosov1/toc-org/issues/35))


### Org visual fill column

	(use-package visual-fill-column
	  :ensure t
	  :hook (org-mode . jeon/org-fill-column))


## Project Manager


### ag

	(use-package ag
	  :ensure t
	  :custom
	  (ag-highlight-search t))


### rg

	(use-package rg
	  :ensure t
	  :config
	  (rg-enable-default-bindings))


### Projectile

	(use-package projectile
	  :ensure t
	  :bind-keymap
	  ("C-c p" . projectile-command-map)
	  ;;:custom
	  ;;(projectile-project-search-path '("~/Dev" "~/Git"))
	  :config
	  (projectile-mode 1))


## Snippet


### Yasnippet

	(use-package yasnippet
	  :ensure t
	  :diminish (yas-minor-mode)
	  :config
	  (yas-reload-all)
	  :hook
	  (prog-mode . yas-minor-mode))


### Snippets

	(use-package yasnippet-snippets
	  :ensure t)


## LSP


### LSP-Mode

	(use-package lsp-mode
	  :ensure t
	  :bind
	  ("C-c l" . lsp-keymap-prefix)
	  :custom
	  (lsp-warn-no-matched-clients nil)		; prevent 'el' file warnings
	  (lsp-idle-delay 0.500)
	  (lsp-completion-provider :none)
	  :hook
	  (lsp-mode . lsp-enable-which-key-integration)
	  (prog-mode . lsp-deferred))


### LSP-UI

	(use-package lsp-ui
	  :ensure t
	  :bind
	  (:map lsp-ui-mode-map
			([xref-find-definitions] . lsp-ui-peek-find-definitions )
			([xref-find-references] . lsp-ui-peek-find-references))
	  :custom
	  (lsp-ui-sideline-show-hover 't)
	  (lsp-ui-sideline-show-code-actions 't)
	  (lsp-ui-doc-position 'at-point)
	  (lsp-ui-doc-show-with-cursor 't)
	  :commands
	  lsp-ui-mode)


### `OBSOLETE eglot`

Using `lsp-mode` instead of `eglot`

	(use-package eglot
	  :hook
	  (prog-mode . eglot-ensure))


## Debug


### Dap-mode

	(use-package dap-mode
	  :ensure t)


## Language Specifics


### Ada

[Ada Doc](https://ada-lang.io/docs/learn/getting-started/editors/#emacs)

	(use-package ada-mode
	  :ensure t)

	;;; ISSUE: 2024-04-06 <Need to check 'gpr-mode', 'gpr-query' - Are these necessary?>
	;; Needs 'gpr-mode' to resolve 'Ada parser exec 'ada_mode_wisi_lr1_parse' not found' issue.
	;; No, it wasn't fixed, yet.
	;; (See: https://forum.ada-lang.io/t/gnu-emacs-ada-mode-8-0-4-released/310 )
	;; > gpr-query and gpr-mode are split out into separate GNU ELPA packages.
	;; However, seems ok to just install 'gpr-mode'

	;; (use-package gpr-mode
	;;   :ensure t)

	;; (use-package gpr-query
	;;   :ensure t)


### C

	(use-package cc-vars
	  :custom
	  ;;(c-basic-offset 4)
	  (c-default-style "k&r"))


### Markdown

[Project Website](https://jblevins.org/projects/markdown-mode/)

	(use-package markdown-mode
	  :ensure t)
