# note
笔记

使用插件化后,leakcanary内存泄露检测到的activity都是代理页面,编写以下脚本,当内存泄露的页面很多时,方便整理,分类

StringBuffer sb = new StringBuffer();
java.util.HashSet<String> hashset = new java.util.HashSet<>();
for (int i = 0; i < leaks.size(); i++) {
	DisplayLeakActivity.Leak l = (DisplayLeakActivity.Leak) leaks.get(i);

	List<com.squareup.leakcanary.LeakTraceElement> elements = l.result.leakTrace.elements;
	int length = elements.size();
	com.squareup.leakcanary.LeakTraceElement current = elements.get(length - 1);

	if (current.toString().indexOf("PluginContainerActivity") != -1) {
		current = elements.get(length - 2);
	}

	if (hashset.contains(current.toString())) {
		continue;
	}
	hashset.add(current.toString());
	sb.append(l.result.leakTrace.toString());
	sb.append("\r\n");
}
sb.toString();


http://www.jianshu.com/p/ade24dd8cba6
