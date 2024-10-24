# Lisp-like interpreter
## Overview
Lisp-like interpreter in Haskell supporting types, evaluations, and recursive let declarations within a static typing and dynamic scoping environment.

## Implemented lisp functionalities
Lisp most basic functionalities are implemented. Also added syntaxical sugar.

    
  ### Functions and operations
    (begin e_1 ... e_n) -                 Instruction sequences
    (λ (x_1 ... x_i) e_1 ... e_n) -       Lambda functions with arguments
    (e_0 ... e_n) -                       Curried functions
    Arithmetic operations                       
    Boolean operations                             
    (if e1 e2 e3) -                       If/Then/Else statement
    (let x e_x e_1 ... e_n) -             Local declaration
    (letrec (d_1 ... d_i) e_1 ... e_n) -  Mutually recursives declarations
    

  ### Memory management
    (: e τ) -      Type annotation
    (ref! e) -     Create memory cell with value e
    (get! e) -     Get value of memory cell e
    (set! e1 e2) - Set value e2 in memory cell e1

## How to use
Easiest way to use it is to load the file in a ghci session and call the `valOf` function on the string of the lisp expression you want to evaluate. 


## Examples

  ```
  (: 2 Int)                                       ; ↝ 2
  +                                       ; ↝ <function>
  (+ 2 4)                                 ; ↝ 6
  
  ((: (λ x x) (Int -> Int) ) 2)                             ; ↝ 2
  
  (((: (λ x y (* x y))) (Int Int -> Int)) 3 5)                                   ; ↝ 15
  
  (let true false
    (: (if true 1 2) Int))              ; ↝ 2
  
  (ref! 5)                                ; ↝ ptr<0>
  
  (let c1 (ref! 5)
   (let c2 c1
    (: (begin (set! c2 6)
     (+ (get! c1) (get! c2))) Int )))           ; ↝ 12
  
  (let c1 (ref! 1)
   (let c2 (ref! 2)
    (let c3 (ref! 3)
     (: (begin (set! c1 (+ 1 (get! c1)))
      (set! c2 (+ 1 (get! c2)))
       (set! c3 (+ 1 (get! c3)))
        (+ (get! c1) (+ (get! c2) (get! c3)))) Int ))))  ; ↝ 9
  
  (: (letrec ( (c1 (ref! 1))
           (c2 (ref! 2))
           (c3 (ref! 3)) )
    (+ (get! c1) (+ (get! c2) (get! c3))) ) Int); ↝ 6
  
  (: (letrec ((a +)
           (s -))
    (letrec ((+ 1)
             (- 2))
      (a + -))) Int )                       ; ↝ 3
  
  (: (letrec ((odd  (λ x (if (= x 1) true (even (- x 1)))))
           (even (λ x (if (= x 0) true (odd (- x 1))))))
    (odd 7)) Int)                              ; ↝ True
  
  (: (letrec ((fac (λ x (if (< x 2) x (* x (fac (- x 1)))))))
    (fac 5)) Int)                              ; ↝ 120

```
