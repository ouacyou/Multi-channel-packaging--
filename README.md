# Multi-channel-packaging--
批量打包,sometimes不仅仅需要动态修改渠道号,maybe需要修改固定的变量值in java.

###如何通过ant打包,动态修改代码？

  ANT打包的方式可以参考其他资料，这里不说了；如果想修改java代码里面的代码，不太可能，目前的ant的打包是修改AndroidManifest.xml,
  so 我们可以在xml文件中声明一个meta-data属性值，在代码中通过get value值获取他的值，而这个值需要在打包的时候动态去改变这样
  目的就达到了。
  
  **in AndroidManifest.xml**
  ```xml
  <meta-data
            android:name="YOUR KEY"
            android:value="NEED VALUE" />
  ```
  **in java**
  ```Java
    ApplicationInfo ai;
		String courseId = null;
		try {
			ai = getPackageManager().getApplicationInfo(getPackageName(),
					PackageManager.GET_META_DATA);
			courseId = ai.metaData.getString("YOUR KEY");
		} catch (NameNotFoundException e) {
			e.printStackTrace();
		}
		System.out.println("yac-value" + courseId);//这是你需要动态获取的值
  ```
[关于在Manifest中meta-data扩展元素数据的配置与获取,click this](http://blog.csdn.net/janice0529/article/details/41583587)

finally you need 通过正则表达式去动态修改value<br>
**in config.xml**
```xml
<propertyregex
            input="${value}"
            override="true"
            property="NEED VALUE"
            regexp=":A(.*)"
            select="\1" />
  <replaceregexp
            byline="true"
            encoding="utf-8"
            file="AndroidManifest.xml"
            match="android:value=&quot;NEED VALUE&quot;"
            replace="android:value=&quot;${NEED VALUE}&quot;" />
```
