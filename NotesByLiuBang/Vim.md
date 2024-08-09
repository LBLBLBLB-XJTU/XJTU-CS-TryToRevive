# Vim

- [Vim](#vim)
    * [模式](#--)
    * [快捷键:](#----)
        + [normal](#normal)
        + [command-line](#command-line)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 模式
正常模式：在文件中四处移动光标进行修改  
插入模式：插入文本   
替换模式：替换文本   
可视化模式（一般，行，块）：选中文本块   
命令模式：用于执行命令
1. 返回正常模式：ESC
2. 进入插入模式：i
3. 进入替换模式: R
4. 进入可视化模式，一般: v，行: V，块: Ctrl-v
5. 进入命令模式: :

## 快捷键:
### normal
1. 基本移动: hjkl （左， 下， 上， 右）
2. 词： w （下一个词）， b （词初）， e （词尾）
3. 行： 0 （行初）， ^ （第一个非空格字符）， $ （行尾）
4. 屏幕： H （屏幕首行）， M （屏幕中间）， L （屏幕底部）
5. 翻页： Ctrl-u （上翻）， Ctrl-d （下翻）
6. 文件： gg （文件头）， G （文件尾）
7. O / o 在之上/之下插入行
8. x 删除字符（等同于 dl） 
9. u 撤销, <C-r> 重做 
10. y 复制
11. p 粘贴

### command-line
1. :q 退出（关闭窗口）
2. :w 保存（写）
3. :wq 保存然后退出
4. :e {文件名} 打开要编辑的文件
5. :ls 显示打开的缓存
6. :help {标题} 打开帮助文档
7. :vsp 垂直选项卡
8. :sp 水平选项卡

 