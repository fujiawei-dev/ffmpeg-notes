# 编解码器

编解码器（codec）指的是一个能够对一个信号或者一个数据流进行变换的设备或者程序。这里指的变换既包括将信号或者数据流进行编码（通常是为了传输、存储或者加密）或者提取得到一个编码流的操作，也包括为了观察
或者处理从这个编码流中恢复适合观察或操作的形式的操作。编解码器经常用在视频会议和流媒体等应用中，通常主要还是用在广电行业，作前端应用。

经过编码的音频或者视频原始码流经常被叫做 “Essence”，以区别于之后加入码流的元信息和其它用以帮助访问码流和增强码流鲁棒性的数据。

大多数编解码器是有损的，目的是为了得到更大的压缩比和更小的文件大小。当然也有无损的编解码器，但是通常没有必要为了一些几乎注意不到的的质量损失而大大增加编码后文件的大小。除非该编码的结果还将在以后进行下一步的处理，此时连续的有损编码通常会带来较大的质量损失。

很多多媒体数据流需要同时包含音频数据和视频数据，这时通常会加入一些用于音频和视频数据同步的元数据。这三种数据流可能会被不同的程序，进程或者硬件处理，但是当它们传输或者存储的时候，这三种数据通常是
被封装在一起的。通常这种封装是通过视频文件格式来实现的，例如常见的 *.mpg, *.avi, *.mov, *.mp4, *.rm, *.ogg or *.tta，这些格式中有些只能使用某些编解码器，而更多可以以容器的方式使用各种编解码器。

编解码器对应的英文 “codec”（coder 和 decoder 简化而成的合成词语）和 decode 通常指软件，当特指硬件的时候，通常使用 “endec” 这个单词。

硬件编解码器有标清编解码器和高清编解码器。所谓标清，英文为 “Standard Definition”，是物理分辨率在 720p 以下的一种视频格式。720p 是指视频的垂直分辨率为 720 线逐行扫描。具体的说，是指分辨率在 400  线左右的 VCD、DVD、电视节目等“标清”视频格式，即标准清晰度。而物理分辨率达到 720p 以上则称作为高清,（英文表述 High Definition）简称 HD。关于高清的标准，国际上公认的有两条：视频垂直分辨率超过 720p 或 1080i；视频宽纵比为 16：9。