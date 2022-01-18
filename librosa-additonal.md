# 二、补充几个概念以及对部分API的理解
> 注：时域信号在经过傅里叶变换转换之后一般为复数形式
## **amplitude和magnitude的区别**
* peak amplitude, often shortened to amplitude, is the nonnegative value of the waveform's peak (either positive or negative).(强调信号的峰值)
* instantaneous amplitude of x is the value of x(t) (either positive or negative) at time t.（瞬时幅度，信号x(t)在t处的值）
* instantaneous magnitude, or simply magnitude, of x is nonnegative and is given by |x(t)|.（瞬时幅度，信号x(t)的模）
需要特意强调的就是，magnitude指的是模，不过在多数情况下，amplitude和magnitude在fft中一般是同一个意思。

## **振幅，分贝(dB)和能量(power)之间的转换关系**
* 振幅：由于傅里叶变换之后得到的为复数T-F矩阵D，在python里，可以简单得到振幅的T-F矩阵和相位(幅角)的T-F矩阵：
```
S = np.abs(D)
P = np.angle(D)
```
* 分贝：librosa的官方文档中对分贝的计算公式定义如下：
```
S_db = 20 * np.log10(S / ref)
```
其中ref的默认取值为1.0，不过文档似乎更习惯于用`ref = np.max(S)`

* 能量：T-F矩阵中每个T-F单元的能量就是幅值取平方即可
```
S_power = S ** 2
```
## **librosa中xx_to_xx型的API**
* `amplitude_to_db(S[, ref, amin, top_db])`
该API是将幅值转换为分贝，这里的第一个参数S，按照librosa相关术语的描述，应该是一个实数T-F矩阵而不是复数T-F矩阵，但实际测试的时候发现API传入S和D的效果是一样的，源码内部实际上有一句`magnitude = np.abs(S)`，那么说明参数中的S实际上是被当成D来看的，D中的每个瞬时值被称为amplitude，不过从字面意义上来看，还是当成实数T-F矩阵S来看比较好，也就是说时域信号`y`经过STFT变换后得到`D`，先经过`S = np.abs(D)`之后再传入该API中的S，当然直接传入`D`也没有任何问题。此外该`amplitude_to_db(S)`与`power_to_db(S ** 2)`等价。

* `db_to_amplitude(S_db[, ref])`
将分贝转换为幅值。这里的幅值是实数，用magnitude更合适，强调模S。

* `power_to_db(S[, ref, amin, top_db, ref_power])`
能量转换为分贝，ref_power参数会在0.6.0版本起抛弃。

* `db_to_power(S_db[, ref])`
分贝转换为能量。

# just a git test
