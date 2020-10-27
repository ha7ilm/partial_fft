# Partial FFT

This repo shows how to build a special kind of FFT:
- it takes N/2 values and N/2 coefficients
- it outputs some valid set of N/2 values and N/2 coefficients

First, have a look at the classic FFT: [`classic_fft.py`](classic_fft.py)

Then note that we can:
 - separate the even and odd outputs (`a` and `b`)
 - think of the inputs as two halfs (the deepest call depth always deals with a value from the first half, and from the second half of the input)
 
It's still an FFT, and does the same thing.
See [`alike_fft.py`](alike_fft.py).

Then, can we drop some of the work? Sure,
we can not generate any of the odd-index outputs.
See [`half_out.py`](half_out_fft.py).

And now a magic trick: can we drop the 2nd half of the inputs,
 and deduce what they could have been, along with the missing outputs?

Yes that is possible, see [`partial_fft.py`](partial_fft.py) (**testing in progress, there may be _bugs_**).
Again, the deepst call depth only deals with 1 value from the original input left half, and 1 value from the right half.
We can only give it the left half, and have it solve for the value from right half, based on expected output.

The idea is then to extend `N` values into `2N`, suiting an FFT with `2N` values, all even outputs being `0`.

The other way around, providing only the second half of the inputs, is also possible, see [`other_partial_fft.py`](./other_partial_fft.py)

And can it be even faster? Yes, kind of. We can drop half of the work, either the don't generate the missing inputs, or don't generate the missing outputs.
Generate the missing inputs (right half): [`input_extension_fft.py`](input_extension_fft.py).
Generate the missing outputs (odd outputs): [`output_extension_fft.py`](output_extension_fft.py).

Note that the input and output extension FFTs are almost interchangeable:
to extend inputs with the output-extension FFT, you could use the inverse-FFT of the output-extension FFT.  


## Benchmarks

```
          FFT: scale_8            200 ops         1186496 ns/op
  Partial FFT: scale_8            200 ops         1128262 ns/op
OtherPart FFT: scale_8            200 ops         1167200 ns/op
 InputExt FFT: scale_8            200 ops          523955 ns/op
OutputExt FFT: scale_8            200 ops          882554 ns/op
```

