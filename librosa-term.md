# 一、librosa中的相关术语介绍
>注：librosa是一个音频处理库，戳[这里](http://librosa.github.io/librosa/)看官方document。
## 中英文对应一览表
|中文|英文|
|:--:|:--:|
|time series|时间序列|
|sampling rate|采样频率|
|frame|帧|
|window|窗（函数）|
|frame length|帧长|
|hop length|帧移|
|window length|窗大小|
|spectrogram|时频矩阵|
|帧起点数组|onset(strength) envelope|

## 术语解释
### **time series**
Typically an audio signal, denoted by `y`, and represented as a one-dimensional numpy.ndarray of floating-point values. `y[t]` corresponds to amplitude of the waveform at sample `t`.
时间序列：一般指的是音频信号，用`y`来表示，`y`是一个浮点类型的一维数组，`y[t]`表示波形图中样本`t`处的幅度值
### **sampling rate**
The (positive integer) number of samples per second of a time series. This is denoted by an integer variable `sr`.
采样频率：时间序列中，每秒的样本数目（一个正整数）。经常用一个整型变量`sr`来表示
### **frame**
A short slice of a time series used for analysis purposes. This usually corresponds to a single column of a spectrogram matrix.
帧：一个时间序列的短切片，常用于分析。帧通常与时频矩阵中一个单独的列相关（specgram的行表示频率，列表示时间），强调一点，帧的意义是时间上的。
### **window**
The (positive integer) number of samples in an analysis window (or frame). This is denoted by an integer variable `n_fft`.
在一个分析窗（或者帧）内的样本的数目（一个正整数）。用整型变量`n_fft`来表示。
注：1、傅里叶变换只能对作用于有限长度的信号，因此才要对信号采样，采样数目`n_fft`就是一帧的长度
2、帧的中心样本称为center，`stft`等函数中，`center`参数若为`True`，信号y会被左右填充(padded)以确保第`k`帧以`k*hop_length`那个样本为中心（v3.0开始），若为`False`，则不填充而且第k帧的起点就是`k*hop_length`那个样本。对应到specgram中就是`D[:, t]`或者`S[:, t]`
center : boolean
* If `True`, the signal `y` is padded so that frame `D[:, t]` is centered at `y[t * hop_length]`.
* If `False`, then `D[:, t]` begins at `y[t * hop_length]`
### **hop length**
The number of samples between successive frames, e.g., the columns of a spectrogram. This is denoted as a positive integer `hop_length`.
帧移：连续帧之间的样本数目，用一个正整数变量`hop_length`来表示。注：其实就是上一帧的起点到下一帧起点的位移
### **window length**
The length (width) of the window function (e.g., Hann window). Note that this can be smaller than the frame length used in a short-time Fourier transform. Typically denoted as a positive integer variable `win_length`.
窗长度：窗的长度（宽度），在短时傅里叶变换中可以比帧长度小（多数时候两者长度一样，窗长小于帧长时使用零填充补充至长度相等）。经常用正整数变量`win_length`来表示
### **spectrogram**
A matrix `S` where the rows index frequency bins, and the columns index frames (time). Spectrograms can be either real-valued or complex-valued. By convention, real-valued spectrograms are denoted as numpy.ndarrays `S`, while complex-valued STFT matrices are denoted as `D`.
时频矩阵：一个矩阵，其行索引频带，列索引帧（时间）。时频矩阵的元素可以是实数也可以是复数。按照约定俗成的表示，实数时频矩阵用数组`S`表示，而复数STFT矩阵则用`D`表示
`np.abs(D[f, t])` is the magnitude of frequency bin `f` at frame `t`
`np.angle(D[f, t])` is the phase of frequency bin `f` at frame `t`
### **onset (strength) envelope**
An onset envelope `onset_env[t]` measures the strength of note onsets at frame `t`. Typically stored as a one-dimensional numpy.ndarray of floating-point values onset_envelope.
帧起点数组：记录着每一帧起始时刻处的强度，用一个浮点型一维数组`onset_envelope`来表示






