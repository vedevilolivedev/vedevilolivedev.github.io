+++
title = "Doom Config"
author = ["Ved Evilolive"]
description = "Just my lit config"
date = 2022-02-11T23:50:00-07:00
draft = false
toc = true
+++

## Headers {#headers}

```emacs-lisp
;;; config.el -*- lexical-binding: t; -*-
```

```emacs-lisp
;;; packages.el -*- lexical-binding: t; no-byte-compile: t; -*-
```


## User info {#user-info}

```emacs-lisp
(setq user-full-name "Ved Evilolive"
      user-mail-address "ved@evilolive.dev")
```


## GPG/Security setup {#gpg-security-setup}

Set the settings for authorization/passwords.

```emacs-lisp
(setq auth-sources '("~/.authinfo.gpg"))
```


## System-specific {#system-specific}


### Definitions {#definitions}


#### Work/DENM {#work-denm}

```emacs-lisp
(defconst DENM (string-equal (system-name) "bburks-mbp"))
```


#### Crius {#crius}

```emacs-lisp
(defconst CRIUS (string-equal (system-name) "crius.local"))
```


## UI {#ui}


### Scrolling {#scrolling}

Perhaps reduce flickering/help with speed on retina. See <https://discourse.doomemacs.org/t/why-is-emacs-doom-slow/83/3>

```emacs-lisp
;; (add-to-list 'default-frame-alist '(inhibit-double-buffering . t))
```


### Dashboard {#dashboard}


#### Fancy splash image {#fancy-splash-image}

```emacs-lisp
(setq fancy-splash-image (concat doom-private-dir "/splash/doomslant.png"))
```


#### Utility Functions {#utility-functions}

These functions are also a convenience one so don't have to repeat them.

```emacs-lisp
(defun +ved/loadlit ()
  "Load the lit.org from ~/.doom.d or whatever the private dir is."
  (interactive)
  (find-file (expand-file-name "lit.org" doom-private-dir)))
(defun +ved/ffchez ()
  (interactive)
  "Explore the chezmoi dotfiles directory since this is frequented."
  (doom-project-find-file "~/chezmoi/dotfiles/"))
(defun +ved/loadzshlit ()
  (interactive)
  "Open the ZSH literate config file."
  (find-file (expand-file-name "~/ZSH-lit.org")))
```


#### Change the mapping {#change-the-mapping}

```emacs-lisp
(map! :map +doom-dashboard-mode-map
      :ne "f" #'find-file
      :ne "r" #'consult-recent-file
      :ne "p" #'doom/open-private-config
      :ne "l" #'+ved/loadlit
      :ne "c" #'+ved/ffchez
      :ne "z" #'+ved/loadzshlit
      :ne "B" #'consult-buffer
      :ne "q" #'save-buffers-kill-terminal)
```


#### Menu Change {#menu-change}

```emacs-lisp
(setq +doom-dashboard-menu-sections
      '(("Open File"
         :icon (all-the-icons-octicon "file-text" :face 'doom-dashboard-menu-title)
         :action find-file)
        ("Recently Opened Files"
         :icon (all-the-icons-octicon "history" :face 'doom-dashboard-menu-title)
         :action recentf-open-files)
        ("Open EMACS Config Dir"
         :icon (all-the-icons-faicon "folder-open-o" :face 'doom-dashboard-menu-title)
         :action doom/open-private-config)
        ("Open EMACS Literate ORG File"
         :icon (all-the-icons-fileicon "emacs" :face 'doom-dashboard-menu-title)
         :action +ved/loadlit)
        ("Open ZSH Literate File"
         :icon (all-the-icons-fileicon "org" :face 'doom-dashboard-menu-title)
         :action +ved/loadzshlit)
        ("Open Chezmoi Dotfiles Dir"
         :icon (all-the-icons-octicon "home" :face 'doom-dashboard-menu-title)
         :action +ved/ffchez)))
```


#### Better Buffer Name {#better-buffer-name}

```emacs-lisp
(setq +doom-dashboard-name " Doom")
```


### Font {#font}

Still trying to figure out which I like best.

```emacs-lisp
(setq doom-font (font-spec :family "FiraMono Nerd Font" :size 13)
      doom-big-font (font-spec :family "FiraMono Nerd Font" :size 18)
      doom-variable-pitch-font (font-spec :family "FiraCode Nerd Font" :size 13)
      doom-serif-font (font-spec :family "CodeNewRoman Nerd Font" :weight 'regular)
      doom-unicode-font doom-font)
```


### Theme {#theme}

Unpin doom-themes

```emacs-lisp
(unpin! doom-themes)
```

Got this for in case I start using one config for two computers. Maybe remove this.

```emacs-lisp
;;(when CRIUS (setq doom-theme 'doom-outrun-electric))
(setq doom-theme 'doom-tokyo-night)
(setq doom-tokyo-night-padded-modeline 4)
;; (setq! doom-outrun-electric-brighter-comments t)
;; (when DENM (setq doom-theme 'doom-dracula))
;; (when (eq doom-theme 'doom-dracula)
;;   (setq! doom-dracula-colorful-headers t))
(setq! doom-themes-padded-modeline t)
```


### Modeline Settings {#modeline-settings}

So it doesnt bump up all the way against the frame. Still might need some padding.

```emacs-lisp
(setq!
        doom-modeline-buffer-file-name-style 'truncate-upto-root
        doom-modeline-major-mode-icon t
        doom-modeline-major-mode-color-icon t
        doom-modeline-buffer-encoding nil
        doom-modeline-workspace-name nil
        doom-modeline-persp-name nil
        doom-modeline-indent-info t
        all-the-icons-scale-factor 1.1
)
```


#### Nyan Mode {#nyan-mode}

Always must have.

```emacs-lisp
(package! nyan-mode)
```

```emacs-lisp
(use-package! nyan-mode
  :after doom-modeline
  :config
        (nyan-mode)
  :custom
        (nyan-animate-nyancat t)
        (nyan-wavy-trail t)
        (nyan-bar-length 27))
```


#### Set indicator colors in modeline {#set-indicator-colors-in-modeline}

I like these to also match the cursor, so below is about the same colors (some adjustments for visibility).

```emacs-lisp
(custom-set-faces!
  '(doom-modeline-evil-insert-state :foreground "#00eeee")
  '(doom-modeline-evil-normal-state :foreground "#Cd5555")
  '(doom-modeline-evil-visual-state :foreground "#6c7b8b")
  '(match :foreground "#000000" :background "#ff02ab")
  )
```

<!--list-separator-->

-  Matching cursor

    Except visual cause otherwise it's hard to see sometimes

    ```emacs-lisp
    (setq! evil-normal-state-cursor '(box "#Ee0000")
           evil-insert-state-cursor '(box "#00eeee")
           evil-visual-state-cursor '((hbar . 3) "#Ee1289")
           )
    ```


### Linenumber colors {#linenumber-colors}

Defaults for electric were too dark, so changing it here.

```emacs-lisp
(after! display-line-numbers
    (custom-set-faces!
     '(line-number :foreground "#6C7B8B")
     '(line-number-current-line :foreground "#FFFFFF" :weight bold)))
```


### Beacon {#beacon}

Highlights the point when scrolling and changing windows and such

```emacs-lisp
(package! beacon)
```

```emacs-lisp
(use-package! beacon
  :config
  (beacon-mode t)
  (setq! beacon-color "#ee1289"
         beacon-blink-delay 0.3
         beacon-blink-duration 0.5
         beacon-blink-when-focused t)
  )
```


### Rainbow Mode {#rainbow-mode}

Show background of colors when colors are set.

```emacs-lisp
(use-package! rainbow-mode
  :defer t
  :hook ((text-mode prog-mode) . rainbow-mode))
(add-hook! 'rainbow-mode-hook (hl-line-mode (if rainbow-mode -1 +1)))
```


#### Fix color menu {#fix-color-menu}

MacOS misses the thing that does colors like DeepPink2 in the menu. This fixes.

```emacs-lisp
(require 'facemenu)
```


### Show Parens {#show-parens}

Sets the colors for show parens

```emacs-lisp
(custom-set-faces!
  '(show-paren-match :foreground "#2f4f4f" :background "#00ee00")
  '(show-paren-mismatch :background "#ee0000" :foreground "#fffc00")
  '(show-paren-match-expression :background "#00868b" :foreground "#ffffff")
  '(match :foreground "#000000" :background "#Ff82ab"))
```


## Org {#org}

First need to set the org directory

```emacs-lisp
(setq org-directory "~/org/")
```


### Allow encryption in org files {#allow-encryption-in-org-files}

```emacs-lisp
  (use-package! org-crypt
    :after org
    :config
    (setq! org-tags-exclude-from-inheritance '("crypt" "read_only")
           org-crypt-key "ved@evilolive.dev"
           org-crypt-disable-auto-save "ask")
    :init
    (org-crypt-use-before-save-magic)
    )
```


### Read-only sections {#read-only-sections}

```emacs-lisp
(defun unpackaged/org-next-heading-tagged (tag)
  "Move to beginning of next heading tagged with TAG and return point, or return nil if none found."
  (when (re-search-forward (rx-to-string `(seq bol (1+ "*") (1+ blank) (optional (1+ not-newline) (1+ blank))
                                               ;; Beginning of tags
                                               ":"
                                               ;; Possible other tags
                                               (0+ (seq (1+ (not (any ":" blank))) ":") )
                                               ;; The tag that matters
                                               ,tag ":"))
                           nil 'noerror)
    (goto-char (match-beginning 0))))

(defun ap/org-remove-read-only ()
  "Remove read-only text properties from Org entries tagged \"read_only\" in current buffer."
  (let ((inhibit-read-only t))
    (org-with-wide-buffer
     (goto-char (point-min))
     (while (unpackaged/org-next-heading-tagged "read_only")
       (remove-text-properties (point) (progn
                                         (org-end-of-subtree)
                                         (point))
                               '(read_only t))))))
(add-hook! 'org-mode-hook #'ap/org-mark-read-only)
```


### Ligatures {#ligatures}

Some customizatons and additions


#### Org {#org}

```emacs-lisp
  (appendq! +ligatures-extra-symbols
             '(:title         "T"
                  :subtitle      ""
                  :author        "Ⓐ"
                  :property      "∹"
                  :name          "⒩"
                  :emptycheck    ""
                  :checkedbox    ""
                  :partialbox    ""
                  :email         "﫯"
                  ))
  (set-ligatures! 'org-mode
    :merge t
    :title "#+TITLE:"
    :title "#+title:"
    :subtitle "#+SUBTITLE:"
    :subtitle "#+subtitle:"
    :author "#+AUTHOR:"
    :author "#+author:"
    :property "#+PROPERTY:"
    :property "#+property:"
    :name "#+NAME:"
    :name "#+name:"
    :emptycheck "[ ]"
    :checkedbox "[X]"
    :partialbox "[-]"
    :email "#+email:"
    :email "#+EMAIL:"
    )
```


#### General {#general}

Fix some that don't show up on Mac for some fucking reason.

```emacs-lisp
(plist-put! +ligatures-extra-symbols
            :true "⊨"
            :false "⊭"
            :str ""
            :bool "ℬ"
)
```


### Templates {#templates}

I've got my own templates I like to use.

```emacs-lisp
(set-file-template! "/\\(?:Sources/.*\\|README\\)\\.org$"
  :trigger "__head"
  :mode 'org-mode)
```


### Org Appear {#org-appear}

This makes org things show when the point is near/in it.

```emacs-lisp
(package! org-appear)
```

```emacs-lisp
(use-package! org-appear
  :hook (org-mode . org-appear-mode)
  :config
  (setq! org-appear-autoemphasis t
         org-appear-autosubmarkers t
         )
  )
```


### General Org Settings {#general-org-settings}

Hide the markers, inherit properties (I think that's needed for the read-only/encryption one), add time to agenda done (not that I use it).
I forget what invisible-edits does.

```emacs-lisp
(after! org
    (setq
        org-use-property-inheritance t
        org-log-done 'time
        org-catch-invisible-edits 'smart
        ;; org-export-with-sub-superscripts '{}
        org-ellipsis ""
        org-support-shift-select t
        org-hide-emphasis-markers t
        org-id-link-to-org-use-id 'create-if-interactive-and-no-custom-id
        )
(add-hook! org-mode (electric-indent-local-mode -1)))
(global-prettify-symbols-mode 1)
```


### Org reformat buffer {#org-reformat-buffer}

<https://github.com/zzamboni/dot-doom/blob/master/doom.org#visual-session-and-window-settings>

```emacs-lisp
(after! org (defun zz/org-reformat-buffer ()
  (interactive)
  (when (y-or-n-p "Really format current buffer? ")
    (let ((document (org-element-interpret-data (org-element-parse-buffer))))
      (erase-buffer)
      (insert document)
      (goto-char (point-min))))))
```


### Disable Company in Org {#disable-company-in-org}

It's annoying

```emacs-lisp
  (after! company (defun ved/org-mode-hook()
    (when (featurep! :completion company)
      (message "Disabling company-mode while in org-capture...")
      (company-mode -1))))
  (after! org (add-hook! org-capture-mode (ved/org-mode-hook)))
```


### Auto-Tangle {#auto-tangle}

Save some effort. Requires you put a property, unless you set `org-auto-tangle-default` to true. Otherwise use `#+auto_tangle: t` at the top.

```emacs-lisp
(package! org-auto-tangle)
```

```emacs-lisp
(use-package! org-auto-tangle
:defer t
:hook (org-mode . org-auto-tangle-mode))
```


### ORG UI {#org-ui}

Setting up org stuff to get it to look better I guess.

```emacs-lisp
(after! org
  (add-hook! 'org-mode-hook #'mixed-pitch-mode)
  (add-hook! 'org-mode-hook #'solaire-mode)
  (setq mixed-pitch-variable-pitch-cursor nil))
(after! org-superstar
  (setq org-superstar-prettify-item-bullets t)
)
```


### More prettify {#more-prettify}

```emacs-lisp
(after! org (custom-set-faces!
  '(outline-1 :weight extra-bold :height 1.25)
  '(outline-2 :weight bold :height 1.15)
  '(outline-3 :weight bold :height 1.12)
  '(outline-4 :weight semi-bold :height 1.09)
  '(outline-5 :weight semi-bold :height 1.06)
  '(outline-6 :weight semi-bold :height 1.03)
  '(outline-8 :weight semi-bold)
  '(outline-9 :weight semi-bold)
  '(org-document-title :height 1.2)))
```


### Keybinds Evil-org overrides {#keybinds-evil-org-overrides}

Just some annoying rebinds that I don't want.

```emacs-lisp
(map! :map evil-org-mode-map
  :ei "C-d" #'org-delete-char
  :ie "C-h" #'embark-help-command
)
```


## General Settings {#general-settings}

Set line numbers and some various things I like to have.

```emacs-lisp
(setq!
 display-line-numbers-type t
        history-delete-duplicates t
       elint-ignored-warnings '(unbound-reference unbound-assignment)
       ediff-split-window-function 'split-window-horizontally
       show-paren-style 'mixed
       show-paren-when-point-in-periphery t
       show-paren-when-point-inside-paren t
       show-paren-delay 0
       kill-whole-line t
       evil-move-cursor-back nil
       evil-kill-on-visual-paste nil
       evil-visual-region-expanded t
       evil-want-fine-undo t
       auto-save-default t
       scroll-margin 2
       scroll-preserve-screen-position nil
       scroll-conservatively 1337
       indent-tabs-mode nil
       tab-width 4
)
(blink-cursor-mode t)
(setq display-time-24hr-format t)
(display-time-mode t)
```


## Keybinds {#keybinds}


### Mac Keybindings {#mac-keybindings}

-   Mac specific keybinds (set `fn` key to `hyper`)
-   Also makes `Super Left-Click` to `Middle Click`
-   `Super` is `Command` on Mac.

<!--listend-->

```emacs-lisp
(when IS-MAC
  (setq! ns-function-modifier 'hyper)
  (define-key key-translation-map (kbd "<s-mouse-1>") (kbd "<mouse-2>")))
```


### General Keybindings {#general-keybindings}

I hate some of the remaps, so this puts it to how I like it/used to it from generic emacs.

```emacs-lisp
(map!
 :gni "C-k" #'kill-line
 :i "C-y" #'yank-from-kill-ring
 :gniv "C-e" #'doom/forward-to-last-non-comment-or-eol
 :gniv "C-a" #'doom/backward-to-bol-or-indent
 :n "U" #'undo-tree-visualize
 :i "C-U" #'undo-tree-visualize
 :gni "C-d" #'delete-char
 :i "C-SPC" #'set-mark-command
 :i "C-v" #'scroll-up-command
 :i "C-x C-f" #'find-file
 :i "C-w" #'kill-region
 :i "C-x C-s" #'save-buffer)
```

`delete-char` doesn't work in org-mode for some reason... (as gei)


### Editing shortcuts {#editing-shortcuts}

```emacs-lisp
(map!
    :gn "H-l" #'+ved/loadlit
    :gn "H-r" #'+ved/ffchez
    :gn "H-z" #'+ved/loadzshlit
    :gn "H-m" (cmd! (find-file (expand-file-name "~/chezmoi/dotfiles/.chezmoi.toml.tmpl")))
    :gn "H-n" (cmd! (doom-project-find-file "~/chezmoi/dotfiles/dot_config/nvim/"))
)
```


### Whichkey {#whichkey}


#### General Settings {#general-settings}

-   First faster pop-up

<!--listend-->

```emacs-lisp
(after! which-key
  (setq which-key-idle-delay 0.5))
```


#### Customizations {#customizations}

-   Remove redundant prefixes (stolen from [tecosaur](https://tecosaur.github.io/emacs-config/config.html#company))

<!--listend-->

```emacs-lisp
(setq which-key-allow-multiple-replacements t)
(after! which-key
  (pushnew!
   which-key-replacement-alist
   '(("" . "\\`+?evil[-:]?\\(?:a-\\)?\\(.*\\)") . (nil . "◂\\1"))
   '(("\\`g s" . "\\`evilem--?motion-\\(.*\\)") . (nil . "◃\\1"))
))
```


## File Modes {#file-modes}


### VIM {#vim}

```emacs-lisp
(package! vimrc-mode)
```

```emacs-lisp
(use-package! vimrc-mode
  :defer t
  :commands vimrc-mode)
```


### Salt {#salt}

```emacs-lisp
(package! salt-mode)
```

```emacs-lisp
(use-package! salt-mode
  :defer t
  :commands salt-mode)
```


### Gitlab {#gitlab}

For `.gitlabci` file

```emacs-lisp
(package! gitlab-ci-mode)
(package! gitlab-ci-mode-flycheck)
```

```emacs-lisp
(use-package! gitlab-ci-mode
  :defer t
  :commands gitlab-ci-mode)
(use-package! gitlab-ci-mode-flycheck
  :defer t
  :commands gitlab-ci-mode-flycheck)
```


### Puppet {#puppet}

```emacs-lisp
(package! puppet-mode)
```

```emacs-lisp
(use-package! puppet-mode
  :defer t
  :commands puppet-mode)
```


### Jenkinsfile {#jenkinsfile}

```emacs-lisp
(package! groovy-mode)
(package! jenkinsfile-mode)
```

```emacs-lisp
(use-package! jenkinsfile-mode
:commands jenkinsfile-mode)
(use-package! groovy-mode
:commands groovy-mode)
```


## Smart Parens {#smart-parens}

First thing is stop duplicates for quotes in vim, lisp, and org modes.

```emacs-lisp
(after! smartparens (sp-local-pair '(lisp-mode org-mode) "`" "`" :actions nil))
(after! smartparens (sp-local-pair '(lisp-mode org-mode) "'" "'" :actions nil))
(after! smartparens (sp-local-pair '(vimrc-mode) "\"" "\"" :actions nil))
```


## Bracket newline {#bracket-newline}

Lets you put a newline and move the point when between brackets and such. Very helpful with things like HCL/Terraform.

```emacs-lisp
(defun +ved/new-line-dwim ()
  (interactive)
  (let ((break-open-pair (or (and (looking-back "{") (looking-at "}"))
                             (and (looking-back ">") (looking-at "<"))
                             (and (looking-back "(") (looking-at ")"))
                             (and (looking-back "\\[") (looking-at "\\]")))))
    (newline)
    (when break-open-pair
      (save-excursion
        (newline)
        (indent-for-tab-command)))
    (indent-for-tab-command)))
(map!
  :i "M-RET" #'+ved/new-line-dwim)
```


## Company {#company}

Customizations for company

```emacs-lisp
(package! company-ispell :disable t)
```

```emacs-lisp
(after! company
  (set-company-backend! 'text-mode '(:separate company-dabbrev company-yasnippet)))
```


### Company Box {#company-box}

Make the box appear less quickly

```emacs-lisp
(after! company-box
  (setq! company-box-doc-delay 2))
```


## Frame dimensions {#frame-dimensions}

See autoload.

```emacs-lisp
(add-hook 'kill-emacs-hook #'my-save-frame-dimensions-h)
(my-restore-frame-dimensions-h)
```

```emacs-lisp
;;; autoload.el -*- lexical-binding: t; -*-
;;;###autoload
(defmacro unpackaged/def-org-maybe-surround (&rest keys)
  "Define and bind interactive commands for each of KEYS that surround the region or insert text.
Commands are bound in `org-mode-map' to each of KEYS.  If the
region is active, commands surround it with the key character,
otherwise call `org-self-insert-command'."
  `(progn
     ,@(cl-loop for key in keys
                for name = (intern (concat "unpackaged/org-maybe-surround-" key))
                for docstring = (format "If region is active, surround it with \"%s\", otherwise call `org-self-insert-command'." key)
                collect `(defun ,name ()
                           ,docstring
                           (interactive)
                           (if (region-active-p)
                               (let ((beg (region-beginning))
                                     (end (region-end)))
                                 (save-excursion
                                   (goto-char end)
                                   (insert ,key)
                                   (goto-char beg)
                                   (insert ,key)))
                             (call-interactively #'org-self-insert-command)))
                collect `(define-key org-mode-map (kbd ,key) #',name))))
;;;###autoload
(defun ap/org-mark-read-only ()
  "Mark all entries in the buffer tagged \"read_only\" with read-only text properties."
  (interactive)
  (org-with-wide-buffer
   (goto-char (point-min))
   (while (unpackaged/org-next-heading-tagged "read_only")
     (add-text-properties (point) (progn
                                    (org-end-of-subtree)
                                    (point))
                          '(read_only t)))))
;;;###autoload
(defun my-save-frame-dimensions-h ()
  "Caches the current frame dimensions and position so we can
  restore it when we launch emacs again."
  (if-let ((main-frame (car-safe (visible-frame-list))))
      (doom-store-put 'last-frame-size
                      (list (frame-position main-frame)
                            (frame-width main-frame)
                            (frame-height main-frame)
                            (frame-parameter main-frame 'fullscreen)))))
;;;###autoload
(defun my-restore-frame-dimensions-h ()
  (if-let (dims (doom-store-get 'last-frame-size "default"))
      (cl-destructuring-bind ((left . top) width height fullscreen) dims
        (setq initial-frame-alist
              (append initial-frame-alist
                      `((left . ,left)
                        (top . ,top)
                        (width . ,width)
                        (height . ,height)
                        (fullscreen . ,fullscreen)))))
    (add-to-list 'default-frame-alist '(height . 52))
    (add-to-list 'default-frame-alist '(width . 105))))
```


## My Lisp {#my-lisp}


### Timestamping {#timestamping}

Creates a timestamp line and modifies it as appropriate.

```emacs-lisp
(add-load-path! "mylisps/")
(require 'ved-timestamping)
```


## Projectile {#projectile}

Should be disabled, but just in case make it not update.

```emacs-lisp
(after! projectile
  (setq projectile-auto-update-cache nil))
```


## Dash {#dash}

This is a document lookup tool for MacOS.


### Install package if MacOS {#install-package-if-macos}

```emacs-lisp
(when IS-MAC
(package! dash-at-point))
```


### Configure if mac {#configure-if-mac}

-   TODO Figure out what hyper is on linux

<!--listend-->

```emacs-lisp
(when IS-MAC
  (use-package! dash-at-point
    :commands (dash-at-point dash-at-point-with-docset))
  (add-to-list 'dash-at-point-mode-alist '(elisp-mode . "elisp"))
  (add-to-list 'dash-at-point-mode-alist '(nix-mode . "nix"))
  (add-to-list 'dash-at-point-mode-alist '(python-mode . "py3"))
  (map!
   :gni "H-d" 'dash-at-point
   :gni "H-p" 'dash-at-point-with-docset)
  )
```


## Unicode Font Stuff {#unicode-font-stuff}

Not sure if this is helping, still have missing ligatures.

```emacs-lisp
(unicode-fonts-setup)
```


## Rainbow Delimeters Config {#rainbow-delimeters-config}

Lua doesn't have it for some reason, so we set it up.

```emacs-lisp
(add-hook! 'lua-mode-hook (rainbow-delimiters-mode))
```


## Editorconfig {#editorconfig}


### Custom major modes {#custom-major-modes}

Mostly for work but sometimes things need it (like `.tmpl` for `chezmoi`)

```emacs-lisp
(package! editorconfig-custom-majormode)
```

```emacs-lisp
(after! editorconfig
  (use-package! editorconfig-custom-majormode))
```
