# ICC Profile Collection & Technical Guide / 色彩特性文件权威合集与技术指南

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Profiles](https://img.shields.io/badge/ICC_Profiles-43-brightgreen.svg)](#-目录结构--directory-structure)
[![Documentation](https://img.shields.io/badge/Docs-Bilingual-orange.svg)](#-核心文档--core-documentation)

> **中英文双语全球标准 ICC 色彩特性文件归档、技术规范与权威指南**  
> *A comprehensive archive of global standard ICC color profiles, detailed technical specifications, and authoritative guide in Chinese and English.*

---

## 📖 核心文档 / Core Documentation

- 📘 **[ICC_Profile_Explanation.md](./ICC_Profile_Explanation.md)**：包含 ICC 核心原理、数学架构、CMM 色彩转换、四大渲染意图深度剖析、全球及中国大陆印刷标准的权威技术指南与完整文件索引。
- 📕 **[ICC_Profile_Explanation.pdf](./ICC_Profile_Explanation.pdf)**：18 页高清排版 PDF 版本，适合离线阅读、技术培训或印刷行业从业人员查阅。

---

## 🗂️ 目录结构 / Directory Structure

本项目共归档 **43 个标准 ICC/ICM 文件**，每个配置文件均配有同名独立 Markdown 中英双语技术说明文档（包含标准依据、纸张类型、总墨量 TAC、测量条件及典型应用）：

```
ICC-Profile/
├── README.md                           # 项目说明文档 / Project README
├── ICC_Profile_Explanation.md          # 权威技术指南与完整索引 / Technical Guide & Index
├── ICC_Profile_Explanation.pdf         # 高清排版 PDF 手册 / Formatted PDF Manual
│
├── European_Standards/                 # 欧洲标准体系 (21个) / European Standards (ECI & Fogra)
│   ├── PSOcoated_v3.icc & .md          # FOGRA51 (M1 测量条件，最新欧洲涂布纸标准)
│   ├── PSOuncoated_v3_FOGRA52.icc & .md# FOGRA52 (M1 测量条件，最新欧洲非涂布纸标准)
│   ├── ISOcoated_v2_eci.icc & .md      # FOGRA39 (ECI 参考基准，欧洲应用最广)
│   ├── ISOcoated_v2_300_eci.icc & .md  # FOGRA39 (TAC 300% 降墨量版)
│   ├── eciCMYK_v2.icc & .md            # ECI 官方安全色彩交换空间
│   └── ... (含 LWC, MFC, SNP, SC 等特种纸及调频加网配置文件)
│
├── North_American_Standards/           # 北美标准体系 (10个) / North American (Idealliance & CGATS)
│   ├── GRACoL2013_CRPC6.icc & .md      # CRPC6 / G7 (M1 测量条件，现代商业胶印基准)
│   ├── CoatedGRACoL2006.icc & .md      # CRPC1 / GRACoL 2006 #1 经典单张纸涂布纸
│   ├── SWOP2013C3_CRPC5.icc & .md      # CRPC5 / SWOP (M1 测量条件，轮转出版印刷)
│   ├── USWebCoatedSWOP.icc & .md       # SWOP v2 事实标准
│   ├── USNewsprintSNAP2007.icc & .md   # SNAP 2007 报业印刷标准
│   └── ... (含 Sheetfed Coated/Uncoated, SWOP Grade 3/5 等)
│
├── Japan_Standards/                    # 日本标准体系 (6个) / Japan Standards (Japan Color & JSPST)
│   ├── JapanColor2011Coated.icc & .md  # Japan Color 2011 最新涂布纸标准
│   ├── JapanColor2001Coated.icc & .md  # Japan Color 2001 商业胶印标准
│   ├── JapanColor2001Uncoated.icc & .md# 日本非涂布纸胶印标准
│   ├── JapanColor2002Newspaper.icc & .md# 日本报纸印刷标准
│   ├── JapanColor2003WebCoated.icc & .md# 日本轮转胶印标准
│   └── JapanWebCoated.icc & .md        # 日本杂志广告轮转标准
│
└── Universal_and_RGB_Standards/        # 通用与广色域 RGB 标准 (6个) / Universal & RGB Spaces
    ├── sRGB Color Space Profile.icm & .md # IEC 61966-2-1 互联网与消费显示基准
    ├── Display-P3.icc & .md            # Apple / DCI-P3 广色域显示器标准
    ├── AdobeRGB1998.icc & .md          # 摄影与印前专业广色域空间
    ├── ProPhoto.icm & .md              # Kodak 超广色域 RAW 工作流程空间
    ├── AppleRGB.icc & .md              # 经典 Macintosh 色彩空间
    └── ColorMatchRGB.icc & .md         # 印刷匹配屏幕色彩空间
```

---

## 💻 安装与使用指南 / Installation & Usage

### Windows
1. 右键单击任一 `.icc` 或 `.icm` 文件，选择 **“安装配置文件” (Install Profile)**。
2. 或手动将文件复制到：  
   `C:\Windows\System32\spool\drivers\color\`

### macOS
将配置文件拷贝到以下目录之一：
- **仅当前用户**：`~/Library/ColorSync/Profiles/`
- **所有系统用户**：`/Library/ColorSync/Profiles/`

### Linux
将配置文件拷贝到：  
`~/.local/share/icc/` 或 `/usr/share/color/icc/`

### Adobe Creative Cloud (Photoshop / Illustrator / InDesign)
- 安装至系统目录后，重启 Adobe 软件。
- 打开 **编辑 (Edit) -> 颜色设置 (Color Settings)**，即可在 CMYK 或 RGB 工作空间下拉菜单中选取对应配置文件。

---

## 📜 许可证 / License

本项目包含的技术文档和说明均采用 [MIT License](LICENSE) 授权。各个 ICC 配置文件的知识产权归其各自所属机构（ECI, Fogra, Idealliance, Japan Color, Adobe, Apple, HP, Kodak 等）所有，仅供学习、测试与色彩管理生产工作流规范使用。
