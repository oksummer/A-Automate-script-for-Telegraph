# Telegraph批量插入图床链接

众所周知，由于各位老哥的白嫖行为，小飞机的Telegraph不允许上传图片了，只能通过图床一个一个插入图片链接，太麻烦了。于是写了一个Power Automate脚本，方便做套图

https://github.com/user-attachments/assets/9fdfc990-7777-428d-a4ec-bd02725baeae

# 使用方法

### 1.新建流，勾选Power FX

### 2.复制下方代码 

### 3.在编辑页面右键粘贴即可


``` scss
由Telegram@smallzhang编写
WebAutomation.LaunchEdge.AttachToEdgeByUrl TabUrl: $fx'https://telegra.ph/' AttachTimeout: $fx'=2' TargetDesktop: $fx'{"DisplayName":"本地计算机","Route":{"ServerType":"Local","ServerAddress":""},"DesktopType":"local"}' BrowserInstance=> Browser
**REGION 测试第一张图片，并询问链接数量
MouseAndKeyboard.SendKeys.FocusAndSendKeysByInstanceOrHandle WindowInstance: $fx'=Browser' TextToSend: $fx'{Down}{Down}{Down}{Left}{Enter}' DelayBetweenKeystrokes: $fx'=3' SendTextAsHardwareKeys: False
Display.InputDialog Title: $fx'你要插入几张图片？' Message: $fx'请输入确切的图片链接数量！（行数）' DefaultValue: $fx'=0' InputType: Display.InputType.SingleLine IsTopMost: False UserInput=> 用户输入 ButtonPressed=> 选择
**ENDREGION
Text.ToNumber Text: $fx'=用户输入' Number=> 数值转换
Variables.DecreaseVariable Value: $fx'=数值转换' DecrementValue: $fx'=2'
LOOP 循环次数 FROM $fx'=循环次数' TO $fx'=数值转换' STEP $fx'=1'
    **REGION 设置图片
    # 可修改按键速度，速度过快可能会导致出错，建议3-5
    MouseAndKeyboard.SendKeys.FocusAndSendKeysByInstanceOrHandle WindowInstance: $fx'=Browser' TextToSend: $fx'{Down}{Down}{Left}{Enter}' DelayBetweenKeystrokes: $fx'=3' SendTextAsHardwareKeys: False
    **ENDREGION
END
Display.ShowMessageDialog.ShowMessage Title: $fx'插入完成' Message: $fx'等待图片全部加载完毕后，点击确定继续删除空白行' Icon: Display.Icon.None Buttons: Display.Buttons.OK DefaultButton: Display.DefaultButton.Button1 IsTopMost: False ButtonPressed=> 选择2
Variables.DecreaseVariable Value: $fx'=循环次数' DecrementValue: $fx'=数值转换'
Variables.IncreaseVariable Value: $fx'=数值转换' IncrementValue: $fx'=2'
LOOP 循环次数 FROM $fx'=循环次数' TO $fx'=数值转换' STEP $fx'=1'
    MouseAndKeyboard.SendKeys.FocusAndSendKeysByInstanceOrHandle WindowInstance: $fx'=Browser' TextToSend: $fx'{Back}{Back}{Up}' DelayBetweenKeystrokes: $fx'=3' SendTextAsHardwareKeys: False
END
Display.ShowMessageDialog.ShowMessage Title: $fx'完成' Message: $fx'🎉图片已经插入完毕啦！' Icon: Display.Icon.None Buttons: Display.Buttons.OK DefaultButton: Display.DefaultButton.Button1 IsTopMost: False ButtonPressed=> 选择2
EXIT Code: $fx'=0'
```

