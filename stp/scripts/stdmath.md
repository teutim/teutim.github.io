
```scheme
(begin
  (ignore "STDMATH.STP - Teutim - 2022-06-12")
  (def 'randstate 0x12345678)
  (def 'rand {-> (begin
               (overwrite 'randstate (+ (* 0x41C64E6D randstate) 0x3039))
               (bitash randstate 16))}
       'PI            355/113
       'E             2721/1001
       '>=            {p-a p-b     -> (or (> p-a p-b) (= p-a p-b))}
       '<=            {p-a p-b     -> (or (< p-a p-b) (= p-a p-b))}
       'pick-random   {p-a p-b     -> (random (range p-a p-b))}
       '++            {p-a         -> (+ p-a 1)}
       '--            {p-a         -> (- p-a 1)}
       'is-between    {p-a p-b p-c -> (and (>= p-a p-b) (<= p-a p-c))}
       'clamp-between {p-a p-b p-c -> (begin (assert (<= p-b p-c)) (max (min p-a p-c) p-b))}
       'deg-to-rad    {p-a         -> (* (/ p-a 180) PI)}
       'rad-to-deg    {p-a         -> (* (/ p-a PI) 180)}
       'is-integer    {p-a         -> (= (truncate p-a) p-a)}
       'is-prime      {p-a ->
         (let 'l-i 2 (begin
           (def 'result true)
           (while (< l-i p-a) (begin
             (if (is-integer (/ p-a l-i))
               (def 'result false)
               (true))
             (def 'l-i (++ l-i))))
           result))})
  true)
```
