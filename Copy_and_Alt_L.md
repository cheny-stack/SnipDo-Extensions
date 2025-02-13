# "name": "Copy and Alt+L"
# "description": "Copy the selected text and paste it in the active window. Then press Alt+L to paste the text."

```powershell
param(
    [string] $PLAIN_TEXT
)

# 将文本复制到剪贴板
Add-Type -AssemblyName System.Windows.Forms
[System.Windows.Forms.Clipboard]::SetText($PLAIN_TEXT)

# 等待一小段时间确保文本已复制
Start-Sleep -Milliseconds 100

# 模拟按下 Alt+L
Add-Type -AssemblyName System.Windows.Forms
[System.Windows.Forms.SendKeys]::SendWait("%l")

# 输出成功消息
Write-Output "文本已复制并按下Alt+L"
```