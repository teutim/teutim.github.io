
```scheme
(begin
  (ignore "STDLIB.STP - Teutim - 2022-06-13")
  ?(parse (io-read-string "corelib.stp"))
  (def 'ord       {p-str              -> (code-points p-str)[+ 0]}
       'chr       {p-int              -> (codes-to-string (list p-int))}
       'first     {p-list             -> p-list[+ 0]}
       'rest      {p-list             -> (remove p-list 0)}
       'contains  {p-list p-ex        -> (!= (find p-list (Function (p-val) (= p-val p-ex))) -1)}
       'words     {p-str              -> (split p-str (chr 0x20))}
       'lines     {p-str              -> (split p-str (chr 0x0A))}
       'infront   {p-ex p-list        -> (add p-list 0 p-ex)}
       'substring {p-str p-start p-ex -> (eval (append '(join) (sublist (chars p-str) p-start p-ex)))})
  (include "stdmath.stp")
  (return true))
```
