醫療器材智慧審查與補件評估平台 (TFDA AI Regulatory Agent)
系統架構、Agentic OS 工作流與技術建置詳細規格書
Technical Specification Document (Enterprise Edition)
本規格書詳述「TFDA 醫療器材智慧審查與補件評估平台」的完整技術架構、AI 代理程式（AI Agent）工作流、核心演算法設計、互動式資訊看板、以及多工作區（Workspace）無縫切換的建置細節。本平台旨在為醫療器材查驗登記（RA）專業人員提供一站式的智慧輔助工具，極大地縮短查驗登記、臨床前測試評估與官方補件公文（Deficiency Letter）的回覆週期。
目錄 (Table of Contents)
第壹章：系統願景與核心定位 (System Vision & Positioning)
第貳章：系統架構與多工作區設計 (System Architecture & Workspace UI)
第參章：大語言模型 (LLM) 與 Agentic ADK 整合規格 (LLM & ADK Integration)
第肆章：22類超音波探頭審查項目與預設合規範本庫 (22 Probe Categories & Templates)
第伍章：核心功能技術實作細節 (Core Feature Implementation Details)
第陸章：六大驚豔 AI 附加功能技術藍圖 (Six WOW AI Features)
第柒章：前端除錯與安全隔離策略 (Debugging & Security Alignment)
第捌章：20 個全面性後續追蹤與引導問題 (20 Follow-up Questions)
第壹頁：系統願景與核心定位 (System Vision & Positioning)
在當今醫療器材產業中，醫療器材查驗登記（Regulatory Affairs, RA）是一項高度複雜、耗時且容錯率極低的專業工作。尤其是高階影像設備（如超音波診斷儀及其中搭配之數十款探頭），其送審文件（Dossiers）通常涵蓋數千頁的技術規範。其範圍包含但不限於：電性安全（IEC 60601-1）、電磁相容（IEC 60601-1-2）、超音波特定安全（IEC 60601-2-37）、生物相容性（ISO 10993 系列）、軟體生命週期確效（IEC 62304）、資通安全（Cybersecurity）以及複雜的電腦輔助診斷（AI CADe/CADx）演算法驗證。
當藥商收到衛生福利部食品藥物管理署（TFDA）的補件通知書（Deficiency Letter）時，通常只有少數的限期天數（例如 30 天）來備齊所有缺失報告。本平台的設計願景即是建構一個合規專家級的智慧代理系統 (Agentic AI Regulatory System)。它不僅能自動解析、比對和對齊技術文件，還能模擬審查員（Auditor）的視角進行預審，並在多工作區中提供直觀且符合直覺的合規輔助環境：
code
Code
┌───────────────────────────────────────────┐
                           │   TFDA Deficiency Letter / Draft Input    │
                           └─────────────────────┬─────────────────────┘
                                                 │
                                                 ▼
                           ┌───────────────────────────────────────────┐
                           │      Agentic ADK & LLM Pipeline           │
                           │  - Google Search Grounding                │
                           │  - Code Interpreter (Acoustic Formulas)   │
                           └─────────────────────┬─────────────────────┘
                                                 │
                                                 ▼
 ┌───────────────────────────────────────────────┴───────────────────────────────────────────────┐
 │                                 Multi-Workspace Interface                                     │
 ├───────────────────────────────────────────────┬───────────────────────────────────────────────┤
 │         Workspace A: TFDA Review              │            Workspace B: Agentic OS            │
 │  - 22 Probe Categories Checklist              │  - Drag-and-drop Skill Playground             │
 │  - Interactive Risk Heatmap (D3/Recharts)     │  - Multi-Model Benchmarking Interface         │
 │  - Voice Dictation (Microphone Support)       │  - Active Live Log Trace Console              │
 │  - Dynamic Revision Timelines                 │  - System Prompt Modifier Drawer              │
 └───────────────────────────────────────────────┴───────────────────────────────────────────────┘
本平台引入了「雙重工作區」概念：
Workspace A: TFDA 審查工作區 (TFDA Review Workspace)：著重於與台灣查驗登記實務對齊。包含 22 類探頭的審查細目、一鍵加載的預設合規範本、以 D3.js/Recharts 打造的風險矩陣圖（Heatmap）、語音聽寫紀錄系統、以及一鍵導出符合 TFDA 送審排版規範的 PDF 報告。
Workspace B: 代理程式沙盒 (Agentic OS Sandbox)：專為技術人員與高階 RA 專家設計。它提供了拖放式 Skill.md 的編輯環境、可調整的 System Prompt 抽屜、Google 搜尋與代碼執行的 ADK 整合工具鏈，以及多模型（如 Gemini-3.1-Flash-lite, Gemini-3.1-Pro 等）輸出比對的 Benchmarker 面板。
第貳章：系統架構與多工作區設計 (System Architecture & Workspace UI)
本平台採用極具現代感、科技感且易用性極高的 Immersive UI（沉浸式使用者介面） 設計。介面以深藍黑色（Deep Slate / Cosmic Dark）為主色調（預設為 #020617），並輔以高對比度的彩色發光標籤（Glow Badges）與柔和的邊框線條。系統支援一鍵在「10 款不同團隊主題色系」中切換，藉此滿足不同專案、模組或審查團隊的視覺偏好。
一、 系統視覺架構與佈局
系統的整體佈局設計為無邊框、多面板、低視覺雜訊的整合式工作台（Integrated Workstation）：
左側導航控制軌 (Side Control Rail)：
寬度固定為 w-16，背景採用極深黑色（#020617）。
上方配置醒目的應用程式圖標（應用了柔和的外發光效果 shadow-indigo-500/20）。
中段配置兩個核心工作區切換按鈕，採用互動式發光指引與側邊微型指示條。
下方配置語系快速切換鈕（繁體中文 / 英文）及狀態健康度（Heartbeat）呼吸燈（採用 animate-pulse 的極光嫩綠）。
頂部全局狀態欄 (Global Header Bar)：
高度固定為 h-16，背景使用帶有毛玻璃效果（Backdrop Blur）的半透明暗色。
包含當前審查機型與所搭配探頭數量的動態副標題（可動態更新，避免硬編碼 Bug）。
配置色彩主題下拉式選單與當前大語言模型切換器。
主工作視窗 (Central Viewport)：
利用 Framer Motion 進行動態管理，透過 <AnimatePresence> 確保 Workspace A 與 Workspace B 在切換時具有流暢的淡入淡出（Fade & Slide）物理動態。
二、 10 款團隊特色風格色系配置表
為使團隊在審查不同探頭或協同工作時具備高度識別度，系統底層設計了對應的動態 Tailwind CSS 變數：
主題 ID	團隊特色風格名稱 (Team Themes)	核心色碼 (Accent Hex)	視覺意象與適用場景
nova-indigo	宇宙靛藍 (Nova Indigo)	#6366f1	預設主題。高科技深度神經網路意象，適用於軟體確效評估。
aether-teal	乙太翡翠 (Aether Teal)	#14b8a6	潔淨與醫學安全意象，適用於生物相容性 (ISO 10993) 審查。
vulcan-amber	火神琥珀 (Vulcan Amber)	#f59e0b	高風險、待補件警告，適用於高功率聲學表面溫升審查。
aurora-green	極光嫩綠 (Aurora Green)	#10b981	完全合規、綠色通道，適用於已就緒 (Completed) 的送審文件。
cyberpunk	霓虹賽博 (Neon Cyberpunk)	#ec4899	強烈視覺對比，適用於突破性創新醫材 (Breakthrough Devices)。
cyber-slate	暗影板岩 (Cyber Slate)	#64748b	中性、低調專業，適合夜間高強度對抗公文修改。
vaporwave-pink	蒸汽蔚藍 (Vaporwave Cyan)	#06b6d4	輕盈且高流暢，適用於日常查核與進度追蹤。
cosmic-purple	極之炫紫 (Cosmic Purple)	#a855f7	深度推理、AI CAD 演算法檢驗專用之科技感色系。
royal-blue	皇家尊藍 (Royal Sky)	#3b82f6	傳統醫學大廠合規風格，適用於向高階主管報告之展示。
monochrome	冷冽鋼灰 (Monochrome Steel)	#71717a	極簡主義風格，去除色彩干擾，專注於合規文本本身。
第參章：大語言模型 (LLM) 與 Agentic ADK 整合規格 (LLM & ADK Integration)
平台的智慧推理層完全架構於新一代的大語言模型體系上，預設使用 gemini-3.1-flash-lite。此模型具有高回應速度、出色的上下文解析力，並且針對法規長文本具有高精準度的特徵。
code
Code
┌───────────────────────┐
                                 │   Agentic ADK Core    │
                                 └───────────┬───────────┘
                                             │
                       ┌─────────────────────┴─────────────────────┐
                       │                                           │
                       ▼                                           ▼
         ┌───────────────────────────┐               ┌───────────────────────────┐
         │  Google Search Grounding  │               │ Code Interpreter Sandbox  │
         │  - Verify latest TFDA     │               │  - Parse CSV datasets     │
         │    regulatory threshold   │               │  - Execute mathematical   │
         │  - Fetch global ISO/IEC   │               │    acoustic validation    │
         │    standard iterations    │               │    thermal rise formulas  │
         └───────────────────────────┘               └───────────────────────────┘
一、 Google 智慧搜尋工具 (Google Search Grounding)
查驗登記極度依賴最新版本的法規與標準。AI Agent 在執行評估時，會自動調用 google_search 工具：
真實性檢驗 (Validation Grounding)：當輸入缺失公文提及「依據本署 2026 年最新公告之人工智慧電腦輔助偵測指引...」時，Agent 會自動聯網檢索 TFDA 官網，確保其生成的補件對策與適應症翻譯完全對齊台灣最新的公告文件（而非過時的歷史基準）。
國際標準映射 (International Standards Mapping)：自動聯網查詢 FDA 或 IEC 官網上關於「IEC 60601-2-37 Edition 3.0」的改版細節，引導原廠研發人員提供最新的測試報告。
二、 程式碼執行沙盒 (Code Interpreter Sandbox)
超音波的安全評估涉及大量的物理量測計算（如：聲壓、機械指數 MI、熱指數 TI、以及表面溫升限值）。
數據集分析 (Dataset Parsing)：使用者可上傳含有 22 款探頭實測聲學輸出的 .csv 或 .json 文件。
自癒式代碼執行 (Self-Healing Code Execution)：Agent 會自動在沙盒中啟動 Python 腳本（built_in_code_execution），對上百個探頭測量點的聲場強度數據進行二次擬合與公式驗證，檢驗其是否真正符合熱指數不大於 1.0、表面溫升不超過 43°C 的物理公式臨界值。
三、 離線 RAG 法規智慧字典 (Offline Vector-Like Index)
考慮到網路連線可能中斷，系統在導出單一 HTML 時，會預先將台灣 TFDA《醫療器材管理法》及《醫療器材查驗登記審查準則》的核心檢索條款、以及 22 類探頭對應的 boilerplate compliance data，壓縮並編譯成高效的前端字典結構。當使用者離線查詢時，無需發送雲端 API，即可透過本機正則表達式（Regex）與模糊檢索（Fuzzy Matching）執行高速的語義相近度推薦。
第肆章：22類超音波探頭審查項目與預設合規範本庫 (22 Probe Categories & Templates)
為了解決醫療器材商「冷啟動 (Cold-Start)」的痛點，本平台獨創了22類超音波探頭預設審查範本庫 (22 Probe Submission Templates)。這 22 個項目完全覆蓋了 TFDA 在臨床前技術文件（Pre-market Technical Dossier）評估中可能挑剔的每一個技術死角。
本系統支援一鍵切換三大核心預設合規範本（如心臟相控陣探頭專用範本、婦產腹部凸陣/線陣範本、以及新型掌上型無線口袋探頭範本），各項目技術審查要點細緻拆解如下：
22 類核心合規評估技術項目細部對照表
MI & TI Assessment (機械與熱指數評估)：評估主機搭配各探頭在最壞情況（Worst-Case）下的機械指數（MI）與熱指數（TIS, TIB, TIC），確認其符合特定臨床部位（如眼科、胎兒、心臟）之安全限值。
Hydrophone Soundfield Calibration (水聽器聲場校正)：檢驗水聽器在聲壓量測過程中的頻率響應校正報告、不確定度宣告（Uncertainty Budget），以及校正有效期限（需在 1 年內）。
IEC 62359 Compliance (IEC 62359 聲學安全宣告)：審查是否依據 IEC 62359:2010 標準計算聲壓與溫升指數，檢附完整的功率與有效強度量測白皮書。
ISO 10993-5 Cytotoxicity (細胞毒性)：探頭與人體接觸部分（如聲透鏡、外殼塑料）必須通過 L929 細胞之體外毒性測試，評估等級需小於等於 1 級。
ISO 10993-10 Skin Irritation (皮膚刺激)：針對皮膚與黏膜接觸探頭，必須提供新西蘭白兔體內刺激性（Skin Irritation / Intracutaneous Reactivity）測試報告，無明顯刺激反應。
ISO 10993-10 Sensitization (皮膚過敏)：提供豚鼠最大斑貼試驗（GPMT）或局部淋巴結分析（LLNA）報告，確認探頭材料無致敏、發炎反應。
Disinfection Compatibility (消毒劑相容性評估)：評估腔內探頭與術中探頭對特定高階化學消毒劑（如 Cidex OPA、Glutaraldehyde）的重覆消殺耐受次數與外觀開裂評估。
IEC 60601-1 Patient Leakage (漏電流)：檢驗探頭之防電擊保護等級（如 CF 型、BF 型），在正常及單一故障（Single Fault Condition）狀態下的患者漏電流（Patient Leakage Current）實測值。
IEC 60601-1-2 EMC Emissions (電磁輻射)：審查探頭與主機系統連接時之電磁干擾發射（Conducted & Radiated Emissions），符合 Class B 醫療級標準。
Dielectric Strength (絕緣強度測試)：確認高強度絕緣保護屏障在最高 4000V AC 耐壓測試下無火花、無閃絡、無擊穿現象。
Axial & Lateral Resolution (軸向與側向解析度)：使用特定仿組織體模（Phantom）測量各探頭在不同深度下的空間解析度（Spatial Resolution）實測規格。
Penetration Depth Test (最大穿透深度測試)：測定各凸陣、線陣、相控陣探頭的最大成像探測深度與信噪比（SNR）衰減極限。
Contrast Resolution / Gray Scale (對比度解析度與灰階)：評估灰階動態範圍（動態範圍需達 120dB 以上）以及區分微小病灶囊腫的能力。
Software Lifecycle IEC 62304 (軟體生命週期規範)：檢查超音波主機軟體之軟體安全分類（Level A/B/C），並提供單元測試、回歸測試與程式碼審查確效報告。
Cybersecurity Penetration Test (網路安全滲透測試)：評估具備 WiFi 或 DICOM 連線功能之探頭與 App，其患者成像數據在傳輸時之加密強度與抗網絡攻擊滲透測試。
AI CADe/CADx Algorithm Validation (AI 電腦輔助偵測算法確效)：審查整合於超音波中的自動病灶偵測（如乳房 S-Detect、甲狀腺偵測、胎兒自動測量）之多中心臨床測試集、ROC 曲線與 AUC 指標。
Lens Temperature Rise (探頭表面溫升測試)：量測探頭於空氣中或仿組織體模中連續運作時的最高表面溫升。與人體接觸之表面溫度必須嚴格控制在 43°C 以下，新生兒/兒科探頭則需更低。
Cable Flexing Endurance (電纜彎折耐受度)：評估探頭通訊訊號導線在連接處承受 180 度反覆彎折（至少 50,000 次）之疲勞強度，不影響電性安全。
IPX7 Waterproofing Rating (IPX7 防水等級測試)：評估手術用探頭或腔內探頭的頭部防水密封防護等級（應符合 IPX7 浸水 1 米深 30 分鐘之規範）。
Contraindication Alignment (說明書禁忌症與對照)：比對英文說明書原文與中文翻譯。確保眼科禁忌、術中接觸等高風險臨床宣告百分之百無歧義對齊。
Warning Symbols & Translation (警語標誌與繁中對齊)：確認說明書內所有 Warning 與 Caution 符號完全符合台灣《醫療器材管理法》之標註形式，無錯別字與不合規簡體字。
Manufacturer QA Certifications (原廠出廠品品管合格紀錄)：要求提報國外原廠出廠之成品出貨品管測試規範（Finished Product Release Specifications）以及實際品管測試報告樣張。
第伍章：核心功能技術實作細節 (Core Feature Implementation Details)
為滿足企業級用戶在進行 TFDA 技術資料整理時的各種應用場景，本平台完美實現了以下核心互動功能：
一、 跨工作區無縫動畫切換 (Framer Motion Switcher)
系統在 App.tsx 中透過 <AnimatePresence> 與 <motion.div> 對 agentic-os 及 tfda-review 工作區進行即時狀態管理。
code
TypeScript
// Workspace切換時的 Framer Motion 配置虛擬程式碼範例
import { motion, AnimatePresence } from 'motion/react';

export default function WorkspaceContainer({ activeWorkspace }) {
  return (
    <div className="flex-1 relative overflow-hidden">
      <AnimatePresence mode="wait">
        {activeWorkspace === 'tfda-review' ? (
          <motion.div
            key="tfda-review-panel"
            initial={{ opacity: 0, x: -30, filter: "blur(10px)" }}
            animate={{ opacity: 1, x: 0, filter: "blur(0px)" }}
            exit={{ opacity: 0, x: 30, filter: "blur(10px)" }}
            transition={{ duration: 0.35, ease: "easeInOut" }}
            className="absolute inset-0 overflow-y-auto"
          >
            <TFDAReviewWorkspace />
          </motion.div>
        ) : (
          <motion.div
            key="agentic-os-panel"
            initial={{ opacity: 0, x: 30, filter: "blur(10px)" }}
            animate={{ opacity: 1, x: 0, filter: "blur(0px)" }}
            exit={{ opacity: 0, x: -30, filter: "blur(10px)" }}
            transition={{ duration: 0.35, ease: "easeInOut" }}
            className="absolute inset-0 overflow-y-auto"
          >
            <AgenticOSWorkspace />
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}
這樣的切換動態能在視覺上建立一種「從底層技術沙盒 (Agentic OS) 提升至上層臨床合規檢核 (TFDA Review)」的流暢空間感，並能有效防止瀏覽器在加載大型數據集與圖表時出現突兀的閃爍。
二、 HTML5 麥克風即時語音聽寫 (Voice Dictation with Speech API)
在實務上，RA 人員在與原廠討論或自行檢查文件時，雙手往往需要翻閱紙本報告。語音聽寫能顯著解放雙手：
底層 API 綁定：系統底層封裝了標準 HTML5 webkitSpeechRecognition 物件。
自動偵測語音語系：
當選擇繁體中文時，內部語言參數自動切換為 zh-TW。
當切換為英文時，自動變更為 en-US。
容錯與分段寫入：監聽 onresult 事件，直接取得辨識度最高的文字字串。將識別出的文本精確追加至當前選定的 deficiency notes 或 response draft 文本域末尾，並自動追加時間戳記，確保記錄不被覆蓋。
安全宣告：本功能完全運行於瀏覽器端本地沙盒，使用前會動態詢問麥克風權限，且絕對不向未經授權的第三方伺服器發送語音裸數據。
三、 結構化、品牌化 PDF 報告一鍵生成 (Structured Branded PDF Export)
不同於粗糙的網頁直接列印，系統在 PDF 輸出時，採用了列印專用媒體樣式表 (@media print) 與特定的 CSS 配置：
強大排版控制：強制隱藏導航軌、頂部狀態欄、沙盒編輯器、與互動式拖拽看板。
頁面溢出防範：在各大主要條款、官方原廠聯繫信（Liaison Letter）容器之間嵌入 page-break-before: always;，保證每一張探頭測試缺失明細在輸出 PDF 時均能以一個完整的新頁面呈現。
Bold Typography 視覺契合：列印模式下強制字體變更為黑體（Noto Sans TC, Sans-Serif），標題設定為粗體大字、深藍色邊框，文字寬度設定為 w-full max-w-full px-0，將所有 textarea 的即時值轉化為無滾動條的實體 p 標籤，確保生成出來的補件說明書宛如 RA 專用排版系統印製般精緻。
四、 高級提示詞調整抽屜 (Advanced Prompt Drawer)
為了讓 RA 專家能隨時控制 AI Agent 的「法規比對標準與語意深度」，系統整合了一個可隨時從右側滑入（Slide-in）的高級 Prompt 抽屜：
使用者可修改預設的 AI 角色指令。例如：從「台灣 TFDA 查驗登記專家」微調為「美國 FDA 510(k) 暨 CE MDR 臨床比對專家」。
一鍵套用 AI 提示優化功能（Enhance System Prompt）。AI 會自動在您的 Prompt 中追加諸如「使用台灣法規常用詞彙如『確效』、『預期用途』；所有不符合項目之回覆草案必須包含具體符合的國際標準（ISO/IEC）編號」等技術補強約束。
五、 Token 溢出與成本預警（Token Limit & Cost Warning Indicator）
在調用 Gemini-3.1-pro 或其他超大型推理模型時，過長的文件上下文可能會導致生成時間變長，甚至超出限制。平台在此處設計了防呆提示：
即時監測模型配置面板中的 Max Tokens、Temperature 參數以及當前編輯中的總字符數。
一旦預估耗費的 Token 上限大於 3,000 或模型溫度（Temperature）過高（容易導致 AI 產生幻覺或與法條偏差），右側與頂部會即時彈出帶有琥珀色（#f59e0b）黃光的外顯警示徽章，並提供 Toast 指引，建議使用者將 Max Tokens 調回預設的 2048、或使用更輕量經濟的 gemini-3.1-flash-lite 執行日常預審，極大降低開發與操作成本。
第陸章：六大驚豔 AI 附加功能技術藍圖 (Six WOW AI Features)
為了將本平台推升至行業領先的「智慧監管大腦（Regulatory Intelligent Hub）」高度，本平台在底層整合了以下 6 大驚豔 AI 代理功能：
code
Code
┌───────────────────────────────────────────┐
                           │   Six WOW AI Regulatory Agent Features    │
                           └─────────────────────┬─────────────────────┘
                                                 │
      ┌───────────────────┬──────────────────────┼──────────────────────┬───────────────────┐
      │                   │                      │                      │                   │
      ▼                   ▼                      ▼                      ▼                   ▼
┌───────────┐       ┌───────────┐          ┌───────────┐          ┌───────────┐       ┌───────────┐
│  WOW 1    │       │  WOW 2    │          │  WOW 3    │          │  WOW 4    │       │  WOW 5    │
│ 合規關係  │       │ 離線問答  │          │ 說明書雙  │          │ AI 模型雙 │       │ 智慧代碼  │
│ 拓撲圖譜  │       │ RAG 助理  │          │ 欄校正    │          │ 盲評測    │       │ 自癒沙盒  │
└───────────┘       └───────────┘          └───────────┘          └───────────┘       └───────────┘
                                                                                            │
                                                                                            ▼
                                                                                      ┌───────────┐
                                                                                      │  WOW 6    │
                                                                                      │ 預測代幣  │
                                                                                      │ 熱能監測  │
                                                                                      └───────────┘
驚豔功能一：合規缺失學術拓撲圖譜 (Visual Compliance Gap Topology Map)
技術實現：在前端基於動態 SVG（具有極輕量與無任何第三方庫加載失敗風險的優點）繪製力導向拓撲圖。以 Q10 主機系統為中心節點，幅射狀連接 22 個評估細目。
互動反饋：
若該項目為 Pass，節點圓圈顯示為翡翠綠，連線為實線。
若項目為 Fail 或 Warning，節點圓圈則會觸發帶有呼吸效果的發光紅色脈衝動畫（node-pulse 樣式），且連線變為虛線，指示「合規斷裂」。
使用者點擊拓撲圖中任何圓圈，右側診斷面板會自動更新其缺失嚴重程度、對應物理限值及快速跳轉按鈕。
驚豔功能二：離線 RAG 法規智慧問答與側邊欄助理 (Offline RAG Chat Assistant)
技術實現：系統封裝了離線語義查詢字典。在加載模版時，會自動將超音波產品的所有申報規格、材質塑料、電路漏電極限值，編譯為前端的可檢索索引。
互動反饋：
在側邊欄對話框中輸入「哪些探頭缺少生物相容性？」或「IEC 60601-2-37 聲學特定要求是什麼？」，離線 RAG 引擎可在 10 毫秒內，基於本機資料提供精準匹配的缺失列表與補件對策指引。
驚豔功能三：說明書中文翻譯與預期用途對應「雙欄防呆對照工具」 (Labeling Alignment Tool)
技術實現：此工具專為解決 RA 申報中最常見的「說明書與臨床規格不一致（標籤不一致）」問題而設。
互動反饋：
左欄顯示原廠英文說明書規格（如：“Contraindication: Ophthalmic use is strictly prohibited...”）。
右欄顯示中文翻譯。AI 會自動利用紅色背光加亮並高能檢測其中的不一致（如：中文版漏掉了「嚴禁」二字、或將「常規」誤拼寫為「當規」）。
使用者點擊「一鍵導入對策」，系統會自動將修正後的繁體中文合規宣告寫入至對應項目的回覆草稿中。
驚豔功能四：AI 多模型雙盲評測與交叉比對器 (Cross-Model Discrepancy Benchmarker)
技術實現：在沙盒工作區中，提供雙欄（Split-View）對照評測功能。
互動反饋：
使用者可指定左欄使用當前預設的極速 gemini-3.1-flash-lite 模型，右欄使用 gemini-3.1-pro 或是經典的 claude-3-5-sonnet 模型。
點擊執行後，雙欄會同步輸出針對同一個技術缺漏點的合規分析對策。系統還會自動比對兩者在引用「法規條款」上的語意漂移度，提供交叉對照，協助 RA 人員挑選最無懈可擊的詞彙。
驚豔功能五：智慧代碼執行與自癒式數據驗證 (Agentic Code Interpreter Sandbox)
技術實現：此功能在 Agentic OS 沙盒中整合了代碼調試工作台。
互動反饋：
當使用者貼上或拖入探頭的實測聲學數值表格（如：熱指數 TI 及機械指數 MI 等 CSV 資料）時，AI Agent 會自動編寫一組 Python 驗證代碼。
在內置虛擬終端機（Live Log Trace）中執行數據校對。若程式在計算熱效應擬合曲線時拋出語法或數值溢出錯誤，AI 代理程式會啟動自我修復機制 (Self-Healing)，在毫秒內自動重構代碼，並輸出完全無誤的聲學強度宣告值與數據校驗表。
驚豔功能六：代幣與成本熱能監測圖譜 (Predictive Token Heatmap Simulator)
技術實現：本功能在高級 Prompt drawer 的邊緣，利用動態漸變圖譜展示 Token 的消耗熱能分佈。
互動反饋：
隨著使用者修改系統 Prompt 與輸入公文的長度，系統會利用顏色漸變（從冰藍色到發熱橙色）即時視覺化展示對應的 Token 消耗、預期反應時間與 API 調用成本。
能有效引導使用者在送出執行前，將冗餘無效的重複公文贅字刪除，達到最高效環保的智慧審查體驗。
第柒章：前端除錯與安全隔離策略 (Debugging & Security Alignment)
為保證整個 HTML 整合平台能完美運行、絕不出現任何白屏（Blank Screen）錯誤，且完全符合安全規範，我們採用了以下核心系統設計：
一、 防禦性 React 元件初始化 (Defensive Rendering & Null Guards)
為了杜絕因後端 API 傳回空物件或資料結構不完整而引發的前端 JavaScript 運行期錯誤（例如 Cannot read properties of undefined (reading 'map')），我們在資料模型層實施了多重空值防禦保護 (Null Guards)：
code
TypeScript
// 渲染表格或列表時的防禦性防護範例
const totalItemsCount = auditItems?.length || 0;
const passedItemsCount = auditItems?.filter(item => item?.status === 'Pass')?.length || 0;

// 當資料庫為空時的 Fallback 渲染 UI
if (!auditItems || auditItems.length === 0) {
  return (
    <div className="flex flex-col items-center justify-center p-12 text-slate-500">
      <AlertTriangle className="w-8 h-8 mb-2" />
      <p className="text-xs">未加載任何技術合規細目，請點擊上方範本加載初始資料。</p>
    </div>
  );
}
任何對 revisions、responseDraft 或 riskScore 的調用都預設加載 fallback 常數，確保即使使用者輸入了格式受損的自訂 JSON，應用程式依然能平穩降級運行，絕不崩潰。
二、 Iframe 權限與麥克風安全性配置 (Iframe Sandbox Compatibility)
由於平台在 Google AI Studio 中會預設在安全 iframe 沙盒中運行，為了讓麥克風語音聽寫（Microphone Support）正常作用，我們必須保證：
Metadata Permissions：已在最外層 /metadata.json 的 requestFramePermissions 陣列中顯式聲明 "microphone"。
Web Speech API Fallback：若偵測到當前宿主環境（Host Environment）拒絕授予 iframe 麥克風錄音權限，系統會自動切換為 fallback 模式：
不強制調用 webkitSpeechRecognition，而是引導用戶使用彈出的「手動備註視窗」或「AI 輔助對策工具」，保證核心回覆功能不中斷。
三、 完美的本地離線存取設計 (Offline Storage & Local Persistence)
本平台所產生的所有修改與補件回應草案，都會自動備份在瀏覽器的 localStorage 中。即使使用者在無網路的飛機上、或是企業內網隔離環境下關閉並重新開啟此網頁，所有撰寫歷史、看板進度與修改後的 System Prompt 都不會遺失。
第捌章：20 個全面性後續追蹤與引導問題 (20 Follow-up Questions)
為了進一步優化平台，以契合您團隊在查驗登記工作流中的具體作業模式，您可以思考並與我們討論以下 20 個關鍵問題：
一、 法規合規性與審查流程 (Regulatory Alignment)
Q1: 您的超音波診斷儀是否包含除 22 類探頭以外的特殊選配組件（例如：內置電子加熱椅、探頭專用穿刺導引架），是否需要將這些也納入預設審查範本庫？
Q2: 針對生物相容性項目（ISO 10993），除了細胞毒性、皮膚刺激及皮膚過敏三項（即 ISO 10993-5 和 -10），探頭是否涉及全身毒性或亞慢性毒性評估，需要系統為您追加對應的合規細目嗎？
Q3: TFDA 經常會要求提供「實質等同性（Predicate Device）比較表」，是否需要 AI Agent 自動讀取並對齊已上市競爭對手的規格參數？
Q4: 對於 IEC 60601-2-37 聲學特定安全，原廠通常只提供英文測試摘要，您是否需要平台在匯出時，自動為您編排出一份符合 TFDA 格式的「聲學輸出申報總結中文對照表」？
Q5: 超音波所搭載的 AI CADe/CADx 演算法，是否需要在 AI 補件引導系統中加載台灣 TFDA「人工智慧/機器學習醫材查驗登記指引」的最新對比檢查表？
二、 系統整合與技術架構 (Technical Integration)
Q6: 平台目前的語音聽寫預設支援國語（zh-TW）與英語（en-US），是否需要追加台語或其他臨床常用語言的辨識？
Q7: 為了避免在 AI 聽寫時錄入鍵盤敲擊等環境雜音，是否需要我們引入前端動態低通濾波（Low-pass Filter）與降噪算法優化音頻訊號？
Q8: 本地 RAG 智慧字典目前的法規庫來源為靜態編譯，是否需要為您開發一個「法規在線同步模組」，能一鍵從衛福部食藥署自動抓取最新的公告標準？
Q9: 您的團隊在導出 Branded PDF 報告時，是否有特定的企業商標（Logo）與頁首頁尾列印模板需要系統進行深度定製與對齊？
Q10: 為了將本平台的離線成果與您企業內部的 PLM（產品生命週期管理系統）或 Document Management System 串接，是否需要支援導出標準的 XML 或 DITA 格式？
三、 使用者體驗與協同工作 (UX & Collaboration)
Q11: 當切換 10 種團隊主題風格時，您最偏好哪幾種顏色搭配？是否需要為特定高亮項目（如 Fail）追加更顯眼的閃爍或語音警報？
Q12: 在 22 類探頭看板任務（Kanban Board）中，是否需要為各別卡片追加「負責人（Owner）」或「任務到期日（Due Date）」的視覺標籤，以提升多部門協作效率？
Q13: 當多個 RA 同事同時修改一份補件對策時，是否需要本系統支援「變更追蹤對比模式（Diff Viewer）」，以紅綠雙色直觀呈現兩次修訂版本間的文字落差？
Q14: 現有的 Visual Revision Timeline 是否需要支援「一鍵復原（Rollback to previous version）」功能，讓使用者能快速退回到 AI 潤飾之前的原始草案？
Q15: 當模型偵測到 token 數即將超限時，除了彈出警報，您是否希望 AI 自動啟用「智慧簡寫與摘要算法」，壓縮公文贅字以節省使用成本？
四、 AI 能力擴充與未來藍圖 (AI Capabilities & Future Roadmap)
Q16: 是否需要將「探頭圖像識別能力」整合進 AI Agent？亦即，當 RA 拍照上傳探頭的外觀照片或銘牌標籤時，AI 會自動識別並判定其標籤是否符合 TFDA 防水等級警告標誌要求。
Q17: 協調信箱（Liaison Email Generator）目前的範本為商務英文，是否需要針對原廠在歐洲、日本的情況，追加德文、法文或日文的語系模板？
Q18: 是否需要 AI 自動針對補件公文中的法規要求，逆向推理出「原廠實驗室所需補測的具體測試配置（Test Setup Diagrams）」並輸出架構建議？
Q19: 在多模型雙盲評測（Cross-Model Benchmarker）中，除了內置的 Gemini 與 Claude，您的企業是否擁有專屬的本地 Fine-tuned 法規私有大模型需要接入？
Q20: 您是否希望本平台與電子簽章系統（如 DocuSign 或 台灣醫材憑證）結合，在生成 PDF 的同時，自動在補件說明的頁尾完成藥商數位蓋章與授權認證？
本規格書為「TFDA 醫療器材智慧審查與補件評估平台」技術實作的最高指引方針。所有前端模組、視覺佈局與智慧 Agent 流程皆严格遵循本文件所述之架構與設計語彙，確保交付產出的醫療器材智慧合規平台兼具極致的工業設計美感與嚴謹的醫療合規實用價值。
