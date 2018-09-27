# VQ based SR4X speech classification

## Codebook generation

Codebooks generated per class and only using the corresponding training
predictors:

```
for class in `ls data/predictors/TRAIN`; do
  vq.learn -P 12 -e 0.0005 -w "$class" data/predictors/TRAIN/$class/*.prd &;
done
```

## VQ classification

### M=512 on training instances

```
vq.classify data/codebooks/[a-z0-9]*/eps_0.0005__0512.cbook data/predictors/TRAIN/*/*.prd

Loading models:
 0: data/codebooks/71523/eps_0.0005__0512.cbook                  : '71523'
 1: data/codebooks/abracadabra/eps_0.0005__0512.cbook            : 'abracadabra'
 2: data/codebooks/computer/eps_0.0005__0512.cbook               : 'computer'
 3: data/codebooks/generation/eps_0.0005__0512.cbook             : 'generation'
 4: data/codebooks/nebula/eps_0.0005__0512.cbook                 : 'nebula'
 5: data/codebooks/sungeeta/eps_0.0005__0512.cbook               : 'sungeeta'
 6: data/codebooks/supernova/eps_0.0005__0512.cbook              : 'supernova'
 7: data/codebooks/tektronix/eps_0.0005__0512.cbook              : 'tektronix'

****************************************************************************************************************************************************************************************************************************************************

              Confusion matrix:
                     0   1   2   3   4   5   6   7     tests   errors

        71523   0   56   0   0   0   0   0   0   0       56       0
  abracadabra   1    0  45   0   0   0   0   0   0       45       0
     computer   2    0   0  19   0   0   0   0   0       19       0
   generation   3    0   0   0  12   0   0   0   0       12       0
       nebula   4    0   0   0   0  22   0   0   0       22       0
     sungeeta   5    0   0   0   0   0  22   0   0       22       0
    supernova   6    0   0   0   0   0   0  37   0       37       0
    tektronix   7    0   0   0   0   0   0   0  31       31       0

              class     accuracy    tests      candidate order
        71523     0      100.00%    56         56   0   0   0   0   0   0   0
  abracadabra     1      100.00%    45         45   0   0   0   0   0   0   0
     computer     2      100.00%    19         19   0   0   0   0   0   0   0
   generation     3      100.00%    12         12   0   0   0   0   0   0   0
       nebula     4      100.00%    22         22   0   0   0   0   0   0   0
     sungeeta     5      100.00%    22         22   0   0   0   0   0   0   0
    supernova     6      100.00%    37         37   0   0   0   0   0   0   0
    tektronix     7      100.00%    31         31   0   0   0   0   0   0   0

                TOTAL    100.00%   244        244   0   0   0   0   0   0   0
```



### M=512 on test instances:

```
vq.classify data/codebooks/[a-z0-9]*/eps_0.0005__0512.cbook data/predictors/TEST/*/*.prd

Loading models:
 0: data/codebooks/71523/eps_0.0005__0512.cbook                  : '71523'
 1: data/codebooks/abracadabra/eps_0.0005__0512.cbook            : 'abracadabra'
 2: data/codebooks/computer/eps_0.0005__0512.cbook               : 'computer'
 3: data/codebooks/generation/eps_0.0005__0512.cbook             : 'generation'
 4: data/codebooks/nebula/eps_0.0005__0512.cbook                 : 'nebula'
 5: data/codebooks/sungeeta/eps_0.0005__0512.cbook               : 'sungeeta'
 6: data/codebooks/supernova/eps_0.0005__0512.cbook              : 'supernova'
 7: data/codebooks/tektronix/eps_0.0005__0512.cbook              : 'tektronix'

************************

              Confusion matrix:
                     0   1   2   3   4   5   6   7     tests   errors

        71523   0    6   0   0   0   0   0   0   0        6       0
  abracadabra   1    0   4   0   0   0   0   0   0        4       0
     computer   2    0   0   2   0   0   0   0   0        2       0
   generation   3    0   0   0   1   0   0   0   0        1       0
       nebula   4    0   0   0   0   2   0   0   0        2       0
     sungeeta   5    0   0   0   0   0   2   0   0        2       0
    supernova   6    0   0   0   0   0   0   4   0        4       0
    tektronix   7    0   0   0   0   0   0   0   3        3       0

              class     accuracy    tests      candidate order
        71523     0      100.00%     6          6   0   0   0   0   0   0   0
  abracadabra     1      100.00%     4          4   0   0   0   0   0   0   0
     computer     2      100.00%     2          2   0   0   0   0   0   0   0
   generation     3      100.00%     1          1   0   0   0   0   0   0   0
       nebula     4      100.00%     2          2   0   0   0   0   0   0   0
     sungeeta     5      100.00%     2          2   0   0   0   0   0   0   0
    supernova     6      100.00%     4          4   0   0   0   0   0   0   0
    tektronix     7      100.00%     3          3   0   0   0   0   0   0   0

                TOTAL    100.00%    24         24   0   0   0   0   0   0   0
```

### Summary

Summary of accuracy with all codebook sizes:

    codebook size \  on training  | on test
             2048   100.00%       | 100.00%
             1024   100.00%       | 100.00%
              512   100.00%       | 100.00%
              256   100.00%       | 100.00%
              128   100.00%       | 100.00%
               64   100.00%       | 100.00%
               32   100.00%       | 100.00%
               16    98.77%       | 100.00%
                8    92.62%       | 100.00%
                4    84.84%       |  91.67%
                2    81.97%       |  87.50%
