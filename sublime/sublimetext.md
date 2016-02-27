# sublime text

## Package Control(被用来安装插件的) 安装
`Ctrl`+`~` 输入对应版本代码，稍等一会儿就安装成功了，重启，就可以了

3

`import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)`

2

`import urllib2,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')`

> [more..](https://packagecontrol.io/packages/Package%20Control)

## 常用插件 [more..](https://packagecontrol.io/browse)
- [Emmet](https://packagecontrol.io/packages/Emmet) 快速编写html，css的神器
- [HTML-CSS-JS Prettify](https://packagecontrol.io/packages/HTML-CSS-JS%20Prettify) 格式化HTML, CSS, JavaScript和JSON 
- [Side​Bar​Enhancements](https://packagecontrol.io/packages/SideBarEnhancements) 侧边栏工具
- [Bracket​Highlighter](https://packagecontrol.io/packages/BracketHighlighter) 用于匹配括号，引号和html标签

![Bracket​Highlighter](https://packagecontrol.io/readmes/img/2c23129492d6d74b8f9139711578e9ad0d1115a0.png)
- [DocBlockr](https://packagecontrol.io/packages/DocBlockr) 可以自动生成PHPDoc风格的注释

![DocBlockr](https://packagecontrol.io/readmes/img/deacf9e19c8eaaaaffe0a8cc2f4f3a15b9baf6b7.gif)
- [Color​Picker](https://packagecontrol.io/packages/ColorPicker) 颜色选择插件
- [Terminal](https://packagecontrol.io/packages/Terminal) 使用终端打开文件
- [Markdown Preview](https://packagecontrol.io/packages/Markdown%20Preview) markdown预览
## 授权码
```
—– BEGIN LICENSE —–
Ryan Clark
Single User License
EA7E-812479
2158A7DE B690A7A3 8EC04710 006A5EEB
34E77CA3 9C82C81F 0DB6371B 79704E6F
93F36655 B031503A 03257CCC 01B20F60
D304FA8D B1B4F0AF 8A76C7BA 0FA94D55
56D46BCE 5237A341 CD837F30 4D60772D
349B1179 A996F826 90CDB73C 24D41245
FD032C30 AD5E7241 4EAA66ED 167D91FB
55896B16 EA125C81 F550AF6B A6820916
—— END LICENSE ——
```
> [more..](http://appnee.com/sublime-text-3-universal-license-keys-collection-for-win-mac-linux)

[官网](http://www.sublimetext.com/) [论坛](http://sublimetext.iaixue.com/forum.php)