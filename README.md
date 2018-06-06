## Mac 外接2k显示器开启hidpi的方法

#### 关闭System Integrity Protection SIP

开机的时候按command+R进入恢复模式
```
csrutil disable
csrutil enable
```
#### 开启mac的hidpi

```
sudo defaults write /Library/Preferences/com.apple.windowserver DisplayResolutionEnabled -bool YES
```

#### 获取2k显示器的DisplayVendorID和DisplayProductID
```
ioreg -lw0 | grep IODisplayPrefsKey | grep -o '/[^/]\+"$'
```
>下面网址可以一键生成
https://wacky.one/blog/macos-hi-dpi/#copy-conf

关注AppleDisplay-**-**，-分隔了两个十六进制数。第一个为VendorID，第二个为ProductID。以我的环境为例：VendorID为10ac，ProductID为a0c4。
我们新建一个名字为DisplayVendorID-XXXX的文件夹，其中XXXX是DisplayVendorID的16进制小写即9d1，则文件夹名字为DisplayVendorID-9d1。然后再创建一个空白文件,将这个文件命名为DisplayProductID-YYYY，其中YYYY即DisplayProductID的16进制小写即801b。

#### 编辑DisplayProductID-YYYY文件

使用PlistEdit Pro去打开这个文件，然后在DisplayProductID和DisplayVendorID处填写这两个值的10进制原始值，然后下面按照如下规则去设置对应的分辨率。
例如我这里要设置 1920 * 1080 hidpi 的设置，我设置 1920 * 1080 和 3840 * 2160 两种。
1920的16进制是00000780，1080的16进制是00000438，后面需要拼接上00000001 00200000，
```
00000780 00000438 00000001 00200000
```
3840的16进制是00000F00，2160的16进制是00000870，后面需要拼接上00000001 00200000
```
00000F00 00000870 00000001 00200000
```
我们将这个数据添加到文件中去。

然后我们把这个文件夹拷贝到 /System/Library/Displays/Contents/Resources/Overrides/ 中去

#### 使用RDM进行切换

重启系统打开RDM，这就可以进行切换了。