# stb_fft
a single-file fast fourier transform library (suitable for power of 2 and non-power of 2)

## example

```C
#ifndef STB_FFT_IMPLEMENTAION
#define STB_FFT_IMPLEMENTAION
#endif

#include "stb_fft.h"
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>


#ifndef max
#define max(a, b) ((a) > (b) ? (a) : (b))
#endif


void fftResample(float *input, float *output, int input_size, int output_size) {
    cmplx *samples = (cmplx *) calloc(max(input_size, output_size), sizeof(cmplx));
    if (samples == NULL) {
        return;
    }
    STB_FFT_R2C(input, samples, input_size);
    if (output_size < input_size) {
        // remove some high frequency samples;
        int half_output = (output_size / 2);
        int diff_size = input_size - output_size;
        memset(samples + half_output, 0, diff_size * sizeof(cmplx));
    } else if (output_size > input_size) {
        int half_input = input_size / 2;
        // add some high frequency zero samples
        int diff_size = output_size - input_size;
        memmove(samples + half_input + diff_size, samples + half_input, half_input * sizeof(cmplx));
        memset(samples + half_input, 0, diff_size * sizeof(cmplx));
    }
    STB_IFFT_C2R(samples, output, output_size);
    float norm = 1.0f / input_size;
    for (int i = 0; i < output_size; i++) {
        output[i] = output[i] * norm;
    }
    free(samples);
}

int main() {
    printf("a single-file fast fourier transform library.\n");
    printf("blog: http://cpuimage.cnblogs.com/\n");
    const int inSize = 4;
    const int outSize = 8;
    float input[inSize];
    float output[outSize];
    printf("\ninput :\n");
    for (int i = 0; i < inSize; ++i) {
        input[i] = i;
        printf("%f \t", input[i]);
    }
    fftResample(input, output, inSize, outSize);
    printf("\noutput :\n");
    for (int i = 0; i < outSize; ++i) {
        printf("%f \t", output[i]);
    }
    printf("\npress any key to exit. \n");
    getchar();
}

```
