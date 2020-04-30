---
title: "Hello World"
date: 2020-04-30T11:01:13+01:00
draft: false
---

This is a test of the Bionics General resource site! There is a long ways to go,
but this is a start!

*Have some Lisp code*

```lisp
(defun hamming-distance (dna1 dna2)
  "Number of positional differences in two equal length dna strands."
  (when (= (length dna1) (length dna2))
    (count nil (map 'list #'char= dna1 dna2))))
```

**Yo dawg, I heard you liked maths...**

{{< katex >}}
x = {-b \pm \sqrt{b^2-4ac} \over 2a}
{{< /katex >}}
