# LaTeX 碩士論文模板（NCHU 格式）

本模板以國立中興大學碩士論文格式為基礎，使用 XeLaTeX 編譯，已整理好封面、書名頁、摘要、目錄、章節、參考文獻與附錄的基本架構。

#下載安裝vscode
下載連結 https://code.visualstudio.com/
開啟vscode
點選左側欄explorer，open folder，開啟下載軟體包之資料夾
使用右側欄chat，登入github，輸入建立專案、編譯PDF，即可產出論文

## 文件入口

- `README.md`：使用方式、快速開始、常見操作與疑難排解
- `FORMAT_SPEC.md`：版面與格式規範、模板對應位置、常見微調

## 快速開始

### 1. 修改論文基本資訊

先編輯 `nchusetup.tex`，至少填入系所、題目、作者、指導教授與日期：

```latex
\nchusetup{
  university      = {國立中興大學},
  institute       = {您的系所名稱},
  title           = {您的論文中文題目},
  title*          = {Your Thesis Title in English},
  author          = {您的名字},
  author*         = {Your Name},
  advisor         = {指導教授名字},
  advisor*        = {Advisor Name},
  date            = {2026-08},
  keywords        = {關鍵字一, 關鍵字二, 關鍵字三},
  keywords*       = {Keyword 1, Keyword 2, Keyword 3},
}
```

### 2. 填寫內容檔案

- `front/acknowledgement.tex`：致謝
- `front/abstract.tex`：中英文摘要
- `contents/chapter01.tex` 到 `contents/chapter05.tex`：各章正文
- `back/references.bib`：BibTeX 參考文獻

### 3. 編譯 PDF

最穩定的方式是直接用 XeLaTeX + BibTeX；如果你的環境已經能正常使用 `latexmk`，再改用它即可。

手動編譯順序如下：

```bash
xelatex main.tex
xelatex main.tex
xelatex main.tex
```

加入 `\cite{...}` 之後，再補跑一次：

```bash
bibtex main
xelatex main.tex
xelatex main.tex
```

若你的環境支援 `latexmk`，也可以使用：

```bash
latexmk -xelatex main.tex
```

### 4. 檢查輸出

至少確認以下幾點：

- 封面與書名頁資訊是否正確
- 致謝、摘要、目錄頁碼是否正常
- 正文是否從阿拉伯數字 `1` 開始
- 圖表、引用、參考文獻是否都有出現

## 專案結構

| 路徑 | 用途 |
|------|------|
| `main.tex` | 主文件，依序串接封面、摘要、目錄、正文與參考文獻 |
| `nchusetup.tex` | 論文基本資訊與額外套件設定 |
| `nchuthesis.cls` | 模板格式定義，控制字體、頁面、章節樣式等 |
| `front/` | 前置資料，如致謝與摘要 |
| `contents/` | 正文章節內容 |
| `back/` | 參考文獻與附錄 |
| `figures/` | 各章圖片資源 |

## 常用操作

### 切換語言或版面選項

在 `main.tex` 的 `\documentclass` 調整：

```latex
\documentclass[
  twoside,
  openright,
  degree    = master,
  language  = chinese,
  fontset   = system,
  watermark = false,
  doi       = false,
]{nchuthesis}
```

常用選項如下：

- `degree = master` 或 `doctor`
- `language = chinese` 或 `english`
- `fontset = system`、`overleaf`、`template`、`default`
- `watermark = true` 或 `false`
- `doi = true` 或 `false`

補充：

- `system`：使用本機已安裝的字體，最符合目前模板預設
- `overleaf`：改用較適合 Overleaf 的字體組合
- `template`：目前等同 `system`，保留為相容別名

### 新增或啟用附錄

`main.tex` 末尾已預留範例，取消註解即可：

```latex
% \appendix{A}{附錄A標題}
% \input{back/appendix01}
% \appendix{B}{附錄B標題}
% \input{back/appendix02}
```

`back/appendix01.tex` 與 `back/appendix02.tex` 只需要放附錄內容，不要再重寫 `\appendix{...}`。

### 插入圖片

將圖片放到對應章節資料夾後，在章節檔中加入：

```latex
\begin{figure}[htbp]
    \centering
    \includegraphics[width=0.7\textwidth]{figures/chapter01/example.png}
    \caption{圖片說明}
    \label{fig:example}
\end{figure}
```

### 新增參考文獻

在 `back/references.bib` 新增條目，正文使用 `\cite{key}`：

```bibtex
@article{example2020,
  title   = {Paper Title},
  author  = {Author, A.},
  journal = {Journal Name},
  year    = {2020}
}
```

## 常見問題

### 中文顯示異常或亂碼

- 請確認使用的是 XeLaTeX，不是 pdfLaTeX
- 請確認檔案編碼為 UTF-8
- 若是非 Windows 環境，請優先嘗試 `fontset = overleaf` 或自行修改 `nchuthesis.cls`

### 找不到字體

- Windows 環境可先嘗試 `fontset = system`
- Overleaf 或其他環境可先嘗試 `fontset = overleaf`
- 目前模板沒有內附字體檔；若系統沒有對應字體，需自行修改 `nchuthesis.cls`

### 圖片沒有出現

- 檢查路徑是否正確，例如 `figures/chapter01/example.png`
- 檢查副檔名大小寫是否一致
- 優先使用 `.png`、`.jpg` 或 `.pdf`

### 頁碼不正確

- 前置資料應使用羅馬數字，正文從 `\mainmatter` 後切為阿拉伯數字
- 若自行調整 `main.tex` 順序，請特別檢查 `\pagenumbering{roman}` 與 `\mainmatter` 的位置

### 編譯時缺少套件

- 建議安裝完整的 TeX Live 或 MiKTeX
- 若用 VS Code，請搭配 LaTeX Workshop
- 若在 Windows + MiKTeX 使用 `latexmk`，可能還需要另外安裝 Perl

## 建議工作流程

1. 先完成 `nchusetup.tex` 基本資訊。
2. 確認 `main.tex` 可以成功編譯成 PDF。
3. 再逐步填寫摘要、各章內容與參考文獻。
4. 每次大改後重新編譯，避免最後一次處理太多錯誤。
5. 送出前用 [FORMAT_SPEC.md](FORMAT_SPEC.md) 對照格式細節。

## 補充說明

- 模板目前以 NCHU 碩士論文需求為主；若系所另有細部規定，請以系所公告為準。
- 若你要調整行距、字體、邊界、章節樣式，請直接看 [FORMAT_SPEC.md](FORMAT_SPEC.md)。

## 維護資訊

- 模板整理與維護：黃哲韋，劉宇諾
- 最近整理日期：2026-04-15

## 授權

除學校格式規範、審核頁／授權頁掃描檔等不屬於本模板原創內容的資料外，本模板的程式與範例內容採用 [MIT License](LICENSE)。
