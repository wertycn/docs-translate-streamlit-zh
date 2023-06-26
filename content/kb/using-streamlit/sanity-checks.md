---
title：健全性检查
slug：/knowledge-base/using-streamlit/sanity-checks
---

# 健全性检查

如果您在运行Streamlit应用时遇到问题，请尝试以下几个方法。

## 检查＃0：您是否正在使用Streamlit支持的Python版本？

Streamlit将在实际可行的情况下与较早的Python版本保持向后兼容性，
保证与至少最近的三个Python 3次要版本兼容。

随着Python的新版本发布，我们将尽快与新版本兼容，尽管我们经常受到其他Python包的支持来支持这些新版本。

Streamlit目前支持Python的版本3.7、3.8、3.9、3.10和3.11。

## 检查1：Streamlit是否正在运行？

在Mac或Linux机器上，在终端上输入以下命令：

```bash
ps -Al | grep streamlit
```

如果在输出中看不到 `streamlit run`（或者如果您运行的是 `streamlit hello`），
则表示 Streamlit 服务器未运行。因此，请重新运行命令并查看错误是否消失。

## 检查 #2: 这是一个已经修复的 Streamlit bug 吗？

我们会尽快修复 bug，所以很多时候问题会在您升级 Streamlit 到最新版本后解决。
因此，当遇到问题时，请尝试升级到最新版本的 Streamlit：

```bash
pip install --upgrade streamlit
streamlit version
```

...然后验证打印的版本号是否与[PyPI](https://pypi.org/project/streamlit/)上显示的版本号相对应。

**现在尝试复现问题。** 如果问题没有解决，请继续阅读。

## 检查 #3: 是否正在运行正确的 Streamlit 二进制文件?

让我们检查一下您的Python环境是否设置正确。编辑您遇到问题的Streamlit脚本，**将所有内容注释掉，并添加以下这些行:**

```python
import streamlit as st
st.write(st.__version__)
```

...然后在您的脚本上调用`streamlit run`，并确保它与上面的版本相同。如果版本不同，请查看[这些说明](/library/get-started/installation)以了解一些设置环境的绝对方法。

## 检查 #4: 您的浏览器是否过于积极地缓存您的应用程序？

有两种简单的方法来检查：

1. 在浏览器中加载您的应用程序，然后按下`Ctrl-Shift-R`或`⌘-Shift-R`进行硬刷新（Chrome/Firefox）。

2. 作为测试，可以在另一个端口上运行Streamlit。这样，浏览器会以全新的缓存启动页面。为此，在命令行上向Streamlit传递`--server.port`参数：

   ```bash
   streamlit run my_app.py --server.port=9876
   ```

## 检查 #5：这是Streamlit的回归问题吗？

如果您已经升级到最新版本的Streamlit，但是出现了问题，您可以随时使用以下命令降级：

```bash
pip install --upgrade streamlit==1.0.0
```

...其中 `1.0.0` 是您想要降级到的版本。请参阅[变更日志](/library/changelog)以获取完整的Streamlit版本列表。

## 检查 #6 [Windows]: Python是否已添加到您的PATH环境变量中？

当通过从[python.org](https://www.python.org/downloads/)下载安装Python时，Python不会自动添加到[Windows系统的PATH](https://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access)中。因此，您可能会收到以下错误消息：

命令提示符:

```bash
C:\Users\streamlit> streamlit hello
'streamlit' is not recognized as an internal or external command,
operable program or batch file.
```

PowerShell:

```bash
PS C:\Users\streamlit> streamlit hello
streamlit : The term 'streamlit' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that
the path is correct and try again.
At line:1 char:1
+ streamlit hello
+ ~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (streamlit:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

要解决此问题，请将[Python添加到Windows系统的PATH](https://datatofish.com/add-python-to-windows-path/)中。

在将Python添加到Windows的PATH后，您应该能够按照我们的[入门指南](/library/get-started)中的说明进行操作。

## 检查 #7 [Windows]：您是否需要安装Visual Studio的构建工具？

Streamlit将[pyarrow](https://arrow.apache.org/docs/python/)作为安装依赖项。偶尔，在尝试从PyPI安装Streamlit时，您可能会遇到以下错误：

```bash
Using cached pyarrow-1.0.1.tar.gz (1.3 MB)
  Installing build dependencies ... error
  ERROR: Command errored out with exit status 1:
   command: 'c:\users\streamlit\appdata\local\programs\python\python38-32\python.exe' 'c:\users\streamlit\appdata\local\programs\python\python38-32\lib\site-packages\pip' install --ignore-installed --no-user --prefix 'C:\Users\streamlit\AppData\Local\Temp\pip-build-env-s7owjrle\overlay' --no-warn-script-location --no-binary :none: --only-binary :none: -i https://pypi.org/simple -- 'cython >= 0.29' 'numpy==1.14.5; python_version<'"'"'3.7'"'"'' 'numpy==1.16.0; python_version>='"'"'3.7'"'"'' setuptools setuptools_scm wheel
       cwd: None

  Complete output (319 lines):

      Running setup.py install for numpy: finished with status 'error'
      ERROR: Command errored out with exit status 1:
       command: 'c:\users\streamlit\appdata\local\programs\python\python38-32\python.exe' -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'C:\\Users\\streamlit\\AppData\\Local\\Temp\\pip-install-0jwfwx_u\\numpy\\setup.py'"'"'; __file__='"'"'C:\\Users\\streamlit\\AppData\\Local\\Temp\\pip-install-0jwfwx_u\\numpy\\setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record 'C:\Users\streamlit\AppData\Local\Temp\pip-record-eys4l2gc\install-record.txt' --single-version-externally-managed --prefix 'C:\Users\streamlit\AppData\Local\Temp\pip-build-env-s7owjrle\overlay' --compile --install-headers 'C:\Users\streamlit\AppData\Local\Temp\pip-build-env-s7owjrle\overlay\Include\numpy'
           cwd: C:\Users\streamlit\AppData\Local\Temp\pip-install-0jwfwx_u\numpy\
      Complete output (298 lines):

      blas_opt_info:
      blas_mkl_info:
      No module named 'numpy.distutils._msvccompiler' in numpy.distutils; trying from distutils
      customize MSVCCompiler
        libraries mkl_rt not found in ['c:\\users\\streamlit\\appdata\\local\\programs\\python\\python38-32\\lib', 'C:\\', 'c:\\users\\streamlit\\appdata\\local\\programs\\python\\python38-32\\libs']
        NOT AVAILABLE

      blis_info:
      No module named 'numpy.distutils._msvccompiler' in numpy.distutils; trying from distutils
      customize MSVCCompiler
        libraries blis not found in ['c:\\users\\streamlit\\appdata\\local\\programs\\python\\python38-32\\lib', 'C:\\', 'c:\\users\\streamlit\\appdata\\local\\programs\\python\\python38-32\\libs']
        NOT AVAILABLE

      # <truncated for brevity> #

      c:\users\streamlit\appdata\local\programs\python\python38-32\lib\distutils\dist.py:274: UserWarning: Unknown distribution option: 'define_macros'
        warnings.warn(msg)
      running install
      running build
      running config_cc
      unifing config_cc, config, build_clib, build_ext, build commands --compiler options
      running config_fc
      unifing config_fc, config, build_clib, build_ext, build commands --fcompiler options
      running build_src
      build_src
      building py_modules sources
      creating build
      creating build\src.win32-3.8
      creating build\src.win32-3.8\numpy
      creating build\src.win32-3.8\numpy\distutils
      building library "npymath" sources
      No module named 'numpy.distutils._msvccompiler' in numpy.distutils; trying from distutils
      error: Microsoft Visual C++ 14.0 is required. Get it with "Build Tools for Visual Studio": https://visualstudio.microsoft.com/downloads/
      ----------------------------------------
  ERROR: Command errored out with exit status 1: 'c:\users\streamlit\appdata\local\programs\python\python38-32\python.exe' -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'C:\\Users\\streamlit\\AppData\\Local\\Temp\\pip-install-0jwfwx_u\\numpy\\setup.py'"'"'; __file__='"'"'C:\\Users\\streamlit\\AppData\\Local\\Temp\\pip-install-0jwfwx_u\\numpy\\setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record 'C:\Users\streamlit\AppData\Local\Temp\pip-record-eys4l2gc\install-record.txt' --single-version-externally-managed --prefix 'C:\Users\streamlit\AppData\Local\Temp\pip-build-env-s7owjrle\overlay' --compile --install-headers 'C:\Users\streamlit\AppData\Local\Temp\pip-build-env-s7owjrle\overlay\Include\numpy' Check the logs for full command output.
  ----------------------------------------
```

这个错误表示在安装过程中，Python试图编译某些库，但是在您的系统上找不到适当的编译器，如 `error: 需要 Microsoft Visual C++ 14.0。使用“Visual Studio的构建工具”获取它。`

安装[Visual Studio的构建工具](https://visualstudio.microsoft.com/downloads/)应该可以解决这个问题。
