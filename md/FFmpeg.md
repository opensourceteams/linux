# FFmpeg命令行工具的使用 
- http://www.sohu.com/a/273248325_100206743


本文将重点介绍ffmpeg、ffprobe与ffplay这三个命令行工具，而ffserver则是作为简单的流媒体服务器存在的，与客户端开发关系不大，因此本书将不做介绍。前文曾经提到ffmpeg是进行媒体文件转码的命令行工具，ffprobe是用于查看媒体文件头信息的工具，ffplay则是用于播放媒体文件的工具。

下面按照从简单开始的原则，先介绍ffprobe——查看媒体文件格式的工具。

1.ffprobe

首先用ffprobe查看一个音频的文件：

ffprobe ~/Desktop/32037.mp3

键入上述命令之后，先看如下这行信息：

Duration：00:05:14.83，start：0.000000，bitrate：64kb/s

这行信息表明，该音频文件的时长是5分14秒零830毫秒，开始播放时间是0，整个媒体文件的比特率是64Kbit/s，然后再看另外一行：

Stream#0:0 Audio：mp3，24000Hz，stereo，s16p，64kb/s

这行信息表明，第一个流是音频流，编码格式是MP3格式，采样率是24kHz，声道是立体声，采样表示格式是SInt16（short）的planner（平铺格式），这路流的比特率是64Kbit/s。

然后再使用ffprobe查看一个视频的文件：

ffprobe ~/Desktop/32037.mp4

键入上述命令之后，可以看到第一部分的信息是Metadata信息：

Metadata:

major_brand: isom

minor_version: 512

compatible_brands: isomiso2avc1mp41

encoder: Lavf55.12.100

这行信息表明了该文件的Metadata信息，比如encoder是Lavf55.12.100，其中Lavf代表的是FFmpeg输出的文件，后面的编号代表了FFmpeg的版本代号，接下来的一行信息如下：

Duration：00:04:34.56 start：0.023220，bitrate：577kb/s

上面一行的内容表示Duration是4分34秒560毫秒，开始播放的时间是从23ms开始播放的，整个文件的比特率是577Kbit/s，紧接着再来看下一行：

Stream#0:0（un）：Video：h264（avc1/0x31637661），yuv420p，480*480，508kb/s，24fps

这行信息表示第一个stream是视频流，编码方式是H264的格式（封装格式是AVC1），每一帧的数据表示是YUV420P的格式，分辨率是480×480，这路流的比特率是508Kbit/s，帧率是每秒钟24帧（fps是24），紧接着再来看下一行：

Stream#0:1（und）：Audio：aac（LC）（mp4a/0x6134706D），44100Hz，stereo，fltp，63kb/s

这行信息表示第二个stream是音频流，编码方式是AAC（封装格式是MP4A），并且采用的Profile是LC规格，采样率是44100Hz，声道数是立体声，数据表示格式是浮点型，这路音频流的比特率是63Kbit/s。

以上就是使用ffprobe来提取音频文件和视频文件头信息的方式，以及提取出来信息的含义。当然，ffprobe还有比较高级的用法，下面就来介绍几个：

ffprobe -show_format 32037.mp4

上述命令可以输出格式信息format_name、时间长度duration、文件大小size、比特率bit_rate、流的数目nb_streams等。

ffprobe -print_format json -show_streams 32037.mp4

上述命令可以以JSON格式的形式输出具体每一个流最详细的信息，视频中会有视频的宽高信息、是否有b帧、视频帧的总数目、视频的编码格式、显示比例、比特率等信息，音频中会有音频的编码格式、表示格式、声道数、时间长度、比特率、帧的总数目等信息。

显示帧信息的命令如下：

ffprobe -show_frames sample.mp4

查看包信息的命令如下：

ffprobe -show_packets sample.mp4

观影小技巧

日常生活中经常会接触到多媒体文件，比如，在电脑上利用ffprobe工具打开一些国粤双语的文件，一般会看到如下3行Stream。

视频Stream：h264 yuv420P

音频Stream：aac 48000Hz stereo fltp（default）title：粤语

音频Stream：aac 48000Hz stereo fltp title：国语

这就是说明，该媒体文件中有三路流：一路是视频流，另外两路是音频流，默认播放的是粤语的声音流，在大多数的播放器里面都可以进行音频流的切换，可以切换到国语的声音流进行观看。

以上介绍的基本上就是日常工作中经常会使用到的ffprobe命令了，其实大家只需要掌握最重要的查看指令就可以了，下面继续来看下一个命令行工具ffplay。

2.ffplay

前文已经提到过，ffplay是以FFmpeg框架为基础，外加渲染音视频的库libSDL来构建的媒体文件播放器。它所依赖的libSDL是1.2版本的，所以在安装ffplay之前也要安装对应版本的libSDL作为其依赖的组件。之后使用ffplay就非常简单了，比如我们要播放一个音频文件：

ffplay 32037.mp3

这时候会弹出一个窗口，一边播放MP3文件，一边将播放声音的语谱图画到该窗口上。针对该窗口的操作如下，点击窗口的任意一个位置，ffplay会按照点击的位置计算出时间的进度，然后跳（seek）到这个时间点上继续播放；按下键盘上的右键会默认快进10s，左键默认后退10s，上键默认快进1min，下键默认后退1min；按ESC键就是退出播放进程；如果按w键则将绘制音频的波形图等。播放一个视频的命令如下所示：

ffplay 32037.mp4

这时候会直接在新弹出的窗口上播放该视频，如果想要同时播放多个文件，那么只需要在多个命令行下同时执行ffplay就可以了，在对比多个视频质量的时候这是一个操作技巧，此外，如果按s键则可以进入frame-step模式，即按s键一次就会播放下一帧图像，这在观察某些视频内部的帧内容时也是常用的技巧。

业界内开源的ijkPlayer其实就是基于ffplay进行改造的播放器，当然其做了硬件解码以及很多兼容性的工作。ijkPlayer是一款非常优秀的播放器，作为开发者的我们需要很多优秀的开源项目。所以在这里笔者呼吁各家互联网公司开源出自己的部分代码，以提高所在领域的整体水平。

更多的ffplay命令介绍如下：

ffplay 32037.mp4 -loop 10

上述命令代表播放视频结束之后会从头再次播放，共循环播放10次。还记得前文中提到过的两路流吗？ffplay也做了这方面的适配，也就是说在ffplay中其实也可以指定使用哪一路音频流或者视频流，命令如下：

ffplay 大话西游.mkv -ast 1

上述命令表示播放视频中的第一路音频流，如果参数ast后面跟的是2，那么就播放第二路音频流，如果没有第二路音频流的话，就会静音，同样也可以设置参数vst，比如：

ffplay 大话西游.mkv -vst 1

上述命令表示播放视频中的第一路视频流，如果参数vst后面跟的是2，那么就播放第二路视频流，但是如果没有第二路视频流，就会是黑屏即什么都不显示。

接下来介绍开发工作中常用的几个命令，这些命令在工作中debug的时候非常有用。首先用ffplay播放裸数据，无论是音频的pcm文件还是视频帧原始格式表示的数据（YUV420P或者rgba）。下面先来看看音频pcm文件的播放命令：

ffplay song.pcm -f s16le -channels 2 -ar 44100

仅键入上述这行命令其实就可以正常播放song.pcm了，当然，前提是格式（-f）、声道数（-channels）、采样率（-ar）必须设置正确，如果其中任何一项参数设置不正确，都不会得到正常的播放结果。第1章在讲音频的基础概念时已经提到过，WAV格式的文件称为无压缩的格式，其实就是在PCM的头部添加44个字节，用于标识这个PCM的采样表示格式、声道数、采样率等信息，对于WAV格式音频文件，ffplay肯定可以直接播放，但是若让ffplay播放PCM裸数据的话，只要为其提供上述三个主要的信息，那么它就可以正确地播放了。

然后再来看一帧视频帧的播放，首先是YUV420P格式的视频帧：

ffplay -f rawvideo -pixel_format yuv420p -s 480*480 texture.yuv

其实对于一帧视频帧，或者更直接来说一张PNG或者JPEG的图片，直接用ffplay是可以显示或播放的，当然PNG或者JPEG都会在其头部信息里面指明这张图片的宽高以及格式表示。若想让ffplay显示一张YUV的原始数据表示的图片，那么需要告诉ffplay一些重要的信息，其中包括格式（-f rawvideo代表原始格式）、表示格式（-pixel_format yuv420p）、宽高（-s 480*480）。对于RGB表示的图像，其实是一样的，命令如下：

ffplay -f rawvideo -pixel_format rgb24 -s 480*480 texture.rgb

上述代码是播放rgb的原始数据，当然还需要指明前面提到的三项基本信息。

另外，对于视频播放器，不得不提的一个问题就是音画同步，在ffplay中音画同步的实现方式其实有三种，分别是：以音频为主时间轴作为同步源；以视频为主时间轴作为同步源；以外部时钟为主时间轴作为同步源。下面就以音频为主时间轴来作为同步源来作为案例进行讲解，这也是后面章节中完成视频播放器项目时要使用到的对齐策略，并且在ffplay中默认的对齐方式也是以音频为基准进行对齐的，那么以音频作为对齐基准是如何实现的呢？

首先要声明的是，播放器接收到的视频帧或者音频帧，内部都会有时间戳（PTS时钟）来标识它实际应该在什么时刻进行展示。实际的对齐策略如下：比较视频当前的播放时间和音频当前的播放时间，如果视频播放过快，则通过加大延迟或者重复播放来降低视频播放速度；如果视频播放慢了，则通过减小延迟或者丢帧来追赶音频播放的时间点。关键就在于音视频时间的比较以及延迟的计算，当然在比较的过程中会设置一个阈值（Threshold），若超过预设的阈值就应该做调整（丢帧渲染或者重复渲染），这就是整个对齐策略。

对于ffplay可以明确指明使用的到底是哪一种具体的对齐方式，比如：

ffplay 32037.mp4 -sync audio

上述命令显式地指定了ffplay使用音频为基准进行音视频同步，用来播放文件32037.mp4，当然这也是ffplay的默认设置（就是写与不写都一样）。

ffplay 32037.mp4 -sync video

上述命令显式地指定了使用以视频为基准进行音视频同步的方式播放视频文件。

ffplay 32037.mp4 -sync ext

上述命令显式地指定了使用外部时钟作为基准进行音视频同步的方式，用来播放视频文件。

大家可以分别使用这三种方式进行播放，尝试着去听一听，做一些快进操作或者直接跳（seek）到某个位置的操作，观察一下不同的对齐策略对最终的播放具体会造成什么样的影响。

3.ffmpeg

ffmpeg其实是这三个命令行工具里最强大的一个工具，如果说ffprobe是用于探测媒体文件的格式以及详细信息，ffplay是一个播放媒体文件的工具，那么ffmpeg就是强大的媒体文件转换工具。它可以转换任何格式的媒体文件，并且还可以用自己的AudioFilter以及VideoFilter进行处理和编辑，总之一句话，有了它，进行离线处理视频时可以做任何你想做的事情了。下面先介绍总体的参数，然后再列出经典场景下的使用案例。

（1）通用参数

·-f fmt：指定格式（音频或者视频格式）。

·-i filename：指定输入文件名，在Linux下当然也能指定：0.0（屏幕录制）或摄像头。

·-y：覆盖已有文件。

·-t duration：指定时长。

·-fs limit_size：设置文件大小的上限。

·-ss time_off：从指定的时间（单位为秒）开始，也支持[-]hh：mm：ss[.xxx]的格式。

·-re：代表按照帧率发送，尤其在作为推流工具的时候一定要加入该参数，否则ffmpeg会按照最高速率向流媒体服务器不停地发送数据。

·-map：指定输出文件的流映射关系。例如：“-map 1：0-map 1：1”要求将第二个输入文件的第一个流和第二个流写入输出文件。如果没有-map选项，则ffmpeg采用默认的映射关系。

（2）视频参数

·-b：指定比特率（bit/s），ffmpeg是自动使用VBR的，若指定了该参数则使用平均比特率。

·-bitexact：使用标准比特率。

·-vb：指定视频比特率（bits/s）。

·-r rate：帧速率（fps）。

·-s size：指定分辨率（320×240）。

·-aspect aspect：设置视频长宽比（4：3，16：9或1.3333，1.7777）。

·-croptop size：设置顶部切除尺寸（in pixels）。

·-cropbottom size：设置底部切除尺寸（in pixels）。

·-cropleft size：设置左切除尺寸（in pixels）。

·-cropright size：设置右切除尺寸（in pixels）。

·-padtop size：设置顶部补齐尺寸（in pixels）。

·-padbottom size：底补齐（in pixels）。

·-padleft size：左补齐（in pixels）。

·-padright size：右补齐（in pixels）。

·-padcolor color：补齐带颜色（000000-FFFFFF）。

·-vn：取消视频的输出。

·-vcodec codec：强制使用codec编解码方式（'copy'代表不进行重新编码）。

（3）音频参数

·-ab：设置比特率（单位为bit/s，老版的单位可能是Kbit/s），对于MP3格式，若要听到较高品质的声音则建议设置为160Kbit/s（单声道则设置为80Kbit/s）以上。

·-aq quality：设置音频质量（指定编码）。

·-ar rate：设置音频采样率（单位为Hz）。

·-ac channels：设置声道数，1就是单声道，2就是立体声。

·-an：取消音频轨。

·-acodec codec：指定音频编码（'copy'代表不做音频转码，直接复制）。

·-vol volume：设置录制音量大小（默认为256）<百分比>。

以上就是日常开发中经常用到的音视频参数以及通用参数，若只介绍这些参数，读者肯定会觉得比较迷茫，下面就结合日常开发中遇到的场景逐个给出具体的实例来实践一下。

1）列出ffmpeg支持的所有格式：

ffmpeg -formats

2）剪切一段媒体文件，可以是音频或者视频文件：

ffmpeg -i input.mp4 -ss 00:00:50.0 -codec copy -t 20 output.mp4

表示将文件input.mp4从第50s开始剪切20s的时间，输出到文件output.mp4中，其中-ss指定偏移时间（time Offset），-t指定的时长（duration）。

3）如果在手机中录制了一个时间比较长的视频无法分享到微信中，那么可以使用ffmpeg将该视频文件切割为多个文件：

ffmpeg -i input.mp4 -t 00:00:50 -c copy small-1.mp4 -ss 00:00:50 -codec copy

small-2.mp4

4）提取一个视频文件中的音频文件：

ffmpeg -i input.mp4 -vn -acodec copy output.m4a

5）使一个视频中的音频静音，即只保留视频：

ffmpeg -i input.mp4 -an -vcodec copy output.mp4

6）从MP4文件中抽取视频流导出为裸H264数据：

ffmpeg -i output.mp4 -an -vcodec copy -bsf:v h264_mp4toannexb output.h264

注意，上述指令里不使用音频数据（-an），视频数据使用mp4toannexb这个bitstream filter来转换为原始的H264数据，在后续的API章节中也会频繁使用到该bitstream filter，在前面的章节中也曾提到过同一编码会有不同的封装格式。

7）使用AAC音频数据和H264的视频生成MP4文件：

ffmpeg -i test.aac -i test.h264 -acodec copy -bsf:a aac_adtstoasc -vcodec copy -f

mp4 output.mp4

上述代码中使用了一个名为aac_adtstoasc的bitstream filter，AAC格式也有两种封装格式，前面的章节中也曾提到过，而且在后续的章节中也会继续使用API调用该bitstream filter。

8）对音频文件的编码格式做转换：

ffmpeg -i input.wav -acodec libfdk_aac output.aac

9）从WAV音频文件中导出PCM裸数据：

ffmpeg -i input.wav -acodec pcm_s16le -f s16le output.pcm

这样就可以导出用16个bit来表示一个sample的PCM数据了，并且每个sample的字节排列顺序都是小尾端表示的格式，声道数和采样率使用的都是原始WAV文件的声道数和采样率的PCM数据。

10）重新编码视频文件，复制音频流，同时封装到MP4格式的文件中：

ffmpeg -i input.flv -vcodec libx264 -acodec copy output.mp4

11）将一个MP4格式的视频转换成为gif格式的动图：

ffmpeg -i input.mp4 -vf scale=100:-1 -t 5 -r 10 image.gif

上述代码按照分辨比例不动宽度改为100（使用VideoFilter的scaleFilter），帧率改为10（-r），只处理前5秒钟（-t）的视频，生成gif。

12）将一个视频的画面部分生成图片，比如要分析一个视频里面的每一帧都是什么内容的时候，可能就需要用到这个命令了：

ffmpeg -i output.mp4 -r 0.25 frames_%04d.png

上述命令每4秒钟截取一帧视频画面生成一张图片，生成的图片从frames_0001.png开始一直递增下去。

13）使用一组图片可以组成一个gif，如果你连拍了一组照片，就可以用下面这行命令生成一个gif：

ffmpeg -i frames_%04d.png -r 5 output.gif

14）使用音量效果器，可以改变一个音频媒体文件中的音量：

ffmpeg -i input.wav -af ‘volume=0.5’ output.wav

上述命令是将input.wav中的声音减小一半，输出到output.wav文件中，可以直接播放来听，或者放到一些音频编辑软件中直接观看波形幅度的效果。

15）淡入效果器的使用：

ffmpeg -i input.wav -filter_complex afade=t=in:ss=0:d=5 output.wav

上述命令可以将input.wav文件中的前5s做一个淡入效果，输出到output.wav中，可以将处理之前和处理之后的文件拖到Audacity音频编辑软件中查看波形图。

16）淡出效果器的使用：

ffmpeg -i input.wav -filter_complex afade=t=out:st=200:d=5 output.wav

上述命令可以将input.wav文件从200s开始，做5s的淡出效果，并放到output.wav文件中。

17）将两路声音进行合并，比如要给一段声音加上背景音乐：

ffmpeg -i vocal.wav -i accompany.wav -filter_complex

amix=inputs=2:duration=shortest output.wav

上述命令是将vocal.wav和accompany.wav两个文件进行mix，按照时间长度较短的音频文件的时间长度作为最终输出的output.wav的时间长度。

18）对声音进行变速但不变调效果器的使用：

ffmpeg -i vocal.wav -filter_complex atempo=0.5 output.wav

上述命令是将vocal.wav按照0.5倍的速度进行处理生成output.wav，时间长度将会变为输入的2倍。但是音高是不变的，这就是大家常说的变速不变调。

19）为视频增加水印效果：

ffmpeg -i input.mp4 -i changba_icon.png -filter_complex

'[0:v][1:v]overlay=main_w-overlay_w-10:10:1[out]' -map '[out]' output.mp4

上述命令包含了几个内置参数，main_w代表主视频宽度，overlay_w代表水印宽度，main_h代表主视频高度，overlay_h代表水印高度。

20）视频提亮效果器的使用：

ffmpeg -i input.flv -c:v libx264 -b:v 800k -c:a libfdk_aac -vf eq=brightness=0.25

-f mp4 output.mp4

提亮参数是brightness，取值范围是从-1.0到1.0，默认值是0。

21）为视频增加对比度效果：

ffmpeg -i input.flv -c:v libx264 -b:v 800k -c:a libfdk_aac -vf eq=contrast=1.5 -f

mp4 output.mp4

对比度参数是contrast，取值范围是从-2.0到2.0，默认值是1.0。

22）视频旋转效果器的使用：

ffmpeg -i input.mp4 -vf "transpose=1" -b:v 600k output.mp4

23）视频裁剪效果器的使用：

ffmpeg -i input.mp4 -an -vf "crop=240:480:120:0" -vcodec libx264 -b:v 600k output.mp4

24）将一张RGBA格式表示的数据转换为JPEG格式的图片：

ffmpeg -f rawvideo -pix_fmt rgba -s 480*480 -i texture.rgb -f image2 -vcodec mjpeg

output.jpg

25）将一个YUV格式表示的数据转换为JPEG格式的图片：

ffmpeg -f rawvideo -pix_fmt yuv420p -s 480*480 -i texture.yuv -f image2 -vcodec mjpeg

output.jpg

26）将一段视频推送到流媒体服务器上：

ffmpeg -re -i input.mp4 -acodec copy -vcodec copy -f flv rtmp://xxx

上述代码中，rtmp：//xxx代表流媒体服务器的地址，加上-re参数代表将实际媒体文件的播放速度作为推流速度进行推送。

27）将流媒体服务器上的流dump到本地：

ffmpeg -i http://xxx/xxx.flv -acodec copy -vcodec copy -f flv output.flv

上述代码中，http://xxx/xxx.flv 代表一个可以访问的视频网络地址，可按照复制视频流格式和音频流格式的方式，将文件下载到本地的output.flv媒体文件中。

28）将两个音频文件以两路流的形式封装到一个文件中，比如在K歌的应用场景中，原伴唱实时切换的场景下，可以使用一个文件包含两路流，一路是伴奏流，另外一路是原唱流：

ffmpeg -i 131.mp3 -i 134.mp3 -map 0:a -c:a:0 libfdk_aac -b:a:0 96k -map 1:a -c:a:1

libfdk_aac -b:a:1 64k -vn -f mp4 output.m4a

其实，FFmpeg的命令工具随意组合的话会有很多种，这里只列举了一部分，如果大家弄清楚了FFmpeg能做什么，那么具体的使用就一目了然了，只要是FFmpeg框架能够实现的功能，那么FFmpeg命令行工具就能将其提供出来了。