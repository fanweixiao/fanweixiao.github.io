---
layout: post
category : mac
tags : [mac, ime, shortcut, KeyRemap4MacBook]
---
{% include JB/setup %}

在Mac上很多IDE都是用Control+Space作爲了默認快捷鍵, 而Command+Space也被Spotlight搶佔了. 所以輸入法切換我就習慣用Option+Space了.

如果使用程序員神器[KeyRemap4MacBook](http://pqrs.org/macosx/keyremap4macbook/)的話,可以將Option_R設置成Option+Space的快捷鍵:

	<item>
        <name>Switch Input Source</name>
        <appendix>Use the right Option key to select the next input source</appendix>
        <identifier>private.switch_input_source_with_right_option</identifier>
        <autogen>--KeyToKey-- KeyCode::OPTION_R, KeyCode::SPACE, ModifierFlag::OPTION_L</autogen>
    </item>

之所以說它是程序員神器,估計MacVim的用戶最明白:key repeat一定要設置成350/23嘛~