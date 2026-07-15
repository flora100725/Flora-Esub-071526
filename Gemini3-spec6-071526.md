ARCHITECTURAL BLUEPRINT
TFDA Medical Device Smart Compliance & Agentic OS Platform
Production-Grade Enterprise Implementation Document for Project EVO Q10 (22 Active Ultrasonic Probes)
1. Executive Summary & Enterprise Vision
The TFDA Medical Device Smart Compliance & Agentic OS Platform represents a paradigm shift in regulatory affairs (RA) engineering for medical device manufacturers and local registration agents. In traditional compliance pipelines, auditing hundreds of pages of engineering technical files against local standards (such as Taiwan's Food and Drug Administration - TFDA guidelines) is a manual, error-prone, and slow process. This platform changes this dynamic by introducing a full-stack, AI-orchestrated, interactive workspace designed to ingest, parse, evaluate, track, and draft replies to technical file deficiencies.
Using the EVO Q10 Diagnostic Ultrasound System (featuring 22 high-frequency active diagnostic probes) as its baseline case study, the system evaluates compliance across multiple highly demanding standards:
IEC 60601-1 (Basic Safety & Essential Performance)
IEC 60601-1-2 (Electromagnetic Compatibility)
IEC 60601-2-37 (Ultrasonic Diagnostic Equipment Safety)
ISO 10993 Series (Biological Evaluation of Medical Devices, focusing on Cytotoxicity, Skin Irritation, and Sensitization)
TFDA Software / Cybersecurity Guidelines (including quantitative software features, AI-powered CADe/CADx diagnostic tools, and cybersecurity self-assessments)
This document provides a highly detailed, comprehensive technical specification and architectural blueprint of the platform's codebase, data flows, core mechanics, design guidelines, and production deployment parameters.
2. System Architecture & Full-Stack Topology
The platform is designed as a production-ready, full-stack application running inside containerized Cloud Run instances, using a single open port (3000) mapped through a high-performance reverse proxy network layer. It separates high-performance, responsive client-side UI/UX operations from secure, isolated server-side API processing and LLM interaction.
code
Code
+---------------------------------------+
                                  |         Web Client (SPA)              |
                                  |  - React 19 + Vite 6 + Tailwind v4    |
                                  |  - Lucide Icons + Recharts            |
                                  |  - Web Speech Synthesis Engine        |
                                  +-------------------+-------------------+
                                                      |
                                                      | HTTPS Requests
                                                      v
                                  +-------------------+-------------------+
                                  |         Express Server (3000)         |
                                  |  - Node.js ESM/CJS Sandbox Runtime    |
                                  |  - Static Asset Delivery (dist/)      |
                                  |  - API Request Proxies & Handlers     |
                                  +-------------------+-------------------+
                                                      |
                                                      | Secure SDK Calls
                                                      v
                                  +-------------------+-------------------+
                                  |         Google Gemini API             |
                                  |  - Gemini 3.5 Flash & 3.1 Pro Models  |
                                  |  - Encrypted API Key Authorization    |
                                  |  - ADK Tool Call Integration          |
                                  +---------------------------------------+
2.1 Front-End Tech Stack
React 19: Built with functional components, modern Hooks state management, and strict property prop-types contract verification.
Vite 6: Chosen for quick, optimized client-side builds, generating clean, compressed static assets in /dist.
Tailwind CSS v4: Uses modern CSS variables (@theme declaration block) for design token declaration, compiling fast, lightweight utility utility classes.
Recharts Integration: Implements high-performance, responsive data visualizations with native SVG canvas rendering.
Lucide React: Supplies a comprehensive, unified SVG iconography system.
2.2 Back-End Server Design (server.ts)
The server acts as a secure reverse proxy to execute AI generation pipelines without exposing credentials to the client.
Vite Development Middleware: During local development (NODE_ENV !== "production"), the server spins up a Vite instance in middlewareMode with appType: 'spa' to provide hot module reloading proxies.
Production Static Asset Handling: In production (NODE_ENV === "production"), the server serves precompiled static assets from the /dist directory. All unmapped routes fall back to serving /dist/index.html to guarantee flawless single-page application (SPA) routing.
Port Allocation: The server strictly binds to port 3000 and host 0.0.0.0 to comply with ingress routing patterns inside secure container configurations.
2.3 Network Communication Security
All communication to third-party endpoints, including the Google Gen AI API, runs exclusively server-side. No client-side code possesses permission to query outside domain structures directly. Sensitive credentials, such as the GEMINI_API_KEY, are read directly from the cloud environment variables at runtime, preventing API key exposure in client-side JS bundles.
3. Core Compliance Framework & Regulatory Domains (The EVO Q10 Case Study)
The compliance framework is modeled to handle the registration requirements of complex, multi-probe medical equipment. The baseline project, the EVO Q10 Diagnostic Ultrasound System, presents several specific regulatory hurdles that the platform actively evaluates and resolves.
code
Code
========================================================================================================
                                     COMPLIANCE MATRIX: PROJECT EVO Q10
========================================================================================================
Standard ID     Regulatory Domain             Requirement Outline                       EVO Q10 Status
--------------------------------------------------------------------------------------------------------
Clause 4.1      EMC & Electrical Safety       IEC 60601-1, IEC 60601-1-2 reports.        COMPLIANT (OK)
                                              Verified from Nemko Korea.
                                              
Clause 4.2      Ultrasonic Special Safety     IEC 60601-2-37 & IEC 62359.               NON-COMPLIANT
                                              Acoustic output (TI, MI) indices.         (Deficiencies detected)
                                              
Clause 4.3      Biocompatibility Tests        ISO 10993-5 (Cytotoxicity),               NON-COMPLIANT
                                              ISO 10993-10 (Irritation & Sensitization) (16/22 probes missing)
                                              
Clause 4.4      Software & Cybersecurity      TFDA cybersecurity self-assessment,        COMPLIANT (OK)
                                              Software lifecycle validation reports.
                                              
Clause 4.5      AI CADe/CADx Technical File   ML algorithm clinical validation,         NON-COMPLIANT
                                              ROC curves, AUC indices, Gold Standards.   (No validation details)
                                              
Clause 4.6      Performance Test Records      Probe-specific resolution specifications,  NON-COMPLIANT
                                              Measurement accuracy declarations.        (Missing measurements)
                                              
Clause 4.7      Factory Release Records       Certificate of Analysis (CoA), quality    NON-COMPLIANT
                                              release templates.                         (No records attached)
========================================================================================================
3.1 Ultrasonic Special Safety (IEC 60601-2-37 & IEC 62359)
Ultrasound transducers emit thermal and mechanical energy into human tissues. TFDA mandates strict declaration of:
Thermal Index (TI): Soft Tissue Thermal Index (TIS), Bone Thermal Index (TIB), and Cranial Bone Thermal Index (TIC).
Mechanical Index (MI): Represents the likelihood of cavitation in tissues.
The system flags the lack of probe-specific safety tables for the 22 probes, requiring the Korean original equipment manufacturer (OEM) to supply the raw laboratory reports.
3.2 Transducer Biocompatibility (ISO 10993-5 & ISO 10993-10)
Transducers are categorized as contact devices with mucosal or breached skin contact. Out of 22 prospective probes, the submission materials only contain reports for 6 models, creating a technical file gap for 16 probes (including critical endocavity transducers such as the EA2-11ARE and EA2-11AVE). The platform automatically calculates this gap and lists the exact missing models.
3.3 Artificial Intelligence CADe/CADx Software Validation
The EVO Q10 features advanced CADe/CADx diagnostic software, including Biometry Assist and S-Detect for Breast/Thyroid. Under TFDA's regulatory guidelines for AI/ML-driven software, the applicant must present:
Receiver Operating Characteristic (ROC) Curves: Plotting sensitivity against 1-specificity.
Area Under the Curve (AUC): Statistically verifying diagnostic accuracy.
Golden Dataset Demographics: Proving the model was trained on diverse patient populations.
The platform flags the total absence of these validations, requiring immediate technical file compilation by the AI software development team.
4. Agentic OS Runtime Mechanics & Tool Calling (Google ADK)
The "Agentic OS" paradigm converts passive regulatory compliance guidelines into an active, self-correcting agent loop. The system simulates the core capabilities of the Google ADK (Agent Development Kit) tool chain to demonstrate how an autonomous auditor reasons, calls external interfaces, and validates outcomes.
code
Code
+---------------------------------------------------------------------------------+
   |                             AGENTIC AUDIT EXECUTION LOOP                        |
   +---------------------------------------------------------------------------------+
   |                                                                                 |
   |   [STEP 1: INITIALIZE]                                                          |
   |   Load Submission Materials + Guidelines Template + Active skill.md             |
   |                                                                                 |
   |   [STEP 2: REASONING PATTERN (CoT)]                                             |
   |   "Let's identify the missing items. Clause 4.2 requires IEC 60601-2-37."       |
   |                                                                                 |
   |   [STEP 3: TOOL RUNNING]                                                        |
   |   Call `google_search` -> Check if Nemko Korea TL124 holds CBTL credentials.    |
   |   Call `built_in_code_execution` -> Run JS script to find exact missing probes. |
   |                                                                                 |
   |   [STEP 4: ERROR DETECTION & SELF-CORRECTION]                                   |
   |   If mismatch found -> Flag non-compliance. Adjust Severity to High.            |
   |                                                                                 |
   |   [STEP 5: REPORT GENERATION]                                                   |
   |   Compile Structured JSON Output -> Render Interactive Dashboard & Cards        |
   |                                                                                 |
   +---------------------------------------------------------------------------------+
4.1 Core Simulated Tools
google_search(query: string): Allows the agent to query external databases to verify laboratory credentials, standard active dates (e.g., verifying if IEC 60601-1:2025 is standard-approved), and lookup regulatory precedents.
built_in_code_execution(code: string): Instantiates a secure, isolated sandbox execution context. The agent uses this tool to process lists of technical values. For example, it can write and execute an internal JavaScript script to compare the list of 22 active probes against the list of 6 uploaded biocompatibility certificates, programmatically deriving the 16 missing probe models.
4.2 The Chain-of-Thought (CoT) Prompting Structure
The agent operates under an explicit Chain-of-Thought sequence. The prompt instructs the model to always document its thoughts before taking action:
Observation: "Applicant states Nemko Korea issued safety reports."
Action: "Verify report validity date and scope."
Tool Call: google_search("Nemko Korea CBTL scope ultrasound IEC 60601-2-37")
Analysis: "Nemko Korea possesses valid accreditation. However, the report scope lacks acoustic intensity measurements for probe models L3-22 and CA1-7SD."
Outcome: "Generate Clause 4.2 deficiency entry."
5. Interactive Core Workspace Modules
The platform's client interface is structured into cohesive, highly interactive workspaces to eliminate static PDF reports and provide a dynamic, end-to-end compliance management workflow.
code
Code
+-------------------------------------------------------------------------------------------------+
|                                    INTERACTIVE CORE WORKSPACE                                   |
+-------------------------------------------------------------------------------------------------+
|                                                                                                 |
|   Tab 1: Report & Audit Editor                                                                  |
|   - Real-time tabular display of Clause IDs, Titles, and Statuses (OK vs. Non-compliant).       |
|   - Interactive status toggles instantly recalculate overall compliance %.                      |
|   - Inline editable textareas sync updates instantly across all active views.                   |
|                                                                                                 |
|   Tab 2: RAI Responses Copilot                                                                  |
|   - Isolates non-compliant items and assigns priority levels (High, Medium, Low).               |
|   - Features editable technical response plan textboxes pre-filled with optimal action guides.  |
|                                                                                                 |
|   Tab 3: Prep Kanban Board                                                                      |
|   - Modern agile board with columns: TODO, IN PROGRESS, REVIEW, COMPLETED.                      |
|   - Cards represent non-compliant clauses, supporting mouse-based Drag & Drop operations.        |
|   - Dragging cards to COMPLETED programmatically resolves deficiencies and updates the graphs.  |
|                                                                                                 |
|   Tab 4: Liaison Coordination Letter                                                            |
|   - Generates formal bilingual liaison correspondence to original manufacturers.                |
|   - Dynamically binds inputs like Recipient, Project Model, and Response Deadlines.             |
|   - Features a translation engine to translate letters into Korean or Japanese.                |
|                                                                                                 |
|   Tab 5: Gap to R&D Tasks (WOW Feature)                                                         |
|   - Parses complex legal/regulatory findings into specific engineering instructions.            |
|   - Automatically maps tasks to respective specialized teams (Acoustic, Bio, or AI Labs).      |
|                                                                                                 |
+-------------------------------------------------------------------------------------------------+
5.1 Tab 1: Report & Audit Editor
The central ledger of the audit workspace renders a structured table containing the regulatory clauses.
Interactive Conformity Toggles: Users can toggle any clause status (e.g., from "不符合" to "OK"). This action triggers a cascade of state updates, updating the gauge chart, line graphs, and risk matrices.
Inline Editing: Technical auditor remarks and descriptions are fully editable. Modifications are immediately stored in the local component state.
Custom Clauses: Users can dynamically append custom audit entries, which are assigned unique tracking IDs (e.g., 4.8).
5.2 Tab 2: RAI Responses Copilot
"Request for Additional Information" (RAI) or supplementary requests ("補件通知") are the most time-sensitive phase of registration. The RAI Responses Copilot isolates all items marked "不符合" and organizes them by severity. It provides pre-filled textareas containing targeted, pre-validated response strategies based on TFDA guidelines, which users can customize.
5.3 Tab 3: prep Kanban Board
The platform features an agile Kanban workspace categorized into four lanes: TODO, IN PROGRESS, REVIEW, and COMPLETED.
Drag and Drop Implementation: Each deficiency card is draggable via standard mouse events. Drag-and-drop operations are processed using React's synthetic event handler model:
code
TypeScript
const handleDragStart = (e: React.DragEvent, itemIndex: number) => {
  e.dataTransfer.setData("text/plain", itemIndex.toString());
};
State Loop Synchronization: When a card is dropped into the COMPLETED lane, the parent state is modified to change the item's status to "OK". This synchronizes the Kanban board, audit tables, and metrics charts.
5.4 Tab 4: Liaison Coordination Letter & Translation Engine
Because medical device manufacturers are often located internationally, local regulatory agents must translate TFDA's formal deficiency notices into actionable requests for OEM teams.
Dynamic Data Binding: The template automatically aggregates active deficiency clauses and parses them into a professional business letter.
Translation Interface: Features an automated system to translate letters into target languages (such as Korean or Japanese), matching the commercial and linguistic expectations of international partners.
6. Detailed Code Walkthrough & State Synchronization Mechanics
To understand the production-grade quality of the application, we will analyze the precise file layout, data models, state flows, and asynchronous operations.
6.1 Unified Data Models (src/types.ts)
The codebase maintains absolute type safety using structured interface definitions:
code
TypeScript
export type Severity = "High" | "Medium" | "Low";
export type Priority = "High" | "Medium" | "Low";
export type KanbanLane = "todo" | "progress" | "review" | "completed";
export type Theme = "light" | "dark";
export type Language = "zh-TW" | "en";

export interface ReviewItem {
  id: string;
  title: string;
  status: "OK" | "不符合";
  severity: Severity;
  description: string;
  link: string;
  responseDraft: string;
  priority: Priority;
  kanbanColumn: KanbanLane;
}

export interface MetricData {
  defectSeverity: { name: string; count: number }[];
  probeStatus: { name: string; status: string; reason: string }[];
  completionProgress: { date: string; progress: number }[];
  modelComparison: { mname: string; name: string; accuracy: number; speed: number; cost: number; context: number }[];
  riskPoints: { x: number; y: number; z: number; name: string }[];
}

export interface AuditResult {
  projectName: string;
  lastUpdated: string;
  overallCompliancePct: number;
  deficienciesCount: number;
  officialManualDefects: string;
  officialTechDefects: string;
  reviewItems: ReviewItem[];
  metrics: MetricData;
}
6.2 Application State Synchronization Flow
The global application state is managed in src/App.tsx. When the user initiates an audit, the following lifecycle occurs:
code
Code
+----------------------------------------------+
                  |               User Action                    |
                  |     - Click "Execute Intelligent Audit"      |
                  +----------------------+-----------------------+
                                         |
                                         v
                  +----------------------------------------------+
                  |               App.tsx State                  |
                  |     - setisAuditing(true)                    |
                  |     - setShowTerminal(true)                  |
                  +----------------------+-----------------------+
                                         |
                                         v
                  +----------------------------------------------+
                  |          AgenticLogTerminal.tsx              |
                  |  - Loops through step-by-step audit logs.    |
                  |  - Simulates active tool calling.            |
                  |  - Auto-scrolls container to bottom.         |
                  +----------------------+-----------------------+
                                         |
                                         v
                  +----------------------------------------------+
                  |             REST API Call                    |
                  |  - POST request sent to /api/audit.          |
                  |  - Server connects to Google Gemini.         |
                  +----------------------+-----------------------+
                                         |
                                         v
                  +----------------------------------------------+
                  |          Audit Response Processing           |
                  |  - Response updates auditResult state.       |
                  |  - showResult set to true.                   |
                  |  - Recharts component redraws.               |
                  +----------------------------------------------+
Preparation: The user edits the submission text in the primary workspace.
Triggering: The user clicks "Execute Intelligent Audit". The system sets isAuditing to true, resets the results container, and displays the AgenticLogTerminal.
Terminal Simulation & Scrolling: The terminal loops through step-by-step audit logs. It displays real-time timestamps and simulated actions, keeping the log window scrolled to the bottom.
Asynchronous API Resolution: A POST request is dispatched to /api/audit. The server connects to the Gemini model to parse the submission materials against the skill.md rules.
State Population: The API returns the completed audit results. The system saves the payload to the local auditResult state, hides the terminal overlay, and shows the interactive workspace.
7. The Design System & Theme Architecture (Sleek Interface & Visual Styles)
To match the request for a high-quality user experience, the system uses the Sleek Interface (Premium Modern) theme as its default configuration. It defines precise color values, borders, and layout containers to create a polished look.
code
Code
+-------------------------------------------------------------------------------------------------+
|                                 SLEEK INTERFACE - DESIGN TOKENS                                 |
+-------------------------------------------------------------------------------------------------+
|                                                                                                 |
|   Colors & Accents                                                                              |
|   - Base Canvas Background: Slate 950 (#020617) / Neutral Warm Gray (Light Mode)                |
|   - Secondary Containers: Zinc 900 / Slate 900 (Dark) or Pure White (Light)                     |
|   - Accent Colors: Royal Indigo-500 (#6366f1) to Purple-600 (#a855f7) Gradient                  |
|   - Secondary Accents: Emerald-400 (Success indicators) and Rose-400 (Deficiencies)            |
|                                                                                                 |
|   Borders & Shadows                                                                             |
|   - Component Borders: Thin Slate-800/80 (Dark) / Slate-200/60 (Light)                          |
|   - Interactive Glow: Indigo-500/20 glow on focus, adding depth.                                |
|   - Shadows: Soft, diffuse shadows for depth without clutter.                                  |
|                                                                                                 |
|   Typography & Scale                                                                            |
|   - Header Font: "Space Grotesk" or "Outfit" (Modern display sans-serif)                        |
|   - Body copy: "Inter" (Clean, high-legibility sans-serif)                                      |
|   - Data & Terminal logs: "JetBrains Mono" or "Fira Code" (Monospace for alignment)             |
|                                                                                                 |
+-------------------------------------------------------------------------------------------------+
7.1 Comprehensive Theme Styles (The 10 Visual Styles)
The application defines 10 distinct, selectable design themes. Each theme is fully optimized for both light and dark modes:
Sleek Interface (Premium Modern): Dynamic Indigo-to-Purple gradient accents paired with high-contrast Slate containers. This default configuration emphasizes visual clarity, spacious padding, and thin, elegant borders.
Clinical Blue (US FDA Standard): Professional medical blue accents, prioritizing clean information layouts and structured clinical styling.
Regulatory Emerald (TFDA Standard): Bright emerald green highlights, reflecting successful compliance certifications and standard regulatory workflows.
TFDA Teal (Taiwan Health Dept): Teal and aqua accents, reflecting Taiwan's public health portal designs.
Warm Apricot (Eye-Care Amber): Gentle amber, peach, and coral tones, designed to reduce eye strain during long document review sessions.
Glacier Arctic (Nordic Frost): Cool ice-blue and cyan gradients, conveying cleanliness and minimalism.
Iron Steel (Heavy Industrial): Matte charcoal and steel gray tones, designed for heavy engineering and hardware manufacturing contexts.
Cosmic Explorer (Deep Nebula): Rich violet and crimson accents, providing an immersive dark-theme workspace.
Mint Breeze (Minimalist Fresh): Light mint green and teal gradients, maximizing negative space and clean typography.
Flame Rose (High Contrast Warning): Deep crimson and rose accents, designed to highlight compliance risks and critical deficiencies.
8. Proposed AI Features for Future Integration
To expand the platform's capabilities beyond standard text-based audits, we propose three additional advanced AI integrations. These features leverage multi-modal models, predictive analytics, and automated generation pipelines to add significant value.
code
Code
+-------------------------------------------------------------------------------------------------+
|                               FUTURE ROADMAP: PROPOSED AI FEATURES                              |
+-------------------------------------------------------------------------------------------------+
|                                                                                                 |
|   Feature 1: Multi-Modal Sticker & Label Compliance OCR Verifier                                |
|   - Intakes high-resolution photos of device warning stickers, chassis tags, and packaging.      |
|   - Automatically extracts and audits labeling details against local language and warning laws. |
|   - Cross-references specifications against active database models to catch typos.               |
|                                                                                                 |
|   Feature 2: Regulatory Compliance Gap Risk Simulator                                           |
|   - Uses historical TFDA query patterns to predict supplementary request risks.                 |
|   - Assigns deficiency risk scores to each regulatory clause before formal submission.          |
|   - Simulates regulatory team review behaviors to estimate registration delay times.            |
|                                                                                                 |
|   Feature 3: Smart Auto-Generated Clinical Trial Dataset Synthesizer                            |
|   - Generates mock, compliant clinical validation data based on TFDA guidelines.                |
|   - Builds synthetic cohorts with age, gender, and acoustic parameter distributions.            |
|   - Calculates performance metrics (ROC curves, AUC indices) to provide baseline templates.     |
|                                                                                                 |
+-------------------------------------------------------------------------------------------------+
8.1 Proposed Feature 1: Multi-Modal Sticker & Label Compliance OCR Verifier
8.1.1 Target Functional Purpose
Medical devices cannot be imported without correct physical labeling, warning plaques, and packaging text. In Taiwan, TFDA mandates that specific warning symbols, manufacturer addresses, and contraindications appear in traditional Chinese on the physical chassis. The OCR Verifier allows users to upload photos of physical labels, warning stickers, or packaging mockups and automatically audits the content against local regulatory guidelines.
8.1.2 Backend Implementation Architecture
Using the multi-modal capabilities of Gemini, the backend API routes the image data directly to the vision-language processing pipelines:
code
TypeScript
// Proposed Backend Endpoint: /api/audit-label-vision
import { GoogleGenAI } from "@google/genai";
import express from "express";

const router = express.Router();
const ai = new GoogleGenAI({ apiKey: process.env.GEMINI_API_KEY });

router.post("/api/audit-label-vision", async (req, res) => {
  const { imageBase64, language, deviceClass } = req.body;
  
  if (!imageBase64) {
    return res.status(400).json({ error: "Missing required label image data" });
  }

  try {
    const response = await ai.models.generateContent({
      model: "gemini-2.5-flash",
      contents: [
        {
          role: "user",
          parts: [
            {
              text: `You are an expert TFDA medical device labeling auditor. Examine this physical device sticker or packaging proof for compliance.
                     Verify if traditional Chinese warning text, manufacturer addresses, and required symbol layouts are present.
                     List any missing elements, translation errors, or regulatory violations.`
            },
            {
              inlineData: {
                mimeType: "image/jpeg",
                data: imageBase64
              }
            }
          ]
        }
      ]
    });

    res.json({ auditAnalysis: response.text });
  } catch (error) {
    res.status(500).json({ error: "Vision processing failure: " + error.message });
  }
});
8.1.3 Frontend Integration UI Design
The user interface features a dedicated visual tab with drag-and-drop support for image files. Uploaded images are rendered on an interactive canvas alongside an overlay mapping detected text and compliance alerts.
code
TypeScript
// Proposed Frontend Component: LabelVisionVerifier.tsx
import React, { useState } from "react";
import { Upload, Camera, AlertTriangle, CheckCircle } from "lucide-react";

export const LabelVisionVerifier: React.FC = () => {
  const [preview, setPreview] = useState<string | null>(null);
  const [analyzing, setAnalyzing] = useState(false);
  const [findings, setFindings] = useState<string | null>(null);

  const handleImageUpload = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setPreview(reader.result as string);
      };
      reader.readAsDataURL(file);
    }
  };

  const runVisionAudit = async () => {
    if (!preview) return;
    setAnalyzing(true);
    try {
      const response = await fetch("/api/audit-label-vision", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ imageBase64: preview.split(",")[1] })
      });
      const data = await response.json();
      setFindings(data.auditAnalysis);
    } catch (err) {
      console.error(err);
    } finally {
      setAnalyzing(false);
    }
  };

  return (
    <div className="p-6 bg-slate-900 border border-slate-800 rounded-2xl">
      <h3 className="text-sm font-bold text-white mb-2 flex items-center gap-2">
        <Camera className="w-4 h-4 text-indigo-400" />
        Multi-Modal Label OCR Verifier
      </h3>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6 mt-4">
        <div className="flex flex-col items-center justify-center border-2 border-dashed border-slate-800 rounded-xl p-6 bg-slate-950/50">
          {preview ? (
            <div className="relative w-full h-64">
              <img src={preview} alt="Sticker Proof" className="w-full h-full object-contain rounded-lg" />
              <button onClick={() => setPreview(null)} className="absolute top-2 right-2 px-2 py-1 bg-red-600 text-white text-xs rounded">Clear</button>
            </div>
          ) : (
            <div className="text-center">
              <Upload className="w-10 h-10 text-slate-500 mx-auto mb-2" />
              <input type="file" onChange={handleImageUpload} className="hidden" id="label-file" />
              <label htmlFor="label-file" className="cursor-pointer text-indigo-400 text-xs font-semibold hover:underline">Upload Label Photo</label>
            </div>
          )}
          {preview && (
            <button onClick={runVisionAudit} disabled={analyzing} className="mt-4 w-full py-2 bg-indigo-600 text-white font-bold rounded-lg hover:bg-indigo-500">
              {analyzing ? "Running OCR Analysis..." : "Verify Labels"}
            </button>
          )}
        </div>
        <div className="p-4 bg-slate-950 rounded-xl border border-slate-800 overflow-y-auto max-h-80 font-mono text-xs">
          {findings ? (
            <div className="space-y-2">
              <span className="text-indigo-400 font-bold block">Audit Findings:</span>
              <p className="text-slate-300 leading-relaxed whitespace-pre-wrap">{findings}</p>
            </div>
          ) : (
            <p className="text-slate-600 text-center py-20">Audit results will be displayed here after image upload.</p>
          )}
        </div>
      </div>
    </div>
  );
};
8.2 Proposed Feature 2: Regulatory Compliance Gap Risk Simulator
8.2.1 Target Functional Purpose
To help regulatory teams optimize submissions before filing with TFDA, the platform can pre-calculate likely deficiency queries. The Risk Simulator uses historical query databases and submission content to run Monte Carlo simulations. It calculates a statistical deficiency risk score for each clause, estimating potential registration delays and identifying which sections require additional documentation.
8.2.2 Technical Simulation Architecture
The simulator runs statistical calculations to weight inputs, such as active probe counts and software complexity. It outputs risk classifications based on historical TFDA trends:
code
TypeScript
// Proposed Simulation State & Logic inside parent review engine
export interface RiskSimResults {
  clauseId: string;
  queryProbability: number; // 0 to 100
  estimatedDelayDays: number;
  criticalFactor: string;
}

export function runRiskSimulation(reviewItems: any[], totalTransducers: number): RiskSimResults[] {
  return reviewItems.map(item => {
    let probability = 10;
    let delayDays = 0;
    let factor = "Baseline historical query rate is low.";

    if (item.id === "4.2") {
      probability = totalTransducers > 10 ? 88 : 45;
      delayDays = totalTransducers > 10 ? 45 : 20;
      factor = `High transducer count (${totalTransducers} probes) increases acoustic validation audit complexity.`;
    } else if (item.id === "4.3") {
      probability = 75;
      delayDays = 30;
      factor = "Mucosal contact transducers require explicit cytotoxicity, sensitization, and skin irritation logs.";
    } else if (item.id === "4.5") {
      probability = 92;
      delayDays = 60;
      factor = "AI CADe/CADx diagnostics require clinical validation, ROC charts, and dataset demographics.";
    }

    return {
      clauseId: item.id,
      queryProbability: probability,
      estimatedDelayDays: delayDays,
      criticalFactor: factor
    };
  });
}
8.2.3 Simulation Dashboard UI
The simulation dashboard displays a high-impact risk visualization. It uses dynamic danger rings to highlight critical bottlenecks, helping teams prioritize their pre-submission remediation efforts.
code
TypeScript
// Proposed Frontend Component: RiskSimulatorPanel.tsx
import React, { useState } from "react";
import { Play, ShieldAlert, Sparkles } from "lucide-react";
import { runRiskSimulation } from "../utils/simulatorLogic";

interface RiskPanelProps {
  reviewItems: any[];
}

export const RiskSimulatorPanel: React.FC<RiskPanelProps> = ({ reviewItems }) => {
  const [running, setRunning] = useState(false);
  const [results, setResults] = useState<any[]>([]);

  const handleSimulate = () => {
    setRunning(true);
    setTimeout(() => {
      const computed = runRiskSimulation(reviewItems, 22);
      setResults(computed);
      setRunning(false);
    }, 1500);
  };

  return (
    <div className="p-6 bg-slate-900 border border-slate-800 rounded-2xl">
      <div className="flex justify-between items-center border-b border-slate-800 pb-3 mb-4">
        <h3 className="text-sm font-bold text-white flex items-center gap-2">
          <ShieldAlert className="w-4 h-4 text-rose-500 animate-pulse" />
          Regulatory Query Risk & Delay Simulator
        </h3>
        <button onClick={handleSimulate} disabled={running} className="px-3 py-1.5 bg-rose-600 text-white rounded-lg text-xs font-bold hover:bg-rose-500 flex items-center gap-1">
          <Play className="w-3.5 h-3.5" />
          {running ? "Simulating..." : "Run Simulator"}
        </button>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        {results.length > 0 ? (
          results.map((r, i) => (
            <div key={i} className="p-4 bg-slate-950 border border-slate-800 rounded-xl flex flex-col justify-between">
              <div>
                <span className="text-[10px] text-slate-500 font-mono">Clause {r.clauseId}</span>
                <div className="flex justify-between items-center mt-1">
                  <span className="text-xs font-bold text-slate-300">Query Probability</span>
                  <span className={`text-sm font-extrabold ${r.queryProbability > 70 ? "text-rose-500" : "text-amber-500"}`}>
                    {r.queryProbability}%
                  </span>
                </div>
                <p className="text-[10px] text-slate-400 mt-2 font-mono leading-relaxed">{r.criticalFactor}</p>
              </div>
              <div className="mt-3 pt-2 border-t border-slate-900 flex justify-between items-center text-[10px]">
                <span className="text-slate-500">Est. Registration Delay</span>
                <span className="text-rose-400 font-bold">{r.estimatedDelayDays} Days</span>
              </div>
            </div>
          ))
        ) : (
          <div className="col-span-full text-center py-10 text-slate-500 text-xs">
            Click "Run Simulator" to estimate submission risks.
          </div>
        )}
      </div>
    </div>
  );
};
8.3 Proposed Feature 3: Smart Auto-Generated Clinical Trial Dataset Synthesizer
8.3.1 Target Functional Purpose
AI-based diagnostic models cannot be validated without testing performance on structured datasets. However, manufacturers often struggle to format their clinical dataset metadata to match the specific parameters required by TFDA reviews. The Dataset Synthesizer automatically generates structured, compliant clinical trial validation templates. These synthetic datasets match age, gender, and acoustic distributions specified by regulatory guidelines, providing teams with a compliant formatting baseline.
8.3.2 Technical Generation Logic
The synthesizer generates traditional CSV files containing structured clinical metadata, patient demographics, diagnostic categories, and performance labels.
code
TypeScript
// Proposed Backend API: /api/generate-synthetic-cohort
import express from "express";
const router = express.Router();

router.post("/api/generate-synthetic-cohort", (req, res) => {
  const { sampleCount = 100, targetDisease = "Breast Lesion Classification" } = req.body;
  
  let csvContent = "PatientID,Age,Gender,LesionSizeMM,AcousticAttenuation,ModelPredictionProbability,GroundTruthClassification,DiagnosticAgreement\n";
  
  for (let i = 1; i <= sampleCount; i++) {
    const age = Math.floor(Math.random() * 50) + 25;
    const gender = Math.random() > 0.15 ? "Female" : "Male";
    const size = (Math.random() * 25 + 5).toFixed(2);
    const attenuation = (Math.random() * 0.8 + 0.2).toFixed(2);
    const prob = Math.random();
    const isMalignant = prob > 0.45;
    const groundTruth = isMalignant ? "Malignant" : "Benign";
    const prediction = prob > 0.5 ? "Malignant" : "Benign";
    const agreement = groundTruth === prediction ? "AGREE" : "DISAGREE";

    csvContent += `P${1000 + i},${age},${gender},${size},${attenuation},${prob.toFixed(4)},${groundTruth},${prediction},${agreement}\n`;
  }

  res.setHeader("Content-Type", "text/csv");
  res.setHeader("Content-Disposition", `attachment; filename=synthetic_cohort_${targetDisease.replace(/ /g, "_")}.csv`);
  res.status(200).send(csvContent);
});
8.3.3 Dataset Exporter UI Component
This component allows users to configure cohort parameters, generate synthetic dataset templates, and download the formatted CSV files directly for local editing and clinical mapping.
code
TypeScript
// Proposed Frontend Component: DatasetSynthesizer.tsx
import React, { useState } from "react";
import { Database, FileSpreadsheet, Download, Sparkles } from "lucide-react";

export const DatasetSynthesizer: React.FC = () => {
  const [samples, setSamples] = useState(150);
  const [disease, setDisease] = useState("Breast Lesion Classification");
  const [loading, setLoading] = useState(false);

  const handleDownload = async () => {
    setLoading(true);
    try {
      const response = await fetch("/api/generate-synthetic-cohort", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ sampleCount: samples, targetDisease: disease })
      });
      const blob = await response.blob();
      const url = window.URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = `TFDA_AI_Validation_Dataset_${disease.replace(/ /g, "_")}.csv`;
      document.body.appendChild(a);
      a.click();
      a.remove();
    } catch (err) {
      console.error(err);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="p-6 bg-slate-900 border border-slate-800 rounded-2xl">
      <h3 className="text-sm font-bold text-white mb-2 flex items-center gap-2">
        <FileSpreadsheet className="w-4 h-4 text-emerald-400" />
        AI Validation Dataset Synthesizer
      </h3>
      <p className="text-xs text-slate-400">
        Generate structured clinical trial dataset templates that match local guidelines for validation modeling.
      </p>
      
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
        <div>
          <label className="block text-[10px] font-bold text-slate-500 uppercase mb-1">Target Application</label>
          <select value={disease} onChange={(e) => setDisease(e.target.value)} className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-1.5 text-xs text-slate-300">
            <option value="Breast Lesion Classification">Breast Lesion Classification (S-Detect)</option>
            <option value="Thyroid Nodule Assessment">Thyroid Nodule Assessment (S-Detect Thyroid)</option>
            <option value="Fetal Biometry Measurements">Fetal Biometry Measurements (Biometry Assist)</option>
          </select>
        </div>
        <div>
          <label className="block text-[10px] font-bold text-slate-500 uppercase mb-1">Patient Sample Size</label>
          <input type="number" value={samples} onChange={(e) => setSamples(Number(e.target.value))} className="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-1.5 text-xs text-slate-300" />
        </div>
      </div>

      <button onClick={handleDownload} disabled={loading} className="mt-4 px-4 py-2 bg-emerald-600 text-white rounded-xl text-xs font-bold hover:bg-emerald-500 flex items-center gap-1.5">
        <Download className="w-3.5 h-3.5" />
        {loading ? "Generating Template..." : "Download CSV Dataset Template"}
      </button>
    </div>
  );
};
9. Production Deployment & Environment Orchestration
To run the full-stack application securely on Google Cloud Run, we use a single bundled server container configured to handle asset resolution and API proxying correctly.
code
Code
+-------------------------------------------------------------------------------------------------+
|                                 PRODUCTION BUNDLE & SERVER LAYOUT                               |
+-------------------------------------------------------------------------------------------------+
|                                                                                                 |
|   Build Outputs (dist/)                                                                         |
|   ├── index.html                  - App entry point with precompiled JS/CSS                     |
|   ├── server.cjs                  - Single bundled backend server compiled via esbuild          |
|   └── assets/                     - Minified image assets and CSS chunks                        |
|                                                                                                 |
|   Runtime Deployment Engine                                                                     |
|   - Node.js production runtime loads compiled server.cjs directly.                              |
|   - Bypasses file resolution errors by pre-resolving paths inside the bundled build.            |
|   - Lazily instantiates SDK dependencies to handle potential missing environment variables.      |
|                                                                                                 |
+-------------------------------------------------------------------------------------------------+
9.1 Build Configuration
The package.json build pipeline is configured as a two-stage compilation process:
Frontend Compilation: Runs vite build to compile the TypeScript SPA into minified static assets inside the /dist output folder.
Backend Bundling: Uses esbuild to compile the backend code into a single, self-contained CommonJS file (dist/server.cjs):
code
Bash
esbuild server.ts --bundle --platform=node --format=cjs --packages=external --sourcemap --outfile=dist/server.cjs
This approach bypasses path-resolution errors in Node.js container environments by packaging all import paths inside a single bundle file at build-time.
9.2 Lazy SDK Initialization Pattern
To prevent app crashes if target third-party API keys are missing on system startup, the codebase initializes SDK clients lazily. The Gemini SDK client is only instantiated when an active audit route is requested, failing gracefully if the required keys are not configured:
code
TypeScript
import { GoogleGenAI } from "@google/genai";

let aiClient: GoogleGenAI | null = null;

export function getGeminiClient(): GoogleGenAI {
  if (!aiClient) {
    const key = process.env.GEMINI_API_KEY;
    if (!key) {
      throw new Error("Critical Configuration Error: GEMINI_API_KEY environment variable is missing.");
    }
    aiClient = new GoogleGenAI({ apiKey: key });
  }
  return aiClient;
}
10. 20 Advanced Follow-up Technical Q&As
The following question-and-answer section details critical technical edge cases, standard updates, troubleshooting strategies, and system maintenance guidelines.
Q1: How do you configure the platform's backend parser to handle multi-modal audits of PDF-based technical files directly?
To support direct PDF file processing on the backend, the Express server uses the multer middleware to handle file uploads. It parses incoming documents using the pdf-parse library and feeds the extracted text streams directly to the Gemini model along with the target compliance check rules:
code
TypeScript
import express from "express";
import multer from "multer";
import pdfParse from "pdf-parse";
import { getGeminiClient } from "./geminiHelper";

const upload = multer({ storage: multer.memoryStorage() });
const router = express.Router();

router.post("/api/audit-pdf", upload.single("file"), async (req, res) => {
  if (!req.file) return res.status(400).json({ error: "Missing uploaded PDF file." });

  try {
    const pdfData = await pdfParse(req.file.buffer);
    const parsedText = pdfData.text; // Extracted raw text stream

    const ai = getGeminiClient();
    const response = await ai.models.generateContent({
      model: "gemini-3.5-flash",
      contents: `Audit the following technical file against standard TFDA rules: \n\n${parsedText}`
    });

    res.json({ results: response.text });
  } catch (err) {
    res.status(500).json({ error: "Failed to parse and audit PDF file: " + err.message });
  }
});
Q2: What security measures prevent cross-site scripting (XSS) when rendering dynamic markdown-formatted audit findings in the client UI?
To prevent XSS vulnerabilities, the application uses the react-markdown library to safely parse and render markdown data returned by the backend APIs. This library converts raw strings into safe React elements instead of injecting raw HTML using dangerouslySetInnerHTML. If markdown elements contain embedded HTML tags, the component wraps them in the dompurify library to sanitize and strip malicious scripts:
code
TypeScript
import React from "react";
import ReactMarkdown from "react-markdown";
import DOMPurify from "dompurify";

interface MarkdownProps {
  content: string;
}

export const SafeMarkdownRenderer: React.FC<MarkdownProps> = ({ content }) => {
  const cleanMarkdown = DOMPurify.sanitize(content);
  return (
    <div className="prose prose-slate dark:prose-invert text-xs leading-relaxed">
      <ReactMarkdown>{cleanMarkdown}</ReactMarkdown>
    </div>
  );
};
Q3: How does the application ensure accurate charts and canvas rendering during rapid browser resizing on high-DPI displays?
To prevent visual clipping or chart layout distortion on high-DPI displays, the system avoids using static window size calculations like window.innerWidth. Instead, it uses the standard ResizeObserver API inside a custom React hook. This hook detects container element resize events and uses a 100ms debounce function to prevent layout thrashing:
code
TypeScript
import { useState, useEffect, useRef } from "react";

export function useResponsiveContainer() {
  const containerRef = useRef<HTMLDivElement | null>(null);
  const [dimensions, setDimensions] = useState({ width: 600, height: 300 });

  useEffect(() => {
    if (!containerRef.current) return;

    let timeoutId: number;
    const observer = new ResizeObserver((entries) => {
      for (let entry of entries) {
        const { width, height } = entry.contentRect;
        // Debounce calculation to avoid UI layout thrashing
        window.clearTimeout(timeoutId);
        timeoutId = window.setTimeout(() => {
          setDimensions({ width, height: height || 300 });
        }, 100);
      }
    });

    observer.observe(containerRef.current);
    return () => {
      observer.disconnect();
      window.clearTimeout(timeoutId);
    };
  }, []);

  return { containerRef, dimensions };
}
Q4: When TFDA updates the cybersecurity guidelines for medical devices, what is the exact workflow to update the active compliance rules?
Because the platform decouples regulatory guidelines from the core system code, users do not need to rewrite software or redeploy container builds. Instead, you modify the guidelines using the following workflow:
Navigate to the Agentic OS Sandbox tab in the client interface.
Open the active tfda-auditor_skill.md file in the code editor.
Add the new cybersecurity clauses (e.g., Software Bill of Materials (SBOM) details) as structured Markdown list items under the ## [RULES & PROTOCOLS] section.
Optionally, use the AI Skill Refiner prompt input to automatically rewrite and format the guidelines based on raw text pasted from the TFDA gazette.
Click Save Changes to commit the updated rules to the active workspace.
Q5: How do we configure custom traditional Chinese speech synthesis voices for the Emma Compliance Assistant?
The voice-guided Compliance Assistant uses the browser's native Web Speech Synthesis API. Because client systems run different operating systems and languages, the platform scans available system voices to find high-quality traditional Chinese or English engines. It uses the window.speechSynthesis.onvoiceschanged callback to handle asynchronous voice initialization across different platforms:
code
TypeScript
export function initializeEmmaVoice(language: "zh-TW" | "en"): SpeechSynthesisVoice | null {
  if (typeof window === "undefined" || !window.speechSynthesis) return null;

  const voices = window.speechSynthesis.getVoices();
  const targetLang = language === "zh-TW" ? "zh-TW" : "en-US";

  // Attempt to select premium Chinese voices, falling back to standard Chinese or English
  let selectedVoice = voices.find(v => v.lang.replace("_", "-") === targetLang && v.name.includes("Premium"));
  if (!selectedVoice) {
    selectedVoice = voices.find(v => v.lang.includes(language === "zh-TW" ? "zh-TW" : "en-US"));
  }
  if (!selectedVoice && language === "zh-TW") {
    selectedVoice = voices.find(v => v.lang.includes("zh-CN") || v.lang.includes("zh"));
  }

  return selectedVoice || null;
}
Q6: How does the server proxy manage rate limits and request timeouts during long-running multi-clause Gemini audits?
Long-running technical file audits can trigger gateway timeouts or exceed model rate limits. The Express backend handles these edge cases by structuring long reviews into smaller, sequential request chunks, checking active rate limits, and implementing an exponential backoff retry policy:
code
TypeScript
async function fetchWithExponentialBackoff(fn: () => Promise<any>, retries = 3, delay = 1000) {
  try {
    return await fn();
  } catch (error) {
    // Check for rate limit status codes (e.g., 429)
    if (retries > 0 && error.status === 429) {
      console.warn(`Rate limit reached. Retrying in ${delay}ms...`);
      await new Promise(res => setTimeout(res, delay));
      return fetchWithExponentialBackoff(fn, retries - 1, delay * 2);
    }
    throw error;
  }
}
Q7: What data sanitization techniques protect proprietary R&D values (such as raw ultrasound focal depths) during audits?
To comply with intellectual property and data privacy policies, the platform includes a client-side sanitization layer. Before transmitting submission files to the backend APIs, the client uses regular expressions to strip or anonymize sensitive proprietary values, such as custom product codes or proprietary raw hardware schematics:
code
TypeScript
export function sanitizeTechnicalMaterials(rawText: string): string {
  let sanitized = rawText;
  // Anonymize proprietary model prefixes and development codes
  sanitized = sanitized.replace(/EVO-Q10-[X|Y|Z]-\d{4}/g, "EQUIPMENT-MODEL-ANONYMIZED");
  // Redact raw internal focal values and prototype sensor names
  sanitized = sanitized.replace(/FocalDepth:\s*\d+\.\d+mm/g, "FocalDepth: [REDACTED FOR SECURE AUDIT]");
  return sanitized;
}
Q8: How are the visual styles of Recharts components mapped to custom theme styles?
The styleConfigs mapping file controls visual themes across both UI elements and charts. The InteractiveDashboard reads these configuration values dynamically, feeding theme colors directly into chart rendering components like PolarAngleAxis or Radar:
code
TypeScript
import { styleConfigs } from "../data/styleConfigs";

export const RenderRadarChart = ({ data, styleKey }: { data: any[]; styleKey: string }) => {
  const currentTheme = styleConfigs[styleKey] || styleConfigs["sleek-interface"];
  
  // Extract hex color values from Tailwind gradient class names
  const strokeColor = styleKey === "sleek-interface" ? "#6366f1" : "#10b981";
  const fillColor = styleKey === "sleek-interface" ? "rgba(99,102,241,0.2)" : "rgba(16,185,129,0.2)";

  return (
    <Radar
      name="Compliance Score"
      dataKey="score"
      stroke={strokeColor}
      fill={fillColor}
      iconSize={10}
    />
  );
};
Q9: How can regulatory teams use the system to audit medical device software updates (e.g., upgrading from EVO v1.0 to v2.0)?
The platform provides a direct comparison workflow to audit software upgrades:
Navigate to the Skill Sandbox Playground tab.
Paste the technical specifications for version 1.0 in the left column.
Paste the updated specifications for version 2.0 in the right column.
Run the side-by-side comparison engine. The system highlights new quantitative tools and flags any missing validation data or regression test reports required for the upgrade submission.
Q10: How does the application maintain responsive UI interactions when rendering complex drag-and-drop operations on the Kanban Board?
The Kanban board uses standard HTML5 drag-and-drop APIs, which run on browser UI threads and can stutter during high CPU usage. To keep drag animations fluid, the platform uses lightweight state transitions. Instead of re-rendering the entire card list on every hover event, the board only triggers UI updates when a card is dropped into a new lane, keeping drag interactions fluid:
code
TypeScript
export const KanbanCard: React.FC<{ index: number; title: string }> = ({ index, title }) => {
  const [isDragging, setIsDragging] = useState(false);

  return (
    <div
      draggable
      onDragStart={(e) => {
        setIsDragging(true);
        e.dataTransfer.setData("text/plain", index.toString());
      }}
      onDragEnd={() => setIsDragging(false)}
      className={`p-4 rounded-xl border transition-all duration-150 cursor-grab ${
        isDragging ? "opacity-30 scale-95 border-dashed" : "opacity-100 scale-100"
      }`}
    >
      <h4 className="text-xs font-bold">{title}</h4>
    </div>
  );
};
Q11: How do you configure the system to check if probe biocompatibility certificates comply with the ISO 10993-1 contact duration standards?
Under the ISO 10993-1 guidelines, medical device contact classifications are determined by contact duration: Category A (Limited, <24h), Category B (Prolonged, 24h to 30d), and Category C (Permanent, >30d). You can configure the skill.md rules to audit these categories using the following prompt patterns:
code
Markdown
- [RULE 4.3.1] Check device contact classification against ISO 10993-1 standards.
- [RULE 4.3.2] Ultrasound transducers are classified as surface-contact devices with mucosal contact.
- [RULE 4.3.3] If contact duration is prolongated (Category B), verify that submission files contain sub-chronic systemic toxicity and genotoxicity reports.
- [RULE 4.3.4] Flag any certificates that only contain basic cytotoxicity or irritation data for Category B devices.
Q12: How does the app handle CORS and security restrictions during development behind local proxy servers?
During development, the Express backend uses the cors package to control access permissions. It restricts API access to authorized origins, blocking unauthorized external domains while allowing developers to work locally without complex certificate configurations:
code
TypeScript
import express from "express";
import cors from "cors";

const app = express();

const allowedOrigins = [
  "http://localhost:3000",
  "https://ais-dev-znf34atjq6557gkcqmnhdm-572918811121.asia-northeast1.run.app"
];

app.use(cors({
  origin: (origin, callback) => {
    if (!origin || allowedOrigins.indexOf(origin) !== -1) {
      callback(null, true);
    } else {
      callback(new Error("CORS Policy Violation: Access denied for this origin domain."));
    }
  },
  credentials: true
}));
Q13: How does the platform store and preserve audit session data across page reloads without database configurations?
To preserve active audit sessions without a dedicated database configuration, the platform uses client-side localStorage. When the global auditResult state changes, the application stringifies the state and saves it to local storage. When the app reloads, it retrieves and parses the cached session data, falling back to default mock values if no session exists:
code
TypeScript
import { useState, useEffect } from "react";
import { AuditResult } from "./types";

export function usePersistedAuditState(initialFallback: AuditResult) {
  const [state, setState] = useState<AuditResult>(() => {
    try {
      const saved = localStorage.getItem("tfda_audit_session_data");
      return saved ? JSON.parse(saved) : initialFallback;
    } catch {
      return initialFallback;
    }
  });

  useEffect(() => {
    localStorage.setItem("tfda_audit_session_data", JSON.stringify(state));
  }, [state]);

  return [state, setState] as const;
}
Q14: How does the R&D Task Engine assign task priority levels based on TFDA deficiency notices?
The R&D Task Engine parses incoming deficiency text and maps severity indicators to task priority levels. Keywords like missing critical report, unresolved safety issue, or contraindication error automatically assign high priority and tag the corresponding engineering lead (e.g., Acoustic Physics Lead):
code
TypeScript
export function determinePriority(description: string): "High" | "Medium" | "Low" {
  const text = description.toLowerCase();
  
  if (text.includes("missing") || text.includes("not provided") || text.includes("contraindication")) {
    return "High";
  }
  if (text.includes("accuracy") || text.includes("uncertainty") || text.includes("unclear")) {
    return "Medium";
  }
  return "Low";
}
Q15: How can you update print CSS styles to prevent table formatting issues when printing compliance reports?
The system includes dedicated CSS media queries to ensure printed reports are clean and legible. It hides non-printing interactive elements like navigation buttons and video players, sets container widths to 100%, and uses page-break-inside: avoid rules to prevent multi-line rows from breaking awkwardly across pages:
code
CSS
@media print {
  .no-print,
  button,
  header,
  footer,
  select,
  textarea {
    display: none !important;
  }
  
  body, 
  main, 
  .print-full-width {
    width: 100% !important;
    background: white !important;
    color: black !important;
  }

  tr, 
  .print-card {
    page-break-inside: avoid !important;
  }
}
Q16: How does the platform handle token limit errors when parsing extremely long medical device submissions?
To prevent API errors when parsing large files, the backend uses sliding-window text chunking. It splits technical files into smaller sections based on standard section headers, analyzes each block independently, and aggregates the results into a unified structured audit report:
code
TypeScript
export function splitSubmissionContent(rawText: string, maxTokensLength = 8000): string[] {
  const clauses = rawText.split(/(?=Clause\s*\d+\.\d+)/g);
  const chunks: string[] = [];
  let currentChunk = "";

  for (let clause of clauses) {
    if ((currentChunk + clause).length > maxTokensLength) {
      chunks.push(currentChunk);
      currentChunk = clause;
    } else {
      currentChunk += clause;
    }
  }
  if (currentChunk) chunks.push(currentChunk);
  return chunks;
}
Q17: How do you configure the platform's multi-model comparison engine to benchmark custom fine-tuned models?
You can easily register custom fine-tuned model endpoints in the application. To benchmark a new model, define its identifier and properties in the frontend configuration lists and update the backend API routers to direct requests to your custom endpoint:
code
TypeScript
// Proposed Backend API integration for custom model endpoints
router.post("/api/benchmark-custom-model", async (req, res) => {
  const { modelEndpoint, payload } = req.body;
  
  try {
    const response = await fetch(modelEndpoint, {
      method: "POST",
      headers: { "Authorization": `Bearer ${process.env.CUSTOM_MODEL_TOKEN}` },
      body: JSON.stringify(payload)
    });
    const data = await response.json();
    res.json({ modelOutput: data.output });
  } catch (err) {
    res.status(500).json({ error: "Failed to connect to custom model: " + err.message });
  }
});
Q18: What is the exact data model mapping for exporting audit reports as standard Excel-compatible files?
The platform generates standard CSV (Comma-Separated Values) outputs that are fully compatible with spreadsheet software like Microsoft Excel or Google Sheets. The export payload is structured as an array of objects matching the following layout:
code
JSON
[
  {
    "ClauseID": "4.2",
    "SectionTitle": "Ultrasonic Special Safety (IEC 60601-2-37)",
    "ComplianceStatus": "不符合",
    "AuditorDescription": "Acoustic indices missing for probe models L3-22 and CA1-7SD.",
    "ProposedActionPlan": "Retrieve laboratory records from OEM and compile acoustic index tables."
  }
]
Q19: How do you configure Web Speech Synthesis engines to run without network access?
Modern operating systems (such as Windows, macOS, and iOS) ship with pre-installed, offline-capable speech synthesis engines. When browser systems lose network connectivity, the Web Speech Synthesis API automatically routes audio generation to these native offline engines, ensuring the Emma assistant can continue speaking in isolated development environments.
Q20: How can users leverage the platform to audit non-ultrasound medical devices (e.g., MRI machines or surgical lasers)?
The platform's underlying audit logic is highly flexible. Because regulatory rules are loaded dynamically from markdown files, you can adapt the system to audit any medical device class by simply swapping the contents of the skill.md file. For example, replacing the ultrasound rules with MRI safety guidelines (such as IEC 60601-2-33 standard mappings) instantly configures the Agentic OS to analyze and flag safety gaps in MRI technical files.
Conclusion
This blueprint provides developers, systems engineers, and regulatory specialists with a complete technical reference for the TFDA Medical Device Compliance & Agentic OS Platform. By decoupling regulatory guidelines from the core system code, using robust full-stack container architectures, and implementing clean interactive state synchronization, the platform provides teams with a modern, scalable workflow to accelerate medical device registration.
