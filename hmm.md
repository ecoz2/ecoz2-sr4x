# VQ/HMM based SR4X speech classification


## Codebook generation

Codebooks trained with all designated training predictors:

```
vq.learn -P 12 -e 0.0005 data/predictors/TRAIN/*/*.prd
```

## Vector quantization

With M=1024 (but other sizes also tested, see below):

```
vq.quantize data/codebooks/_/eps_0.0005__1024.cbook data/predictors/*/*/prd
```

This generates both training and testing observation sequences.

## HMM training

We train an HMM model per class and only using the corresponding
training sequences:

```
for class in `ls data/sequences/TRAIN/M1024`; do
  hmm.learn -a 0.1 -N 16 data/sequences/TRAIN/M1024/$class/*.seq &;
done
```

## HMM classification

### On training sequences:

```
hmm.classify data/hmms/N16__M1024_t3__a0.1/*hmm data/sequences/TRAIN/M1024/*/*.seq

Loading models:
 0: data/hmms/N16__M1024_t3__a0.1/71523.hmm
 1: data/hmms/N16__M1024_t3__a0.1/abracadabra.hmm
 2: data/hmms/N16__M1024_t3__a0.1/computer.hmm
 3: data/hmms/N16__M1024_t3__a0.1/generation.hmm
 4: data/hmms/N16__M1024_t3__a0.1/nebula.hmm
 5: data/hmms/N16__M1024_t3__a0.1/sungeeta.hmm
 6: data/hmms/N16__M1024_t3__a0.1/supernova.hmm
 7: data/hmms/N16__M1024_t3__a0.1/tektronix.hmm

*****************************************************************************************************_****************************************************_**********************************************************_******************************

              Confusion matrix:
                     0   1   2   3   4   5   6   7     tests   errors

        71523   0   56   0   0   0   0   0   0   0       56       0
  abracadabra   1    0  45   0   0   0   0   0   0       45       0
     computer   2    1   0  18   0   0   0   0   0       19       1
   generation   3    0   0   0  12   0   0   0   0       12       0
       nebula   4    0   0   0   0  22   0   0   0       22       0
     sungeeta   5    0   0   0   0   0  21   1   0       22       1
    supernova   6    0   0   0   0   0   0  37   0       37       0
    tektronix   7    1   0   0   0   0   0   0  30       31       1

              class     accuracy    tests      candidate order
        71523     0      100.00%    56         56   0   0   0   0   0   0   0
  abracadabra     1      100.00%    45         45   0   0   0   0   0   0   0
     computer     2       94.74%    19         18   1   0   0   0   0   0   0
   generation     3      100.00%    12         12   0   0   0   0   0   0   0
       nebula     4      100.00%    22         22   0   0   0   0   0   0   0
     sungeeta     5       95.45%    22         21   0   1   0   0   0   0   0
    supernova     6      100.00%    37         37   0   0   0   0   0   0   0
    tektronix     7       96.77%    31         30   1   0   0   0   0   0   0

                TOTAL     98.77%   244        241   2   1   0   0   0   0   0
```

### On test sequences:

```
hmm.classify data/hmms/N16__M1024_t3__a0.1/*hmm data/sequences/TEST/M1024/*/*.seq

Loading models:
 0: data/hmms/N16__M1024_t3__a0.1/71523.hmm
 1: data/hmms/N16__M1024_t3__a0.1/abracadabra.hmm
 2: data/hmms/N16__M1024_t3__a0.1/computer.hmm
 3: data/hmms/N16__M1024_t3__a0.1/generation.hmm
 4: data/hmms/N16__M1024_t3__a0.1/nebula.hmm
 5: data/hmms/N16__M1024_t3__a0.1/sungeeta.hmm
 6: data/hmms/N16__M1024_t3__a0.1/supernova.hmm
 7: data/hmms/N16__M1024_t3__a0.1/tektronix.hmm

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

### With other codebook sizes

With N=16, summary of accuracy with various codebook sizes:

```
codebook size \  on training  | on test
         2048    99.18%       |  95.83%
         1024    98.77%       | 100.00%
          512    99.18%       |  95.83%
          256    98.77%       | 100.00%
          128    99.59%       |  95.83%
           64    97.13%       |  95.83%
           32    96.31%       |  87.50%
           16    93.03%       |  83.33%
            8    82.38%       |  75.00%
            4    80.33%       |  62.50%
            2    68.85%       |  62.50%
```

With N=8, summary of accuracy with various codebook sizes:

```
codebook size \  on training  | on test
         2048    99.76%       | 95.12%
         1024    99.53%       | 95.12%
          512    99.76%       | 95.12%
          256    99.53%       | 95.12%
          128    98.82%       | 95.12%
           64    98.34%       | 92.68%
           32    96.45%       | 90.24%
           16    92.42%       | 85.37%
            8    72.99%       | 78.05%
            4    63.51%       | 58.54%
            2    38.39%       | 31.71%
```
