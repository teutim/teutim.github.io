
```scheme
(begin
  (ignore "CORELIB.STP - Teutim - 2022-06-12")
  (def 'last         {list -> list[- (length list) 1]}
       'require-file {file -> ?(parse (io-read-string file))}
       'require      {     -> (last (foreach @args require-file))}
       'include-file {file -> (if (io-exists file) ?(parse (io-read-string file)) ())}
       'include      {     -> (last (foreach @args include-file))})
  true)
```
