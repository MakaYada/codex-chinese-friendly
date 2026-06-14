# Codex Desktop 国内网络使用指南：接入 DeepSeek、开启中文界面和自动化任务

> Windows 纯国内网络实测流程：通过 CC Switch 把 Codex Desktop 接入 DeepSeek，再开启中文界面和自动化任务入口。

## 目录

- [适合谁](#适合谁)
- [最终效果](#最终效果)
- [重要提醒](#重要提醒)
- [1. 安装 Codex Desktop](#1-安装-codex-desktop)
- [2. 安装 CC Switch](#2-安装-cc-switch)
- [3. 配置 CC Switch](#3-配置-cc-switch)
- [4. 中文界面设置](#4-中文界面设置)
- [5. 开启自动化入口](#5-开启自动化入口)
- [6. 完整脚本](#6-完整脚本)
- [常见问题](#常见问题)
- [参考链接](#参考链接)

## 适合谁

- Windows 用户。
- 国内网络环境。
- 已经有 DeepSeek 或其他兼容 OpenAI 风格接口的 API Key。
- 想使用 Codex Desktop 做本地编程、项目修改、自动化任务。

## 最终效果

完成后你会得到：

- 一个可运行的 Codex Desktop。
- 一个通过 CC Switch 转发到 DeepSeek 的模型链路。
- 一个复制版 Codex，支持中文界面。
- 一个可选的自动化任务入口。

流程如下：

```text
Codex Desktop
    |
    | OpenAI / Responses 风格请求
    v
CC Switch 本地代理
    |
    | 转发到你配置的供应商
    v
DeepSeek / 其他兼容 API
```

## 重要提醒

- 本教程只修改复制出来的 Codex，不直接改 Microsoft Store 的官方安装目录。
- Codex 官方更新后，复制版需要重新运行补丁脚本。
- 如果你接入的模型不支持多模态，给 Codex 发图片可能会失败。

## 1. 安装 Codex Desktop

打开 Microsoft Store，搜索并安装 Codex。

<p align="center">
  <img src="images-github/01-codex-install-store.png" alt="微软商店安装 Codex" width="860">
</p>

安装完成后，可以在开始菜单看到 Codex 图标。

<p align="center">
  <img src="images-github/02-codex-taskbar-icon.png" alt="开始菜单 Codex 图标" width="860">
</p>

第一次打开会提示登录。这里先直接关闭，因为后面会通过 CC Switch 接入 DeepSeek。

<p align="center">
  <img src="images-github/03-codex-login-required.png" alt="首次打开提示登录" width="860">
</p>

右下角托盘也要退出干净，避免后续补丁脚本复制文件时被占用。

<p align="center">
  <img src="images-github/04-codex-tray-exit.png" alt="右下角退出 Codex" width="360">
</p>

## 2. 安装 CC Switch

打开 CC Switch 官网：

<https://www.ccswitch.io/zh/>

<p align="center">
  <img src="images-github/05-ccswitch-official-site.png" alt="CC Switch 官网" width="860">
</p>

点击下载后会跳转到 GitHub 仓库。

<p align="center">
  <img src="images-github/06-ccswitch-github-repo.png" alt="CC Switch GitHub 仓库" width="860">
</p>

在 Release 里找到 Windows 安装包，通常选择 `Windows.msi`。版本号不需要和截图一致，下载页面最新版本即可。

<p align="center">
  <img src="images-github/07-ccswitch-windows-installer.png" alt="找到 Windows 安装包" width="860">
</p>

安装时一路 `Next -> Next -> Install`。

## 3. 配置 CC Switch

打开 CC Switch 后，选择 `OpenAI`。

<p align="center">
  <img src="images-github/08-ccswitch-openai-provider-tab.png" alt="选择 OpenAI" width="860">
</p>

点击右上角加号。

<p align="center">
  <img src="images-github/09-ccswitch-add-provider.png" alt="点击加号" width="860">
</p>

供应商选择 `DeepSeek`。其他供应商思路类似。

<p align="center">
  <img src="images-github/10-ccswitch-select-deepseek.png" alt="选择 DeepSeek" width="860">
</p>

填入自己的 API Key，然后点击添加。

<p align="center">
  <img src="images-github/11-ccswitch-enter-api-key.png" alt="填写 API Key" width="860">
</p>

添加成功后，进入左上角设置。

<p align="center">
  <img src="images-github/12-ccswitch-provider-added.png" alt="供应商添加成功" width="860">
</p>

点击最上面的“路由”。

<p align="center">
  <img src="images-github/13-ccswitch-route-settings.png" alt="进入路由设置" width="860">
</p>

展开本地路由选项，打开：

- 在主页显示本地路由。
- 路由总开关。
- Codex 开关。

<p align="center">
  <img src="images-github/14-ccswitch-route-configured.png" alt="配置完成" width="860">
</p>

看到配置完成后，回到主页。

点击启用。注意：CC Switch 后续要保持运行，它是中间转发站。

<p align="center">
  <img src="images-github/15-ccswitch-enable-routing.png" alt="启用 CC Switch" width="860">
</p>

### 重新打开 Codex

重新从开始菜单打开 Codex，这时一般可以点击 `Skip` 跳过登录。

<p align="center">
  <img src="images-github/16-codex-skip-login.png" alt="跳过登录" width="860">
</p>

进入主界面后，可以看到模型已经切到 DeepSeek。

<p align="center">
  <img src="images-github/17-codex-deepseek-home.png" alt="进入主界面" width="860">
</p>

但此时可能还是英文，设置里切语言也不生效。

<p align="center">
  <img src="images-github/18-codex-language-switch-fails.png" alt="语言切换无效" width="860">
</p>

原因是 Codex 包里虽然有中文资源，但还需要打开本地开关。接下尝试开启中文界面。

## 4. 中文界面设置

这一步只做两件事：

1. 把官方 Codex 复制到用户目录。
2. 修改复制版，让中文界面可用。

先彻底关闭 Codex，然后以管理员身份打开 PowerShell。

<p align="center">
  <img src="images-github/19-powershell-run-as-admin.png" alt="管理员 PowerShell" width="720">
</p>

把下面脚本完整复制进去运行。

<details>
<summary>只开启中文界面的补丁脚本</summary>

```powershell
# 复制版 Codex 的保存目录。
# 这样不会直接修改 Microsoft Store 安装目录，后续官方更新也更容易恢复。
$root = "$env:LOCALAPPDATA\CodexPatched"

# 找到 Microsoft Store 安装的官方 Codex 包。
# 如果这里报错，说明 Codex 可能还没有安装成功。
$pkg = Get-AppxPackage OpenAI.Codex
$src = $pkg.InstallLocation
$version = $pkg.PackageFullName

# current 是实际运行的复制版目录。
# version.txt 用来记录上次复制的官方 Codex 版本。
$dst = Join-Path $root "current"
$marker = Join-Path $root "version.txt"

# 先关闭正在运行的 Codex，避免 app.asar 或其他文件被占用。
Get-Process Codex,codex -ErrorAction SilentlyContinue | Stop-Process -Force

# 确保复制版根目录存在。
New-Item -ItemType Directory -Path $root -Force | Out-Null

# 默认需要复制。
# 如果 current 已存在，并且 version.txt 记录的版本和当前官方版本一致，就不重复复制。
$needCopy = $true
if ((Test-Path $dst) -and (Test-Path $marker)) {
    $needCopy = ((Get-Content $marker -Raw).Trim() -ne $version)
}

# 如果官方 Codex 更新过，或者第一次运行脚本，就重新复制一份。
if ($needCopy) {
    if (Test-Path $dst) {
        Remove-Item $dst -Recurse -Force
    }

    # robocopy 比 Copy-Item 更适合复制整个应用目录。
    # /E 复制所有子目录，包括空目录。
    # /COPY:DAT 和 /DCOPY:DAT 保留常规文件/目录属性。
    # /R:2 /W:1 表示失败时最多重试 2 次，每次等待 1 秒。
    robocopy $src $dst /E /COPY:DAT /DCOPY:DAT /R:2 /W:1 /NFL /NDL /NJH /NJS | Out-Null
    if ($LASTEXITCODE -gt 7) {
        throw "Copy failed. Robocopy exit code: $LASTEXITCODE"
    }

    # 记录这次复制的官方版本号。下次运行时可以判断是否需要重新复制。
    Set-Content -Path $marker -Value $version
}

# Codex 前端资源包。中文界面开关就在这个文件里。
$asar = Join-Path $dst "app\resources\app.asar"
$backup = "$asar.bak"

# 第一次修改前备份 app.asar。
# 如果后续想手动恢复，可以用这个 .bak 文件覆盖回去。
if (!(Test-Path $backup)) {
    Copy-Item $asar $backup -Force
}

# 以字节方式读取 app.asar。
# 这里用 28591 单字节编码把字节映射成字符串，方便按字符串查找位置。
# 真正写回时仍然写回 byte[]，避免因为编码转换破坏文件。
$bytes = [System.IO.File]::ReadAllBytes($asar)
$enc = [System.Text.Encoding]::GetEncoding(28591)
$text = $enc.GetString($bytes)
$changed = $false

# 在 app.asar 里做“等长 ASCII 替换”。
# 重点：Old 和 New 必须一样长。
# 这样只改原位置的字节，不改变文件整体长度，降低破坏 asar 结构的风险。
function Patch-AsciiSameLength {
    param(
        [byte[]] $Bytes,
        [string] $Text,
        [string] $Old,
        [string] $New
    )

    if ($Old.Length -ne $New.Length) {
        throw "Pattern length mismatch: [$Old] -> [$New]"
    }

    $count = 0
    $start = 0

    # 从头到尾查找旧字符串。
    # 找到一次，就把同位置的字节替换成新字符串的 ASCII 字节。
    while (($idx = $Text.IndexOf($Old, $start, [System.StringComparison]::Ordinal)) -ge 0) {
        $newBytes = [System.Text.Encoding]::ASCII.GetBytes($New)
        [Array]::Copy($newBytes, 0, $Bytes, $idx, $newBytes.Length)
        $count++
        $start = $idx + $Old.Length
    }

    return $count
}

# 开启中文界面。
# Codex 包里本来有中文资源，但部分版本默认关闭 i18n。
# 这里把 enable_i18n 的 false 写法改成 true 写法。
$c = Patch-AsciiSameLength $bytes $text 'enable_i18n`,!1' 'enable_i18n`,!0'
if ($c -gt 0) {
    Write-Host "Patched i18n: $c"
    $changed = $true
} else {
    Write-Host "i18n old pattern not found; may already be patched."
}

# 如果补丁点命中，就把修改后的字节写回 app.asar。
# 如果没有命中，说明可能已经补过，或者 Codex 新版本内部字符串变了。
if ($changed) {
    [System.IO.File]::WriteAllBytes($asar, $bytes)
    Write-Host "Patch written: $asar"
} else {
    Write-Host "No changes written."
}

# 启动复制版 Codex。
# 注意：以后建议固定并打开这个复制版，而不是开始菜单里的官方版。
Start-Process -FilePath (Join-Path $dst "app\Codex.exe") -WorkingDirectory (Join-Path $dst "app")

```

</details>

脚本结束后会自动打开复制版 Codex。成功后界面会显示中文。

<p align="center">
  <img src="images-github/20-codex-chinese-ui.png" alt="复制版中文界面" width="860">
</p>

### 测试 Codex 并固定复制版

输入一个简单任务：

```text
完成一个画风可爱的 HTML 版贪吃蛇小游戏
```

如果能开始执行，说明链路已经通了。

<p align="center">
  <img src="images-github/21-codex-task-running.png" alt="开始运行任务" width="860">
</p>

执行完成后会生成下面这个“可爱”的贪吃蛇小游戏。

<p align="center">
  <img src="images-github/22-codex-snake-game-result.png" alt="小游戏结果" width="560">
</p>


建议把复制版 Codex 固定到任务栏，以后直接打开复制版。

<p align="center">
  <img src="images-github/23-codex-pin-patched-app.png" alt="固定到任务栏" width="700">
</p>

此时如果从开始菜单打开 Codex，通常还是官方版；从任务栏打开你固定的复制版，才是中文版本。官方版可以用来接收更新，更新后再重新运行补丁脚本即可。

## 5. 开启自动化入口

现在复制版已经能正常使用中文。接下来作为进阶功能，单独开启左侧的“自动化”入口。

先关闭复制版 Codex，然后在管理员 PowerShell 中运行下面脚本。

<details>
<summary>只开启自动化入口的补丁脚本</summary>

```powershell
# 这里直接定位前面已经复制好的 Codex。
$dst = "$env:LOCALAPPDATA\CodexPatched\current"
$asar = Join-Path $dst "app\resources\app.asar"
$backup = "$asar.bak"

# 关闭正在运行的 Codex，避免 app.asar 被占用。
Get-Process Codex,codex -ErrorAction SilentlyContinue | Stop-Process -Force

# 如果没有找到复制版，说明需要先运行“中文界面补丁脚本”。
if (!(Test-Path $asar)) {
    throw "Patched Codex not found: $asar"
}

# 如果之前没有备份，这里补一个备份。
if (!(Test-Path $backup)) {
    Copy-Item $asar $backup -Force
}

# 读取 app.asar。
$bytes = [System.IO.File]::ReadAllBytes($asar)
$enc = [System.Text.Encoding]::GetEncoding(28591)
$text = $enc.GetString($bytes)

# 这个 feature gate 控制左侧 Automations 入口是否显示。
# 新旧字符串长度必须一致，避免破坏 asar 文件结构。
$old = 'So(`3075919032`)'
$new = '(()=>!0)()      '

if ($old.Length -ne $new.Length) {
    throw "Pattern length mismatch."
}

$count = 0
$start = 0

while (($idx = $text.IndexOf($old, $start, [System.StringComparison]::Ordinal)) -ge 0) {
    $newBytes = [System.Text.Encoding]::ASCII.GetBytes($new)
    [Array]::Copy($newBytes, 0, $bytes, $idx, $newBytes.Length)
    $count++
    $start = $idx + $old.Length
}

if ($count -gt 0) {
    [System.IO.File]::WriteAllBytes($asar, $bytes)
    Write-Host "Patched automations sidebar gate: $count"
} else {
    Write-Host "Automations pattern not found. It may already be patched, or this Codex version changed."
}

# 启动复制版 Codex。
Start-Process -FilePath (Join-Path $dst "app\Codex.exe") -WorkingDirectory (Join-Path $dst "app")
```

</details>

等待复制版 Codex 打开后，左侧会出现“自动化”入口。

<p align="center">
  <img src="images-github/24-codex-automation-sidebar.png" alt="自动化入口" width="860">
</p>

点击自动化，可以通过聊天创建，也可以手动创建。

<p align="center">
  <img src="images-github/25-codex-automation-create-options.png" alt="创建自动化" width="860">
</p>

### 手动创建自动化任务

先输入任务名称和任务内容。

<p align="center">
  <img src="images-github/26-codex-automation-task-input.png" alt="输入任务名称和任务" width="860">
</p>

选择项目，也就是运行任务时的文件夹。

<p align="center">
  <img src="images-github/27-codex-automation-select-project.png" alt="选择项目" width="860">
</p>

设置运行时间和频率。

<p align="center">
  <img src="images-github/28-codex-automation-schedule.png" alt="设置运行频率" width="860">
</p>

选择任务使用的模型。

<p align="center">
  <img src="images-github/29-codex-automation-select-model.png" alt="选择模型" width="860">
</p>

选择思维链长度。这个选项是否有明显效果，取决于当前版本和模型支持情况。

<p align="center">
  <img src="images-github/30-codex-automation-reasoning-effort.png" alt="选择思维链长度" width="860">
</p>

创建完成后，任务会出现在自动化列表。

<p align="center">
  <img src="images-github/31-codex-automation-list.png" alt="自动化任务列表" width="860">
</p>

点进任务可以查看详情、下次运行时间和设置。

<p align="center">
  <img src="images-github/32-codex-automation-detail.png" alt="自动化任务详情" width="860">
</p>

任务执行后会生成结果。比如这里，奶龙已经走向国际。

<p align="center">
  <img src="images-github/33-codex-automation-nailong-result.png" alt="自动化任务结果" width="860">
</p>

### 通过聊天创建自动化任务

也可以在新对话里直接输入提示词。

<p align="center">
  <img src="images-github/34-codex-chat-automation-prompt.png" alt="新对话输入提示词" width="860">
</p>

推荐提示词：

```text
我要创建一个 Codex 自动化任务。

重要要求：
1. 必须使用 Codex 的 automation_update 工具创建自动化。
2. 不要手写 automation.toml。
3. 如果 automation_update 工具不可用，请明确告诉我“工具不可用”，不要假装已经创建成功。
4. 模型必须使用 deepseek-v4-flash，不要使用默认模型，不要使用 o4-mini、gpt、codex 等模型。
5. executionEnvironment 必须是 local。
6. reasoningEffort 使用 medium。
7. status 使用 ACTIVE。
8. rrule 使用标准格式，不要加 RRULE: 前缀。

请创建这个自动化任务：

每隔一小时给我收集猪猪侠的全部信息，汇总成 HTML 发给我。
```

创建完成后，列表里会出现新任务。

<p align="center">
  <img src="images-github/35-codex-chat-automation-created.png" alt="聊天创建自动化成功" width="860">
</p>

任务执行后会产出结果。

<p align="center">
  <img src="images-github/36-codex-zhuzhuxia-result.png" alt="猪猪侠结果" width="860">
</p>

## 6. 完整脚本

前面按实际操作流程拆成了两段脚本：先中文，后自动化。  
也可以直接使用下面这个完整脚本，一次复制 Codex，并同时开启中文界面和自动化入口。

<details>
<summary>中文版 + 自动化入口完整脚本</summary>

```powershell
# 复制版 Codex 的保存目录。
$root = "$env:LOCALAPPDATA\CodexPatched"

# 找到 Microsoft Store 安装的官方 Codex 包。
$pkg = Get-AppxPackage OpenAI.Codex
$src = $pkg.InstallLocation
$version = $pkg.PackageFullName

# current 是实际运行的复制版目录。
$dst = Join-Path $root "current"
$marker = Join-Path $root "version.txt"

# 关闭正在运行的 Codex，避免文件占用。
Get-Process Codex,codex -ErrorAction SilentlyContinue | Stop-Process -Force

New-Item -ItemType Directory -Path $root -Force | Out-Null

# 如果官方版本变化，或者第一次运行，就重新复制。
$needCopy = $true
if ((Test-Path $dst) -and (Test-Path $marker)) {
    $needCopy = ((Get-Content $marker -Raw).Trim() -ne $version)
}

if ($needCopy) {
    if (Test-Path $dst) {
        Remove-Item $dst -Recurse -Force
    }

    robocopy $src $dst /E /COPY:DAT /DCOPY:DAT /R:2 /W:1 /NFL /NDL /NJH /NJS | Out-Null
    if ($LASTEXITCODE -gt 7) {
        throw "Copy failed. Robocopy exit code: $LASTEXITCODE"
    }

    Set-Content -Path $marker -Value $version
}

$asar = Join-Path $dst "app\resources\app.asar"
$backup = "$asar.bak"

if (!(Test-Path $backup)) {
    Copy-Item $asar $backup -Force
}

$bytes = [System.IO.File]::ReadAllBytes($asar)
$enc = [System.Text.Encoding]::GetEncoding(28591)
$text = $enc.GetString($bytes)
$changed = $false

function Patch-AsciiSameLength {
    param(
        [byte[]] $Bytes,
        [string] $Text,
        [string] $Old,
        [string] $New
    )

    if ($Old.Length -ne $New.Length) {
        throw "Pattern length mismatch: [$Old] -> [$New]"
    }

    $count = 0
    $start = 0

    while (($idx = $Text.IndexOf($Old, $start, [System.StringComparison]::Ordinal)) -ge 0) {
        $newBytes = [System.Text.Encoding]::ASCII.GetBytes($New)
        [Array]::Copy($newBytes, 0, $Bytes, $idx, $newBytes.Length)
        $count++
        $start = $idx + $Old.Length
    }

    return $count
}

# 开启中文界面。
$c = Patch-AsciiSameLength $bytes $text 'enable_i18n`,!1' 'enable_i18n`,!0'
if ($c -gt 0) {
    Write-Host "Patched i18n: $c"
    $changed = $true
} else {
    Write-Host "i18n old pattern not found; may already be patched."
}

# 开启自动化侧边栏入口。
$c = Patch-AsciiSameLength $bytes $text 'So(`3075919032`)' '(()=>!0)()      '
if ($c -gt 0) {
    Write-Host "Patched automations sidebar gate: $c"
    $changed = $true
} else {
    Write-Host "Automations sidebar gate pattern not found; may already be patched or version changed."
}

if ($changed) {
    [System.IO.File]::WriteAllBytes($asar, $bytes)
    Write-Host "Patch written: $asar"
} else {
    Write-Host "No changes written."
}

Start-Process -FilePath (Join-Path $dst "app\Codex.exe") -WorkingDirectory (Join-Path $dst "app")
```

</details>

## 常见问题

### 为什么打开后还是英文？

大概率打开的是官方版，不是复制版。复制版路径是：

```text
%LOCALAPPDATA%\CodexPatched\current\app\Codex.exe
```

### 为什么脚本提示 pattern not found？

可能已经打过补丁，也可能 Codex 新版本改了内部代码。先重新运行对应脚本；如果还是不行，就需要重新定位新版本开关字符串。

### 为什么任务跑不动？

优先检查：

1. CC Switch 是否还在运行。
2. CC Switch 是否点击了启用。
3. DeepSeek API Key 是否有效。
4. 模型名是否和 CC Switch 中一致。
5. 本地路由里的 Codex 开关是否开启。

### 问 Codex 是什么模型，它说自己是 Codex / GPT-5，正常吗？

正常。这常常来自 Codex 的系统提示词，不代表实际请求一定走 OpenAI。真实请求走哪里，看 CC Switch 请求记录更靠谱。

## 参考链接

- Codex Windows 官方页面：<https://developers.openai.com/codex/app/windows>
- Codex App 官方文档：<https://developers.openai.com/codex/app>
- CC Switch 官网：<https://www.ccswitch.io/zh/>
- CC Switch GitHub：<https://github.com/farion1231/cc-switch>


