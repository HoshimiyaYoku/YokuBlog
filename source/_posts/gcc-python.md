---
title: 'pythonによる.cファイルの自動コンバイル'
date: 2023-07-06 11:39:00
tag: python
category: code

---
#　pythonによる.cファイルの自動コンバイル
---

学校で副手の仕事をやっていますので、学生の課題を採点することも副手の仕事です。
しかし、学校のシステムからダウンロードした課題ファイルを一つ一つでgccコマンドでコンバイルするのは手間掛かるので、pythonで自動的にコンバイルするプログラムを書きました。

---
##code

```pthnon
import os

# 递归遍历目录树
def traverse(path):
    # 获取当前目录下的所有文件和子目录
    files = os.listdir(path)
    for file in files:
        # 拼接路径
        file_path = os.path.join(path, file)
        # 如果是子目录，递归处理子目录
        if os.path.isdir(file_path):
            traverse(file_path)
        # 如果是以 .c 结尾的文件，执行编译操作
        elif file.endswith('.c'):
            # 获取文件名（不包括扩展名）
            file_name = os.path.splitext(file)[0]
            # 获取源文件相对于当前目录的路径
            src_path = os.path.relpath(file_path, '.')
            # 获取输出文件的路径
            out_path = os.path.join(path, file_name + '.out')
            # 执行编译操作
            os.system(f'gcc {src_path} -o {out_path}')

# 开始遍历当前目录及其子目录
traverse('.')
```
簡単なプログラムですけど、採点の速度は上がりました。ただし、このプログラムはスペースなどの符号を使用したフォルダに対応できません。
元々、gccでコンバイルした.outファイルを自動的実行するプログラムも作りたいですが、出力したファイルのpathはプログラムと同じpathになるので、採点できなくなりました（出力したファイルはほどんど学籍番号が書いていません）。

---
採点するとき、学生さんはどうの部分が間違いしやすいをわかってきて、自分にも勉強になります。たまに、面白いミスもありました。でも、コピペする人もいました。学籍番号も書き替えずに、そのまま他人の学籍番号までコッピするのは悲しいことです。
