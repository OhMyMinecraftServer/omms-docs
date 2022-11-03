# 插件编写

**首先来说，OMMS Central是支持添加插件的，所有插件均使用Groovy语言编写**  
**每一个插件均有且仅有一个继承了`PluginMain`类的Groovy类.**  
**每一个插件都要求文件名与插件主类名相同,就像java类一样。**  
**这是一个简单插件的示例：**  

```groovy
import net.zhuruoling.plugin.LifecycleServerInterface
import net.zhuruoling.plugin.PluginLogger
import net.zhuruoling.plugin.PluginMain
import net.zhuruoling.plugin.PluginMetadata

import java.lang.module.ModuleDescriptor

class MyPlugin extends PluginMain {
    PluginLogger logger = null

    @Override
    void onLoad(LifecycleServerInterface serverInterface) {
        logger = serverInterface.getLogger()
        logger.info("Plugin loaded!")
    }

    @Override
    void onUnload(LifecycleServerInterface serverInterface) {
        logger.info("Plugin loaded!")
    }

    @Override
    PluginMetadata getPluginMetadata() {
        return new PluginMetadata("my_plugin", ModuleDescriptor.Version.parse("0.0.1"), "ZhuRuoLing")
    }
}
```

可以看到`MyPlugin`类实现了插件抽象类`PluginMain`中的三个方法, 缺少任意一个方法都会导致该插件加载失败。  
>这个类（`MyPlugin`）中的三个方法分别的作用解释如下：  
> onLoad方法是插件在加载时会执行的第二个方法， 可以在这个方法中注册控制台指令，自定义Socket请求响应等等。  
> onUnload方法是插件在卸载时执行的方法，这个方法中可以做一些清理收尾工作。  
> getPluginMetadata是插件在加载时执行的第一个方法，OMMS Central从中取得插件的元数据信息，包括插件版本，id，名称，插件依赖等等。


