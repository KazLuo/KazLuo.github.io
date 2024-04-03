---
layout: article
title: Gem::FilePermissionError ＆ Bundler::GemNotFound
mathjax: true
tags: [SSL,.NET,Azure]
---

### 前言

&emsp;&emsp;因為近期開始Windows轉用Mac的關係，陸陸續續的將一些專案都移動到新電腦，久久之後發現我竟然忘了我還有一個部落格（窘
雖說Mac本身就有Ruby，不過版本好像與我的部落格框架不相容於是出現以下種種...


```csharp
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

### **解決 Ruby 和 gem 安裝問題**

### 常見問題

1. **權限問題**：無法在系統目錄中安裝 gem。
2. **PATH 環境變量問題**：gem 安裝後的執行檔不在 PATH 中。
3. **Ruby 版本問題**：安裝的 gems 需要比系統提供的 Ruby 版本更新的版本。

### 解決步驟

1. **解決權限問題**
    - 使用 **`sudo`** 命令安裝 gem（不推薦）。
    - 或者更好的做法，安裝和使用 Ruby 版本管理器（如 **`rbenv`** 或 **`RVM`**）。
2. **解決 PATH 問題**
    - 確保 **`~/.gem/ruby/[version]/bin`** 路徑被添加到您的 PATH 環境變量中。
3. **解決 Ruby 版本問題**
    - 使用 **`rbenv`** 或 **`RVM`** 升級到所需的 Ruby 版本。

### 以 **`rbenv`** 在 macOS 上安裝和設置 Ruby 的具體步驟
&emsp;&emsp;通常在這個步驟我們只能一步一步驗證，將一些本來還有安裝的東西都先確認過一次
1. **安裝 Homebrew**（如果尚未安裝）

    ```

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

    ```

2. **安裝 `rbenv` 和 `ruby-build`**

    ```

    brew install rbenv ruby-build

    ```

3. **初始化 `rbenv` 並添加到 shell 配置中**

    ```

    echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.zshrc
    source ~/.zshrc

    ```

4. **安裝 Ruby**
    - 查找可用的最新 Ruby 版本：

        ```
        rbenv install -l
        ```

    - 安裝最新版本的 Ruby（例如 3.0.0）：

        ```
        rbenv install 3.0.0
        rbenv global 3.0.0
        ```

5. **檢查 Ruby 版本**

    ```
    ruby -v
    ```

6. **如果安裝後 Ruby 版本沒有更新**
    - 確保 **`rbenv`** 初始化命令在您的 shell 配置檔案中（**`.zshrc`** 或 **`.bash_profile`**）。
    - 重新載入配置檔案或重新啟動 Terminal。
    - 使用 **`rbenv global [version]`** 確定正確的 Ruby 版本被設置。


喔！完成了嗎？沒有...出了新問題

## (Bundler::GemNotFound)

```csharp
raise_not_found!': Could not find gem 'rake (~> 10.0)' in locally installed gems.
 (Bundler::GemNotFound)
```

錯誤訊息指出，當嘗試運行 Jekyll 時，Bundler 未能找到一個特定版本的 **`rake`** gem。雖然已安裝 **`rake-13.0.3`**，但是 Jekyll 或其依賴項似乎需要 **`rake`** 的 **`~> 10.0`** 版本。為了解決這個問題，我們可以嘗試安裝需要的 **`rake`** 版本。

請按照以下步驟操作：

1. **安裝所需版本的 `rake`**：

    Jekyll 似乎需要一個符合 **`~> 10.0`** 的 **`rake`** 版本。您可以嘗試安裝這個特定版本的 **`rake`**：

    ```

    gem install rake -v '~> 10.0'

    ```


2. **執行 `bundle install`**：

    在 Jekyll 網站目錄中，執行以下命令來安裝所有依賴：

    ```

    bundle install

    ```

3. **再次啟動 Jekyll**：

    使用以下命令嘗試再次啟動 Jekyll：

    ```

    bundle exec jekyll serve

    ```

