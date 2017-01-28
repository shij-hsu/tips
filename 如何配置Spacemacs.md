## 如何配置*Spacemacs*(ProofGeneral+company-coq)

1. 安装spacemacs, 去官网git clone
2. 安装source code pro字体，官网给出了链接
3. 安装FDK工具包
4. 在source code pro 目录下执行./build.sh，target目录下会生成otf和ttf字体
5. copy到/usr/local/share/fonts目录，sudo fc-cache
6. 执行emacs --insecure，至此常规安装结束，接下来修复bug，注意spacemacs不使用.emacs文件，最好移除该文件
7. emacs的启动太慢，这个锅emacs不背，是helm包惹的祸，将如下代码添加进~/.spacemacs，详见注释中的issuse链接

```lisp
;; https://github.com/syl20bnr/spacemacs/issues/2705
;; Fix up the startup-time
(setq tramp-ssh-controlmaster-options "-o ControlMaster=auto -o ControlPath='tramp.%%C' -o ControlPersist=no")
```

8. 针对melpa源网速差的情况，可以使用这个


```lisp
(require 'package)  
(add-to-list 'package-archives '("melpa" . "https://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/") t)
(package-initialize) 
```
9. ProofGeneral 照常安装，添加

```lisp
;; Open .v files with Proof General's Coq mode
(load "~/.emacs.d/lisp/PG/generic/proof-site")
```

10.  安装company-coq自动补全后需要注意，spacemacs会自动删除orphan packages所以就悲剧了，下次启动时你会发现刚装了包又被删除了，需要添加如下代码进入.spacemacs

```lisp
(defun dotspacemacs/layers ()
    (setq-default
   dotspacemacs-additional-packages '(company-coq)
   ))
```

并在ProofGeneral代码后添加

```lisp
;; Load company-coq when opening Coq files
(add-hook 'coq-mode-hook #'company-coq-mode)
(add-hook 'coq-mode-hook
          (lambda ()
            ( setq-local prettify-symbols-alist
                         '(("Alpha" . ?Α) ("Beta" . ?Β) ("Gamma" . ?Γ)
                           ("Delta" . ?Δ) ("Epsilon" . ?Ε) ("Zeta" . ?Ζ)
                           ("Eta" . ?Η) ("Theta" . ?Θ) ("Iota" . ?Ι)
                           ("Kappa" . ?Κ) ("Lambda" . ?Λ) ("Mu" . ?Μ)
                           ("Nu" . ?Ν) ("Xi" . ?Ξ) ("Omicron" . ?Ο)
                           ("Pi" . ?Π) ("Rho" . ?Ρ) ("Sigma" . ?Σ)
                           ("Tau" . ?Τ) ("Upsilon" . ?Υ) ("Phi" . ?Φ)
                           ("Chi" . ?Χ) ("Psi" . ?Ψ) ("Omega" . ?Ω)
                           ("alpha" . ?α) ("beta" . ?β) ("gamma" . ?γ)
                           ("delta" . ?δ) ("epsilon" . ?ε) ("zeta" . ?ζ)
                           ("eta" . ?η) ("theta" . ?θ) ("iota" . ?ι)
                           ("kappa" . ?κ) ("lambda" . ?λ) ("mu" . ?μ)
                           ("nu" . ?ν) ("xi" . ?ξ) ("omicron" . ?ο)
                           ("pi" . ?π) ("rho" . ?ρ) ("sigma" . ?σ)
                           ("tau" . ?τ) ("upsilon" . ?υ) ("phi" . ?φ)
                           ("chi" . ?χ) ("psi" . ?ψ) ("omega" . ?ω)
                           ("\\\\" . ?⇓)("|->" . ?↦)(":=" . ?≜)("\\in" .?∈)
                           ("~" .?¬)("Proof." . ?∵) ("Qed." . ?■)
                           ("Defined." . ?□) ("Time" . ?⏱) ("Admitted." . ?😱)))))
```

以上是一些字符的显示

11. 最后可以添加如下代码在.spacemacs文件头部

```lisp
;;Show lineno
(global-linum-mode t)
;;Hide tool bar
(tool-bar-mode -1)
;;Set cursor mode 'bar for 束线 'box for 方块
(setq-default cursor-type 'bar)
```

