# ICC Profile (色彩特性文件) 权威综合指南与标准体系
# Comprehensive Guide to ICC Profiles and Color Standardization Systems

---

## 目录 / Table of Contents
1. [引言与核心定义 / Introduction & Core Definition](#sec-1)
2. [为什么需要 ICC Profile？/ Why Do We Need ICC Profiles?](#sec-2)
3. [ICC 技术架构与内部数学原理 / Technical Architecture & Internal Mechanics](#sec-3)
4. [四大渲染意图深度剖析 / The Four Rendering Intents Explained](#sec-4)
5. [全球主要区域色彩与印刷标准体系 / Major Regional Color & Printing Standards](#sec-5)
6. [中国大陆 ICC 与印刷色彩标准化体系 / Mainland China's ICC & Color Standardization System](#sec-6)
7. [本机已归档 43 个 ICC 配置文件索引 / Index of 43 Archived ICC Profiles on Local Machine](#sec-7)

---

## 1. 引言与核心定义 / Introduction & Core Definition {#sec-1}

### 中文
**ICC Profile（色彩特性文件 / 色彩描述文件）** 是由 **国际色彩联盟（International Color Consortium, ICC）** 制定的一套跨平台、跨设备、开放式的工业级标准化二进制文件格式，常见扩展名为 `.icc` 或 `.icm`。它记录了特定成像设备（如相机、扫描仪、显示器、打印机、印刷机等）在受控工作条件下的色彩再现能力和物理呈色特性。

在数字色彩工作流中，ICC Profile 充当“数字颜色值”与“真实人类视觉色度”之间的转换字典。借助于色彩管理模块（CMM），计算机系统能够将设备相关的数值（如 RGB、CMYK）转换为设备中立的绝对物理色度（如 CIE L*a*b*、CIE XYZ），从而实现数字图像从采集、后期编辑、屏幕软打样到工业化大批量生产的“所见即所得”。

### English
An **ICC Profile** is an open, cross-platform, vendor-neutral binary file format standardized by the **International Color Consortium (ICC)**, typically using the `.icc` or `.icm` extension. It mathematically characterizes the color reproduction capabilities and physical rendering behavior of specific input, display, or output devices (such as digital cameras, scanners, monitors, proofers, and offset presses) under controlled operational conditions.

Within digital color workflows, an ICC Profile functions as a translation dictionary between device-dependent coordinate values and absolute human-perceptual colorimetry. Driven by a Color Management Module (CMM), the system translates device-specific drive signals (such as RGB or CMYK) into device-independent color coordinates (such as CIE L*a*b* or CIE XYZ), guaranteeing true "What You See Is What You Get" (WYSIWYG) fidelity across capture, post-processing, screen soft-proofing, and high-volume industrial reproduction.

---

## 2. 为什么需要 ICC Profile？/ Why Do We Need ICC Profiles? {#sec-2}

### 中文
1. **设备依赖性的物理局限（Device-Dependent Nature）**：
   * **RGB 发光设备（显示器、手机屏）**：采用加色混合原理（Additive Color Mixing）。不同面板（IPS、OLED、Mini-LED）的红绿蓝荧光粉纯度、背光光谱及量子点涂层各不相同。纯红数值 `RGB(255, 0, 0)` 在高色域专业广色域屏上极其浓郁鲜艳，但在普通办公屏上则显得暗淡偏移。
   * **CMYK 印刷设备（胶印机、凹印机、数码机）**：采用减色吸收原理（Subtractive Color Mixing）。青（C）、品红（M）、黄（Y）、黑（K）四色油墨的光谱吸收率各异，且油墨在不同承印物上的毛细吸收与网点扩大差异极大。相同的一组配方 `CMYK(100, 100, 0, 0)`，印在高白度高平滑度的涂布铜版纸上呈现深邃鲜艳的皇家蓝，而印在吸墨严重的未涂布新闻纸上则会变成暗沉发灰的浑浊紫褐色。
2. **“数字数值并不等同于绝对色彩”**：
   原始文件中的数值（如 50% 网点或 8-bit 数值 128）本质上只是控制硬件喷墨量或电子激发强度的“驱动信号”。没有色彩特性文件的约束，这些数值在客观世界中没有任何精确的色度学意义。ICC 特性文件为每个设备建立数学方程，赋予数字信号精确的物理现实意义。

### English
1. **Physical Limitations of Device Dependency**:
   * **RGB Emissive Devices (Monitors, Mobile Displays)**: Rely on additive color mixing. Disparities in phosphors, LED backlights, color filters, and quantum dot implementations mean that distinct panels possess unique color gamuts. A native value of `RGB(255, 0, 0)` produces intensely saturated crimson on an Apple Display-P3 or AdobeRGB panel, but appears pale and shifted on an entry-level sRGB monitor.
   * **CMYK Print Devices (Lithographic Presses, Gravure, Digital Presses)**: Operate on subtractive color synthesis. Primary process inks (Cyan, Magenta, Yellow, Black) exhibit imperfect pigment purity, variable ink trapping, and divergent dot gain across substrates. An identical formulation of `CMYK(100, 100, 0, 0)` yields a vibrant, deep royal blue on premium coated gloss paper, yet deteriorates into a muddy, dull brownish-purple on absorbent uncoated newsprint.
2. **"Raw Numbers Do Not Equal Absolute Color"**:
   Digital values inside an image file (such as a 50% dot area or a byte value of 128) represent merely raw actuator drive signals for hardware devices. Without an ICC profile binding them, these numbers convey zero unambiguous colorimetric meaning. An ICC profile provides the rigorous mathematical model that translates arbitrary digital code words into absolute physical color.

---

## 3. ICC 技术架构与内部数学原理 / Technical Architecture & Internal Mechanics {#sec-3}

### 中文
#### 3.1 PCS：特性文件连接空间 (Profile Connection Space)
ICC 色彩管理架构的核心是 **PCS**。PCS 是一个与任何物理设备无关的绝对标准比色参考空间：
* **CIE 1976 L*a*b***：根据人类视觉对色相、明度和饱和度的感知均匀性设计，应用最为广泛；
* **CIE 1931 XYZ**：基于人类视网膜三色感官细胞实验响应曲线建立的三刺激值空间。

转换流程始终采用两步星型中转拓扑，彻底消除了不同设备之间 N*(N - 1) 种转换矩阵的组合爆炸：
```
源设备 (Device A)  ===[ A2B 转换 ]===>  中立 PCS (Lab 或 XYZ)  ===[ B2A 转换 ]===>  目标设备 (Device B)
```

#### 3.2 配置文件内部二进制数据结构
根据 ISO 15076-1 / ICC.1 规范，ICC 配置文件由三大部分组成：
1. **128 字节头部 (Profile Header)**：记录文件大小、CMM 类型代码、规范版本号（如 v2.1.0、v2.4.0、v4.2.0）、设备类别代码（`mntr` 显示器、`prtr` 印刷机、`scnr` 扫描仪、`link` 直连）、数据色彩空间（RGB、CMYK、GRAY）、PCS 代码（Lab、XYZ）、创建时间、默认渲染意图以及 D50 标准白点三刺激值。
2. **标签索引表 (Tag Table)**：记录标签总数及每个标签的 4 字符标识符（Tag Signature）、数据在文件内的绝对偏移量（Offset）和字节长度（Size）。
3. **标签元素数据区 (Tagged Element Data)**：
   * **Matrix-TRC 模型**：包含红绿蓝三原色色度坐标矩阵（`rXYZ`, `gXYZ`, `bXYZ`）以及单色调再现阶调曲线（`rTRC`, `gTRC`, `bTRC`）。用于显示器 RGB 文件，计算极快；
   * **多维查找表（LUT Models）**：包含 `A2B0`/`A2B1`/`A2B2`（设备输入至 PCS）和 `B2A0`/`B2A1`/`B2A2`（PCS 输出至设备）多维网格插值查找表。用于复杂 CMYK 胶印与数码打印；
   * **分色与油墨总量限制（TAC / GCR / UCR）**：定义青、品、黄被黑版替代的程度（灰成分替代 GCR / 底色去除 UCR），并将四色总网点面积率严格限制在最大安全阈值内（如 300%、330%），杜绝油墨堆积擦脏。

### English
#### 3.1 PCS: Profile Connection Space
The cornerstone of the ICC color architecture is the **Profile Connection Space (PCS)**, an absolute, device-independent reference color space:
* **CIE 1976 L*a*b***: Widely preferred due to its perceptual uniformity across lightness, chroma, and hue;
* **CIE 1931 XYZ**: The foundational tristimulus color space based on human retinal cone responses.

Transformations strictly follow a two-step hub-and-spoke topology, completely avoiding the combinatorial explosion of N*(N - 1) direct device-to-device matrices:
```
Source Device (A)  ===[ A2B Transform ]===>  Neutral PCS (Lab/XYZ)  ===[ B2A Transform ]===>  Target Device (B)
```

#### 3.2 Internal Binary Data Structure
Under the ISO 15076-1 / ICC.1 specification, every ICC file is partitioned into three segments:
1. **128-Byte Profile Header**: Encodes overall profile byte size, CMM signature, version number (e.g. v2.1.0, v2.4.0, v4.2.0), device class (`mntr` for display, `prtr` for output, `scnr` for input, `link` for DeviceLink), data color space (RGB, CMYK, GRAY), PCS signature (Lab or XYZ), creation timestamp, default rendering intent, and standard CIE D50 illuminant coordinates.
2. **Tag Table**: A dictionary cataloging total tag count, 4-character tag signatures, byte offsets, and tag lengths.
3. **Tagged Element Data**:
   * **Matrix-TRC Models**: Comprises 3x3 primary chromaticity matrix vectors (`rXYZ`, `gXYZ`, `bXYZ`) and Tone Reproduction Curves (`rTRC`, `gTRC`, `bTRC`). Standard for RGB displays due to low computational overhead;
   * **Multidimensional Look-Up Tables (LUTs)**: Encodes N-dimensional interpolation grids including `A2B0`/`A2B1`/`A2B2` (Device to PCS) and `B2A0`/`B2A1`/`B2A2` (PCS to Device). Standard for non-linear CMYK printing;
   * **Color Separation & Ink Limiting (TAC / GCR / UCR)**: Governs Gray Component Replacement (GCR), Under Color Removal (UCR), and Total Area Coverage (TAC, e.g. 300% or 330%) to ensure rapid ink drying and prevent set-off.

---

## 4. 四大渲染意图深度剖析 / The Four Rendering Intents Explained {#sec-4}

### 中文
当源色彩空间的色域（Gamut）超出目标设备的物理显色能力时，ICC 规范提供了 4 种精确的色域压缩算法：

1. **可感知 / Perceptual (`A2B0` / `B2A0`)**：
   * **原理**：全局成比例压缩。维持所有颜色之间的相对明暗与反差关系，即使色域内的颜色也会微调移动，最大程度保留暗调阴影和高光渐变的层次细腻度。
   * **适用场景**：数码摄影、风光及人像照片、艺术画册扫描、民用喷墨照片打印。
2. **相对比色 / Relative Colorimetric (`A2B1` / `B2A1`)**：
   * **原理**：白点对齐与边界裁切。将源图像白点对齐目标介质白点；目标色域能再现的颜色保持 100% 绝对物理色度不变，色域外的溢色全部被垂直裁切投影至色域边界。
   * **适用场景**：商业平版胶印、包装印刷、品牌 VI / 标志设计、工业印前制版（最常用标准）。
3. **绝对比色 / Absolute Colorimetric (`A2B1` / `B2A1`)**：
   * **原理**：绝对测色值无差别重现。不对齐白点，保留绝对色度测量值。若原印刷纸张发黄，数码打样机将在纯白打样纸底色上微量喷印浅黄/青网点以仿真实际印刷纸色。
   * **适用场景**：合同级数码打样（Contract Proofing），用于在打样机上严谨预检实际上机纸张底色与最终印刷效果。
4. **饱和度 / Saturation (`A2B2` / `B2A2`)**：
   * **原理**：极度鲜艳度优先。牺牲色相准确度与阶调平衡，强制将颜色推向目标设备色域的最外边缘，换取视觉冲击力。
   * **适用场景**：商务演示 PPT 图表、信息图、超市促销海报、儿童卡通绘本。

### English
When the color gamut of a source image exceeds the physical reproduction boundaries of the target device, the ICC specification defines four deterministic gamut-mapping rendering intents:

1. **Perceptual (`A2B0` / `B2A0`)**:
   * **Mechanics**: Proportionately compresses the entire source color space into the destination gamut. Preserves relative tonal relationships and visual harmony across shadows and highlights at the expense of exact colorimetric coordinates.
   * **Ideal Use Cases**: Continuous-tone pictorial photography, portrait/fine-art reproduction, consumer desktop photo printing.
2. **Relative Colorimetric (`A2B1` / `B2A1`)**:
   * **Mechanics**: Maps the source white point directly onto the destination substrate white point. All in-gamut colors maintain their exact physical chromaticity; out-of-gamut colors are clipped directly to the closest gamut perimeter.
   * **Ideal Use Cases**: Commercial offset lithography, packaging, corporate identity logos, prepress separations (Industry Standard Default).
3. **Absolute Colorimetric (`A2B1` / `B2A1`)**:
   * **Mechanics**: Reproduces measured colors without adapting to the destination white point. If the simulated paper substrate has a yellowish cast, the proofer prints a tint pattern to emulate the physical paper white.
   * **Ideal Use Cases**: Contract proofing on calibrated digital proofers to preview actual press sheet color and unprinted substrate tint.
4. **Saturation (`A2B2` / `B2A2`)**:
   * **Mechanics**: Prioritizes vividness over colorimetric accuracy. Pushes saturated colors to the extreme outer perimeter of the target gamut, sacrificing hue accuracy and tonal gradation.
   * **Ideal Use Cases**: Business charts, PowerPoint slide graphics, retail sale flyers, cartoon illustrations.

---

## 5. 全球主要区域色彩与印刷标准体系 / Major Regional Color & Printing Standards {#sec-5}

### 中文
国际印刷界根据各地区的纸张抄造标准、环保油墨配方（如 ISO 2846）及机台调校哲学，形成了三大洲际标准：

#### 5.1 欧洲标准体系（European Standards / ECI & Fogra）
* **权威制定机构**：**ECI (European Color Initiative)**、**Fogra (德国印刷科研协会)**、**bvdm (德国印刷与媒体工业联合会)**。
* **基准国际规范**：**ISO 12647-2**（平版印刷）。
* **代表性标准与特性文件**：
  * **`ISO Coated v2 (ECI)` / `Coated FOGRA39`**：2004 年发布，长期主导欧洲及全球外贸印刷的黄金准则。适用于优质涂布纸（类型 1 和类型 2），标准版总墨量 330%，优化版 `ISO Coated v2 300%` 将总墨量收紧至 300%，极大提升干燥效率。
  * **`PSO Coated v3 (FOGRA51)`**：2015 年推出的新一代欧洲平版胶印标准（ISO 12647-2:2013）。采用全新的 **M1 测量条件**（包含紫外线成分），精准预测和再现了现代含荧光增白剂（OBA）纸张在标准 D50 看样光源下的视觉效果，消除了跨介质同色异谱现象。
  * **`PSO Uncoated v3 (FOGRA52)`**：针对现代高 OBA 含量非涂布纸（双胶纸、纯质纸、书写纸）的新一代胶印基准。
  * **`eciCMYK v2 (FOGRA59)`**：由 ECI 官方定制的色彩交换空间（Exchange Color Space），作为印前设计跨媒体分色的通用中转色彩桥梁。

#### 5.2 北美标准体系（North American Standards / Idealliance & CGATS）
* **权威制定机构**：**Idealliance (国际数字企业联盟)**、**CGATS (美国图形艺术技术标准委员会)**。
* **核心控制哲学**：**G7 灰平衡校准方法**。突破传统的网点增大率（TVI）控制，将中性灰印刷浓度曲线（NPDC）和灰平衡平衡度作为核心基准，实现不同印刷工艺间高度一致的视觉共享外观（Shared Neutral Appearance）。
* **代表性标准与特性文件**：
  * **`Coated GRACoL 2006` (CGATS TR 006)**：北美高质量商业平版单张纸胶印事实标准（针对 Grade 1 顶级涂布纸）。
  * **`GRACoL 2013 (CRPC6)`**：依据 **ISO 15339 / CGATS 21** 规范制定，为现代宽色域印刷机和数字打印机提供通用参考条件（CRPC6）。
  * **`U.S. Web Coated (SWOP) v2` / `SWOP 2006 / 2013`**：北美轮转热固出版印刷（Specifications for Web Offset Publications）核心规范，专用于杂志、周刊及商业型录。

#### 5.3 日本标准体系（Japan Standards / Japan Color）
* **权威制定机构**：**Japan Color 认证制度委员会**、**日本印刷学会 (JSPST)**。
* **技术特征**：针对日本特有的微涂纸、高平滑铜版纸及本土油墨呈色特性定制，强调极细腻的高光层次还原与极纯净的暗调分离度。
* **代表性标准与特性文件**：
  * **`Japan Color 2001 Coated` / `Japan Color 2011 Coated`**：日本商业单张纸平版印刷的法定义规，国内承接日韩外贸出版物印刷必须严格遵守的色彩标准。
  * **`Japan Color 2003 Web` / `Japan Web Coated (Ad)`**：日本商业轮转期刊与报刊广告的指定色彩文件。

### English
Global printing regions adhere to distinct standard frameworks reflecting variations in paper chemistry, ink formulations (ISO 2846), and pressroom calibration practices:

#### 5.1 European Framework (ECI & Fogra)
* **Standardization Bodies**: **European Color Initiative (ECI)**, **Fogra Graphic Technology Research Association**, and **bvdm (German Printing and Media Industries Federation)**.
* **Core Foundation**: **ISO 12647-2** (Offset Lithographic Processes).
* **Key Profiles**:
  * **`ISO Coated v2 (ECI)` / `Coated FOGRA39`**: Introduced in 2004, this remains the most widely deployed legacy benchmark globally for offset printing on Grade 1 & 2 coated stocks (TAC 330% or 300%).
  * **`PSO Coated v3 (FOGRA51)`**: The flagship standard introduced in 2015 based on ISO 12647-2:2013. Designed around **M1 measurement conditions** (D50 with defined UV content), effectively eliminating metameric failure on substrates containing Optical Brightening Agents (OBAs).
  * **`PSO Uncoated v3 (FOGRA52)`**: The modern benchmark for woodfree uncoated papers with moderate-to-high OBA content.
  * **`eciCMYK v2 (FOGRA59)`**: The official exchange color space facilitating seamless multi-channel conversions.

#### 5.2 North American Framework (Idealliance & CGATS)
* **Standardization Bodies**: **Idealliance** and **Committee for Graphic Arts Technologies Standards (CGATS)**.
* **Core Calibration Methodology**: **G7 Methodology**, focusing on Neutral Print Density Curves (NPDC) and midtone gray balance rather than conventional dot gain (TVI) targets.
* **Key Profiles**:
  * **`Coated GRACoL 2006` (CGATS TR 006)**: The de facto North American sheetfed commercial offset standard on Grade 1 premium coated papers.
  * **`GRACoL 2013 (CRPC6)`**: Modern dataset conforming to **ISO 15339 / CGATS 21**, establishing a characterized reference print condition across offset and digital devices.
  * **`U.S. Web Coated (SWOP) v2` / `SWOP 2006 / 2013`**: Specifications for Web Offset Publications, targeting publication and catalog web offset presses.

#### 5.3 Japanese Framework (Japan Color)
* **Standardization Bodies**: **Japan Color Certification Scheme Committee** and **The Japanese Society of Printing Science and Technology (JSPST)**.
* **Characteristics**: Specifically formulated for high-smoothness Japanese coated stocks and domestic ink pigments, delivering exceptional highlight micro-contrast and clean shadow separation.
* **Key Profiles**:
  * **`Japan Color 2001 Coated` / `Japan Color 2011 Coated`**: Primary standards for sheetfed commercial printing across Japan and export publishing.
  * **`Japan Color 2003 Web` / `Japan Web Coated (Ad)`**: Dedicated profiles for Japanese heatset publication rotary presses and newspaper magazine inserts.

---

## 6. 中国大陆 ICC 与印刷色彩标准化体系 / Mainland China's ICC & Color Standardization System {#sec-6}

### 中文
中国大陆在国家标准与行业标准层面，构建了高度严谨、全面同步国际 ISO 体系的色彩管理技术规程，由 **全国印刷标准化技术委员会（SAC/TC 170）**、**全国颜色标准化技术委员会（SAC/TC 79）** 以及国家新闻出版署归口统一管理。

#### 6.1 核心国家标准（GB/T）与国际 ISO 对标矩阵

| 国家标准编号 (GB/T) | 国际对应标准 (ISO) | 标准中文全称 / Title | 工业核心作用与技术规范 (Technical Function) |
| :--- | :--- | :--- | :--- |
| **GB/T 17934.1-2007** | ISO 12647-1:2002 | **印刷技术 网目调分色版、样张和生产印刷品的加工过程控制 第1部分: 参数与测量方法** | 确立全国印刷色彩控制的基础参数定义、测量几何条件（0°:45° 或 45°:0°）、分光反射率评价规程与公差界限。 |
| **GB/T 17934.2-2014** | ISO 12647-2:2013 | **第2部分: 胶印过程 (Offset Lithographic Processes)** | **中国平版商业印刷核心国家标准**。严格规定了 1 类（亮光铜版纸）、2 类（无光铜版纸）、4 类（未涂布白纸）、5 类（未涂布微黄书写纸）的标准色度座标（L*a*b*）、实地油墨色差公差（ΔE_ab ≤ 5）、网点增大阶调再现曲线（TVI Curves A~F）与灰平衡容限。 |
| **GB/T 17934.3-2003** | ISO 12647-3:1998 | **第3部分: 新闻纸冷固型油墨轮转胶印** | 规范全国报纸轮转印前分色、油墨实地密度及网点增大补偿。 |
| **GB/T 17934.4-2007** | ISO 12647-4:2005 | **第4部分: 出版凹版印刷** | 规范高端画册与软包装凹版印刷油墨迁移与色彩容差。 |
| **GB/T 17934.5-2007** | ISO 12647-5:2001 | **第5部分: 网印 (Screen Printing)** | 丝网印刷厚墨层与特种承印介质的加工控制规范。 |
| **GB/T 17934.6-2014** | ISO 12647-6:2012 | **第6部分: 柔性版印刷 (Flexographic Printing)** | 现代瓦楞纸箱、标签及食品软包装柔印色彩控制标准。 |
| **GB/T 17934.7-2014** | ISO 12647-7:2013 | **第7部分: 处在直接数字数据状态下的样张加工过程** | **数码打样国家核心标准**。规定喷墨打样机模拟传统印刷时的系统配置、色彩测控条（如 IDEAlliance / Fogra Media Wedge）判定公差（平均 ΔE_00 ≤ 1.5，纸白 ΔE_00 ≤ 3.0）。 |
| **GB/T 17934.8-2014** | ISO 12647-8:2012 | **第8部分: 采用网目调分色的工业印刷验证过程** | 工业级快速印刷打样与小批量快印签样验证规程。 |
| **GB/T 23649-2009** | ISO 12642-1 / ISO 12642-2 | **印刷技术 印刷色彩管理 特征化数据的测试图** | **制作与检验任何印刷 ICC Profile 的核心国标**。等同转化国际标准的 IT8.7/3（928 色块）与 IT8.7/4（1617 色块）基准测试色靶。 |
| **GB/T 33244-2016** | 自主制定 / National | **数字印刷 印刷色彩管理指南** | 指导生产型数码印刷机（高速喷墨、静电碳粉）建立机台专属性 ICC 曲线的工程指南。 |
| **GB/T 36319-2018** | 自主制定 / National | **数码打样印刷色彩管理** | 针对跨介质打样与专色仿真工作流的色彩管理规程。 |
| **ISO 15076-1 转化** | ISO 15076-1:2010 | **图像技术 色彩管理 体系、特性文件格式和数据结构** | 规范国内印前软件对国际 ICC.1:2010 标准二进制数据结构的兼容解析。 |

#### 6.2 印刷行业标准（CY/T 系列）
* **CY/T 242-2021《印刷技术 印刷色彩管理 印刷机状态校准指南》**：指导印刷厂通过 CTP 出版补偿曲线、水墨平衡校准，使机台稳定达到标准色彩状态。
* **CY/T 50-2008《平版印刷品质量要求及检验方法》**：国内商业印刷品质量成品验收的国家法定基准。
* **CY/T 130.1-2015《绿色印刷 通用技术要求与评价方法》**：引入环保低 VOC 油墨并对总墨量（TAC）提出控制要求。

#### 6.3 中国大陆实际生产应用与本土化项目
1. **外贸与商业印前生产现状**：
   在 Adobe Creative Cloud 及工业流程软件（如海德堡 Prinect、柯达印能捷 Prinergy）中，国内大多数书刊、商业画册及包装出口企业，印前默认首选为 **`ISO Coated v2 (FOGRA39)`**、新一代 **`PSO Coated v3 (FOGRA51)`**、北美 **`GRACoL 2006/2013`** 与日本 **`Japan Color 2011`**。
2. **CMAC（中国印刷色彩管理规范）与本土色彩数据库**：
   由中国印刷科学技术研究院牵头，联合国内主力纸业巨头（太阳纸业、晨鸣纸业、博汇纸业）及本土环保油墨厂，采集了国内大宗涂布纸与双胶纸的反射光谱特性，建立了专门的中国印刷基准色彩数据库。
3. **大型印刷企业的定制化 ICC Profile**：
   大型印刷上市公司（如裕同科技、劲嘉股份、雅昌文化、美盈森等）均依据 **GB/T 17934.2 / G7** 规范，使用分光光度计（如 X-Rite eXact、Konica Minolta FD-7）在现场定期测量 IT8.7/4 色靶，生成并固化厂内专用的机台-纸张级 ICC Profile。

### English
Mainland China has established a comprehensive, rigorous standardization framework for color management aligned with international ISO benchmarks, governed under **SAC/TC 170 (National Technical Committee on Printing Standardization of Standardization Administration of China)** and the National Press and Publication Administration.

#### 6.1 National Standards (GB/T) Cross-Reference
* **GB/T 17934.1-2007 (ISO 12647-1)**: Foundational measurement parameters, 0°:45° geometry, and colorimetric evaluation criteria.
* **GB/T 17934.2-2014 (ISO 12647-2)**: Core lithographic process control, establishing substrate CIELAB targets, solid ink tolerances (ΔE_ab ≤ 5), TVI dot gain curves (A~F), and gray balance criteria across Paper Types 1 through 5.
* **GB/T 17934.7-2014 (ISO 12647-7)**: Direct digital contract proofing standard, prescribing tolerances for media verification control strips (Average ΔE_00 ≤ 1.5, Substrate white ΔE_00 ≤ 3.0).
* **GB/T 23649-2009 (ISO 12642-1/2)**: Standard test targets (IT8.7/3 and IT8.7/4), serving as the universal empirical foundation for profiling press characterization.
* **GB/T 33244-2016 & GB/T 36319-2018**: Production guidelines governing digital press profiling and multi-substrate digital contract proofing.
* **ISO 15076-1 Alignment**: National transposition ensuring complete binary compatibility with ICC.1:2010 profile specifications.

#### 6.2 Industry Standards (CY/T)
* **CY/T 242-2021**: Pressroom calibration protocols detailing RIP/CTP curve linearizations to match standard target conditions.
* **CY/T 50-2008**: Statutory quality requirements and inspection methodologies for planographic prints.
* **CY/T 130.1-2015**: Green printing standards enforcing low-VOC inks and total ink coverage ceilings.

#### 6.3 Industrial Practice in Mainland China
1. **Commercial & Export Prepress**: Adobe Creative Cloud and RIP systems (Heidelberg Prinect, Kodak Prinergy) default to **`ISO Coated v2 (FOGRA39)`**, **`PSO Coated v3 (FOGRA51)`**, **`GRACoL 2006/2013`**, and **`Japan Color 2011`**, ensuring zero-loss global interoperability.
2. **CMAC (China Management of Printing Color)**: Led by the China Academy of Printing Technology, gathering spectral data from domestic paper mills (Sun Paper, Chenming Paper, Bohui) and domestic eco-inks to produce native characterization datasets.
3. **Enterprise Custom Profiling**: Leading packaging corporations (Yuto, Jinjia, Artron) calibrate custom press profiles internally under GB/T 17934.2 and G7 methodologies using automated spectrophotometers.

---

## 7. 本机已归档 43 个 ICC 配置文件索引 / Index of 43 Archived ICC Profiles on Local Machine {#sec-7}

当前目录下包含四个体系分类目录，共计 43 个标准 ICC/ICM 文件。每一个文件均配有独立的同名 `.md` 中英文双语详尽文档：

```
ICC-Profile/
├── ICC_Profile_Explanation.md          <-- [本文件] 综合技术指南与标准体系
├── European_Standards/                  <-- 欧洲 ECI & Fogra 标准体系 (21个)
├── North_American_Standards/          <-- 北美 Idealliance & CGATS 标准体系 (10个)
├── Japan_Standards/                   <-- 日本印刷学会 Japan Color 标准体系 (6个)
└── Universal_and_RGB_Standards/       <-- 通用 RGB 及广色域显示空间 (6个)
```

---

### 详细配置文件索引表 / Detailed Profile Index Table

> 以下为本仓库中所有 ICC/ICM 配置文件的快速索引，点击链接可查看每个配置文件的详细说明文档。
>
> Below is a quick index of all ICC/ICM profiles in this repository. Click each link to view its detailed documentation.

### 欧洲标准 / European Standards

| 配置文件 / Profile | 标准 / Standard | TAC | 说明文档 / Documentation |
|---|---|---|---|
| `CoatedFOGRA27.icc` | ISO 12647-2:2004 / FOGRA27 | 350% | [CoatedFOGRA27.md](./European_Standards/CoatedFOGRA27.md) |
| `CoatedFOGRA39.icc` | ISO 12647-2:2004/Amd 1 / FOGRA39 | 330% | [CoatedFOGRA39.md](./European_Standards/CoatedFOGRA39.md) |
| `EuroscaleCoated.icc` | Euroscale（欧洲旧标准） | 350% | [EuroscaleCoated.md](./European_Standards/EuroscaleCoated.md) |
| `EuroscaleUncoated.icc` | Euroscale（欧洲旧标准） | 280% | [EuroscaleUncoated.md](./European_Standards/EuroscaleUncoated.md) |
| `ISOcoated_v2_300_eci.icc` | FOGRA39 / ISO 12647-2 | 300% | [ISOcoated_v2_300_eci.md](./European_Standards/ISOcoated_v2_300_eci.md) |
| `ISOcoated_v2_eci.icc` | FOGRA39 / ISO 12647-2 | 330% | [ISOcoated_v2_eci.md](./European_Standards/ISOcoated_v2_eci.md) |
| `ISOuncoatedyellowish.icc` | FOGRA30 / ISO 12647-2 | 280% | [ISOuncoatedyellowish.md](./European_Standards/ISOuncoatedyellowish.md) |
| `PSO_Coated_300_NPscreen_ISO12647_eci.icc` | FOGRA39 / ISO 12647-2 | 300% | [PSO_Coated_300_NPscreen_ISO12647_eci.md](./European_Standards/PSO_Coated_300_NPscreen_ISO12647_eci.md) |
| `PSO_Coated_NPscreen_ISO12647_eci.icc` | FOGRA39 / ISO 12647-2 | 330% | [PSO_Coated_NPscreen_ISO12647_eci.md](./European_Standards/PSO_Coated_NPscreen_ISO12647_eci.md) |
| `PSO_LWC_Improved_eci.icc` | FOGRA45 / ISO 12647-2 | 300% | [PSO_LWC_Improved_eci.md](./European_Standards/PSO_LWC_Improved_eci.md) |
| `PSO_LWC_Standard_eci.icc` | FOGRA44 / ISO 12647-2 | 300% | [PSO_LWC_Standard_eci.md](./European_Standards/PSO_LWC_Standard_eci.md) |
| `PSO_MFC_Paper_eci.icc` | FOGRA46 / ISO 12647-2 | 280% | [PSO_MFC_Paper_eci.md](./European_Standards/PSO_MFC_Paper_eci.md) |
| `PSO_SNP_Paper_eci.icc` | FOGRA47 / ISO 12647-3 | 240% | [PSO_SNP_Paper_eci.md](./European_Standards/PSO_SNP_Paper_eci.md) |
| `PSO_Uncoated_ISO12647_eci.icc` | FOGRA47 / ISO 12647-2 | 280% | [PSO_Uncoated_ISO12647_eci.md](./European_Standards/PSO_Uncoated_ISO12647_eci.md) |
| `PSO_Uncoated_NPscreen_ISO12647_eci.icc` | FOGRA47 / ISO 12647-2 | 280% | [PSO_Uncoated_NPscreen_ISO12647_eci.md](./European_Standards/PSO_Uncoated_NPscreen_ISO12647_eci.md) |
| `PSOcoated_v3.icc` | FOGRA51 / ISO 12647-2:2013 | 300% | [PSOcoated_v3.md](./European_Standards/PSOcoated_v3.md) |
| `PSOuncoated_v3_FOGRA52.icc` | FOGRA52 / ISO 12647-2:2013 | 280% | [PSOuncoated_v3_FOGRA52.md](./European_Standards/PSOuncoated_v3_FOGRA52.md) |
| `SC_paper_eci.icc` | FOGRA48 / ISO 12647-2 | 260% | [SC_paper_eci.md](./European_Standards/SC_paper_eci.md) |
| `UncoatedFOGRA29.icc` | ISO 12647-2:2004 / FOGRA29 | 280% | [UncoatedFOGRA29.md](./European_Standards/UncoatedFOGRA29.md) |
| `WebCoatedFOGRA28.icc` | ISO 12647-2:2004 / FOGRA28 | 300% | [WebCoatedFOGRA28.md](./European_Standards/WebCoatedFOGRA28.md) |
| `eciCMYK_v2.icc` | ECI 交换空间 | 350% | [eciCMYK_v2.md](./European_Standards/eciCMYK_v2.md) |

### 北美标准 / North American Standards

| 配置文件 / Profile | 标准 / Standard | TAC | 说明文档 / Documentation |
|---|---|---|---|
| `CoatedGRACoL2006.icc` | CRPC1 / GRACoL / IDEAlliance | 340% | [CoatedGRACoL2006.md](./North_American_Standards/CoatedGRACoL2006.md) |
| `GRACoL2013_CRPC6.icc` | CRPC6 / G7 / IDEAlliance | 300% | [GRACoL2013_CRPC6.md](./North_American_Standards/GRACoL2013_CRPC6.md) |
| `SWOP2013C3_CRPC5.icc` | CRPC5 / SWOP / IDEAlliance | 300% | [SWOP2013C3_CRPC5.md](./North_American_Standards/SWOP2013C3_CRPC5.md) |
| `USNewsprintSNAP2007.icc` | SNAP 2007 | 240% | [USNewsprintSNAP2007.md](./North_American_Standards/USNewsprintSNAP2007.md) |
| `USSheetfedCoated.icc` | SWOP / GRACoL | 350% | [USSheetfedCoated.md](./North_American_Standards/USSheetfedCoated.md) |
| `USSheetfedUncoated.icc` | SWOP / GRACoL | 260% | [USSheetfedUncoated.md](./North_American_Standards/USSheetfedUncoated.md) |
| `USWebCoatedSWOP.icc` | SWOP | 300% | [USWebCoatedSWOP.md](./North_American_Standards/USWebCoatedSWOP.md) |
| `USWebUncoated.icc` | SWOP | 260% | [USWebUncoated.md](./North_American_Standards/USWebUncoated.md) |
| `WebCoatedSWOP2006Grade3.icc` | SWOP 2006 | 300% | [WebCoatedSWOP2006Grade3.md](./North_American_Standards/WebCoatedSWOP2006Grade3.md) |
| `WebCoatedSWOP2006Grade5.icc` | SWOP 2006 | 300% | [WebCoatedSWOP2006Grade5.md](./North_American_Standards/WebCoatedSWOP2006Grade5.md) |

### 日本标准 / Japan Standards

| 配置文件 / Profile | 标准 / Standard | TAC | 说明文档 / Documentation |
|---|---|---|---|
| `JapanColor2001Coated.icc` | Japan Color 2001 | 350% | [JapanColor2001Coated.md](./Japan_Standards/JapanColor2001Coated.md) |
| `JapanColor2001Uncoated.icc` | Japan Color 2001 | 310% | [JapanColor2001Uncoated.md](./Japan_Standards/JapanColor2001Uncoated.md) |
| `JapanColor2002Newspaper.icc` | Japan Color 2002 | 240% | [JapanColor2002Newspaper.md](./Japan_Standards/JapanColor2002Newspaper.md) |
| `JapanColor2003WebCoated.icc` | Japan Color 2003 | 320% | [JapanColor2003WebCoated.md](./Japan_Standards/JapanColor2003WebCoated.md) |
| `JapanColor2011Coated.icc` | Japan Color 2011 | 350% | [JapanColor2011Coated.md](./Japan_Standards/JapanColor2011Coated.md) |
| `JapanWebCoated.icc` | Japan Color | 300% | [JapanWebCoated.md](./Japan_Standards/JapanWebCoated.md) |

### 通用与 RGB 标准 / Universal & RGB Standards

| 配置文件 / Profile | 标准 / Standard | TAC | 说明文档 / Documentation |
|---|---|---|---|
| `AdobeRGB1998.icc` | Adobe | N/A | [AdobeRGB1998.md](./Universal_and_RGB_Standards/AdobeRGB1998.md) |
| `AppleRGB.icc` | Apple | N/A | [AppleRGB.md](./Universal_and_RGB_Standards/AppleRGB.md) |
| `ColorMatchRGB.icc` | Radius/Apple | N/A | [ColorMatchRGB.md](./Universal_and_RGB_Standards/ColorMatchRGB.md) |
| `Display-P3.icc` | Apple / DCI-P3 | N/A | [Display-P3.md](./Universal_and_RGB_Standards/Display-P3.md) |
| `ProPhoto.icm` | Kodak / ISO 22028-2 | N/A | [ProPhoto.md](./Universal_and_RGB_Standards/ProPhoto.md) |
| `sRGB Color Space Profile.icm` | IEC 61966-2-1 / HP & Microsoft | N/A | [sRGB Color Space Profile.md](./Universal_and_RGB_Standards/sRGB Color Space Profile.md) |

