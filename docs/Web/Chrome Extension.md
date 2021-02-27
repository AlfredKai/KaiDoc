# Chrome Extension

## Manifest

- 插件的設定，只要有這個檔案就能安裝
- permissions管理可存取的資源權限，ex: storage, activeTab
- background scripts, page_action(popup), icon, option_page都是跟他註冊

## Background Script

- 使用在`瀏覽器`層級的JS(不是在頁面層級)
- event based which are browser triggers
- such as navigating to a new page, removing a bookmark, or closing a tab. 
- The extension is first installed or updated to a new version.
- The background page was listening for an event, and the event is dispatched.
- A content script or other extension sends a message.
- Another view in the extension, such as a popup, calls runtime.getBackgroundPage.
- 在擴充功能頁面按`查看檢視模式背景頁面`可以檢查log

## UI element

- 擁有各種形式，最常用的是popup
- 其他形式如omnibox(search bar), context menu
- 可以設定全網頁啟用或特定網頁啟用
- 內嵌的popupjs可直接對UI右鍵檢查開啟devTool
- popupjs可使用`Inject Scripts`方式與content script溝通

## Content Script

- 真正跑在網頁context的script，可以操作DOM
- 可傳遞訊息給background script

## Option Page

## Chrome API

- declarativeContent跟開啟的分頁互動

## Misc

- tabs使用`id`來定位，`index`才是順序
- [各component關係](https://developer.chrome.com/extensions/overview)與[各component溝通](https://developer.chrome.com/extensions/messaging)

## 雷

- `about:blank`使用activeTab permission會失敗(還不知道要使用甚麼)
- catch error in callback by [lastError](https://developer.chrome.com/extensions/runtime#property-lastError)