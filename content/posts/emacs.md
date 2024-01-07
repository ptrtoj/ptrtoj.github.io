---
title:  "Emacs Config"
date:   2024-01-07
url: 'emacs'
---

My Super Minimal Emacs Configuration.
Files are availbale on [Github](https://github.com/ptrtoj/Emacs.git).
## Basics
### Fix Scope

```elisp
;; -*- coding: utf-8 lexical-binding: t; -*-
```

### Custom.el
Prevent `custom-set-variables` and `custom-set-faces` wrote inside `init.el`

```elisp
;; Remove Custom Section
(setq custom-file (concat user-emacs-directory "custom.el"))
(when (file-exists-p custom-file)
(load custom-file))
```

## Initialize Packages
### Setup

```elisp
(require 'package)
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/"))
(package-initialize)
(unless package-archive-contents
(package-refresh-contents))
```

## Emacs

Use [Use-package](https://github.com/jwiegley/use-package) for customizing Emacs and 3rd party packages.

```elisp
(use-package emacs
:custom
(user-full-name "WooHyung Jeon")
(user-mail-address "jeon@ptrtoj.com")
(visible-bell t)
(inhibit-startup-screen t)
(inhibit-startup-message t)
(inhibit-startup-echo-area-message t)
(initial-major-mode 'fundamental-mode)
(defalias 'yes-or-no-p 'y-or-n-p))
```

## UI
### Themes

```elisp
(use-package custom
:config
;;(load-theme 'modus-operandi t)
(load-theme 'leuven t))
```

### Fonts

```elisp
(use-package faces
:config
(set-face-attribute 'font-lock-comment-face nil :slant 'italic)
(set-face-attribute 'font-lock-keyword-face nil :slant 'italic)
(add-to-list 'default-frame-alist '(font . "Berkeley Mono")))
```

### Disable Unnecessaries

#### Tool Bar

```elisp
(use-package tool-bar
:config
(tool-bar-mode -1))
```

#### Tooltip

```elisp
(use-package tooltip
:config
(tooltip-mode -1))
```

#### Menu Bar

```elisp
(use-package menu-bar
:config
(menu-bar-mode -1))
```

### Whitespaces

```elisp
(use-package whitespace
:hook
(before-save . whitespace-cleanup))
```

### Simple

```elisp
(use-package simple
:config
(column-number-mode 1)
:hook
(before-save . delete-trailing-whitespace))
```

### Line Numbers

```elisp
(use-package display-line-numbers
:custom
(display-line-numbers-type 'relative)
:config
(global-display-line-numbers-mode 1)
(dolist (mode '(term-mode-hook
shell-mode-hook
eshell-mode-hook))
(add-hook mode (lambda () (display-line-numbers-mode -1)))))
```

### Fill Column

```elisp
(use-package display-fill-column-indicator
:config
(global-display-fill-column-indicator-mode 1))
```

### Highlight Line

```elisp
(use-package hl-line
:config
(global-hl-line-mode 1))
```

### Frame

Currently, not using.

```elisp
(use-package frame
:config
;; Transparency
(set-frame-parameter (selected-frame) 'alpha '(90 . 85))
(add-to-list 'default-frame-alist '(alpha . ( 90 . 85)))
;; Fullscreen when open Emacs
(add-to-list 'default-frame-alist '(fullscreen . maximized)))
```

## File Related

### Files

```elisp
(use-package files
:custom
(make-backup-files nil))
```

### Recent Files

```elisp
(use-package recentf
:config
(recentf-mode 1)
:bind
("C-x C-r" . recentf-open-files))
```

### Auto Revert

```elisp
(use-package autorevert
:custom
(global-auto-revert-non-file-buffers t))
```

### Save history

```elisp
(use-package savehist
:config
(savehist-mode 1))
```

## Custom Key Bindings

which doesn't belong anywhere.

```elisp
(use-package bind-keys
:bind
;; Changed the same keybinding to 'auto-package-update-now'
;; See, 'auto-package-update' section below
;;("C-c p" . package-refresh-contents)
("C-c k" . describe-personal-keybindings))
```

## Programming Related

### LSP

```elisp
(use-package eglot
:hook
(prog-mode . eglot-ensure))
```

### Parenthesis Paring

```elisp
(use-package elec-pair
:hook
(prog-mode . electric-pair-mode)
(org-mode . electric-pair-mode))
```

### C Language Specific

#### in Emacs

```elisp
(use-package cc-vars
:custom
(c-default-style "k&r")
(c-basic-offset 4))
```

## 3rd Party Packages

### Use-package related

#### Auto Update

Automatically update Emacs packages with [auto-package-update](https://github.com/rranelli/auto-package-update.el).

```elisp
(use-package auto-package-update
:ensure t
:config
(setq auto-package-update-prompt-before-update t)
(setq auto-package-update-delete-old-versions t)
:bind
("C-c p" . auto-package-update-now))
```

#### Diminish

Hide minor modes with [diminish](https://github.com/myrjola/diminish.el).

```elisp
(use-package diminish
:ensure t)
```

### Which Key

Display available keybindings with [emacs-which-key](https://github.com/justbur/emacs-which-key).

```elisp
(use-package which-key
:ensure t
:diminish
:config
(which-key-mode 1))
```

### Orgmode Table of Contents

Insert Table of Contents inside org documents with [toc-org](https://github.com/snosov1/toc-org).

```elisp
(use-package toc-org
:ensure t
:diminish
:hook
(org-mode . toc-org-mode))
```

### Fix path

Fix path problems occur in MacOS by [exec-path-from-shell](https://github.com/purcell/exec-path-from-shell).

```elisp
(use-package exec-path-from-shell
:ensure t
:defer t
:config
(when (memq window-system '(mac ns x))
(exec-path-from-shell-initialize)))
```