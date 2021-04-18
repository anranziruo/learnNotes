#### import的引入
关于python的import引用的最大关键是init.py文件的作用，这个文件对于import的方法使用至关重要。
这个是我在搭建自动化框架过程中用到的import的方法使用。
比如说，我现在login.py想引用bottom底下的log.py的时候，这个时候，我们如何引用呢？
from bottom import log
又比如说，我现在想在test文件中引用login.py那这个时候如何引用？
from case.login import login
又比如说，我想在test.py中引用log.py这个时候怎么引用呢？
import bottom.log
这个只是对于import方法的最初学习。但是这个主要适用同级文件夹，如下图我运行demo文件夹底下demo.py，同时又引用bottom和casepy底下的文件，这个时候我们上边说的方法，只需要引入sys module就可以。
import sys
sys.path.append("../bottom")
sys.path.append("../")
sys.path.append("../case")
sys.path.append("../")
通过这个方法，就可以引用上述文件夹了