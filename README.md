liquid-types.el
===============

Error reporting (via flycheck) and type display (via pos-tip) for

+ [liquidhaskell](https://github.com/ucsd-progsys/liquidhaskell)

__Now a minor mode!__

Requirements
------------

Make sure you have the following packages installed:

+ `flycheck`
+ `haskell-mode`
+ `pos-tip`
+ `popup`
+ `button-lock`
+ `flycheck-color-mode-line`

We recommend using [MELPA](http://melpa.org/#/getting-started) to install the dependencies.

Install
-------

Add the following to your load-path:

~~~~~
(add-to-list 'load-path "~/.emacs.d/liquid-types.el/")
~~~~~

*Step 1* Grab the various mode files:

~~~~~
cd ~/.emacs.d
git clone https://github.com/themattchan/liquid-types.el.git
~~~~~

*Step 2* Add the following to your `init.el` or equivalent:

~~~~~
;; ----------------------- Configure Flycheck ------------------

(require 'flycheck)

;; Global Flycheck
(global-flycheck-mode)

;; Rerun check on idle and save
(setq flycheck-check-syntax-automatically
      '(mode-enabled idle-change save))

;; ----------------------- Configure LiquidHaskell -------------

(require 'haskell-mode)
(require 'flycheck-liquid)
(require 'liquid-tip)

(add-hook 'haskell-mode-hook
	  '(lambda () (liquid-tip-mode)))
~~~~~

To toggle `liquid-tip-mode` manually, do `M-x liquid-tip-mode`.

Customization
-------------
You can customize liquid-tip-mode using `M-x customize`. Search for
`liquid-tip`, then select from the options.

Customizable variables are :

- `liquid-tip-style` The style for the popup tooltip. Available styles are
`'ascii` and `'balloon`
- `liquid-tip-checker-name` The name of the checker you run liquidhaskell with
--- specify which file prefixes to load. Either `'flycheck` or `nil`
- `liquid-tip-trigger` The available options are double click `'double-mouse-1`
  and shift-double click `'S-double-mouse-1`, or you can enter a symbol
  corresponding to a mouse action.

You can customize flycheck in various ways.

**Multiple Checkers** You can *chain* multiple checkers by:

~~~~~
(add-hook 'flycheck-mode-hook
      (lambda ()(require 'flycheck-liquid)
        (flycheck-add-next-checker 'haskell-ghc 'haskell-hlint)
        (flycheck-add-next-checker 'haskell-hlint 'haskell-liquid)))
~~~~~

**Error Highlighting** To highlight errors in red:

~~~~~
(require 'flycheck-color-mode-line)

(eval-after-load "flycheck"
  '(add-hook 'flycheck-mode-hook 'flycheck-color-mode-line-mode))

(set-face-attribute 'flycheck-error nil
                    :foreground "red"
	            :background "pink")
~~~~~

**Disable**  To disable flycheck-liquid on a *particular* file, add:

    -- Local Variables:
    -- flycheck-disabled-checkers: (haskell-liquid)
    -- End:

at the end of the file.
