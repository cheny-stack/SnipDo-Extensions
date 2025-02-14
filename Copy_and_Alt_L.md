# "name": "CopyRead"
# "description": "Copy the selected text and paste it in the active window. Then press Alt+L to paste the text."

```powershell
# 清理文本，移除特殊字符和表情符号
$cleanText = $PLAIN_TEXT

# 移除表情符号和特殊 Unicode 字符
$ranges = @(
    # 移除表情符号和其他特殊符号，但保留中文和基本ASCII
    '[^\x00-\x7F\u4e00-\u9fa5\p{P}\s]',
    # 移除特定符号
    '[#@$%^&*+=\\|<>{}[\]]'
)

foreach ($range in $ranges) {
    $cleanText = $cleanText -replace $range, ''
}

# 将清理后的文本复制到剪贴板
Add-Type -AssemblyName System.Windows.Forms
[System.Windows.Forms.Clipboard]::SetText($cleanText)



# 等待一小段时间确保文本已复制
Start-Sleep -Milliseconds 100

# 获取文本的前5个字符
$first5Chars = $PLAIN_TEXT.Substring(0, [Math]::Min(5, $PLAIN_TEXT.Length))

# 检查是否包含中文字符（Unicode范围：0x4E00-0x9FFF）
$containsChinese = $false
foreach ($char in $first5Chars.ToCharArray()) {
    if ([int][char]$char -ge 0x4E00 -and [int][char]$char -le 0x9FFF) {
        $containsChinese = $true
        break
    }
}

# 根据是否包含中文发送不同的快捷键
Add-Type -AssemblyName System.Windows.Forms
if ($containsChinese) {
    [System.Windows.Forms.SendKeys]::SendWait("%j")
} else {
    [System.Windows.Forms.SendKeys]::SendWait("%k")
}

# 等待一小段时间
Start-Sleep -Milliseconds 100

# 发送 Alt+L
[System.Windows.Forms.SendKeys]::SendWait("%l")

# 输出成功消息
Write-Output "文本已复制，判断语言并发送相应快捷键"
```
