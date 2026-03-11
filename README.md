import { useState, useEffect, useRef } from "react";

const portfolioData = {
  projects: [
    {
      id: "01",
      title: "Dynamic File Processing Pipeline",
      subtitle: "ADLS Automation",
      color: "#00D4FF",
      accent: "#0066FF",
      icon: "⚡",
      scenarios: 8,
      description: "Automates ingestion and processing of files from Azure Data Lake Storage with dynamic naming, pattern filtering, and merge capabilities.",
      tags: ["Wildcard Filters", "ForEach Loop", "Parameters", "ADLS Gen2"],
      scenarios_list: ["Dynamic file naming", "Copy latest N files", "Merge by pattern", "Multi-folder ingestion"]
    },
    {
      id: "02",
      title: "Data Lake Metadata Monitoring",
      subtitle: "Lake Intelligence",
      color: "#00FFB2",
      accent: "#00A86B",
      icon: "🔍",
      scenarios: 5,
      description: "Monitor Data Lake structure, detect empty folders, capture file metadata, and ingest structural data into SQL for operational insights.",
      tags: ["Get Metadata", "If Condition", "SQL Sink", "Control Flow"],
      scenarios_list: ["Load folder metadata to SQL", "Detect empty folders", "Valid date folder detection", "Write filenames to CSV"]
    },
    {
      id: "03",
      title: "Conditional File Processing",
      subtitle: "Smart Orchestration",
      color: "#FFB800",
      accent: "#FF6B00",
      icon: "🎯",
      scenarios: 3,
      description: "SQL control tables drive pipeline execution — files are processed only when business conditions are satisfied, enabling controlled data workflows.",
      tags: ["Lookup Activity", "Control Table", "If Condition", "Dynamic SQL"],
      scenarios_list: ["Status-driven copy", "Multi-file conditional load", "ADLS-to-SQL matching"]
    },
    {
      id: "04",
      title: "Pipeline Monitoring & Logging",
      subtitle: "Observability Layer",
      color: "#FF4D6D",
      accent: "#C9184A",
      icon: "📊",
      scenarios: 6,
      description: "Full observability stack for ADF pipelines — execution logging, error capture, email alerts, and queryable run history via Log Analytics.",
      tags: ["Stored Procedures", "Log Analytics", "Email Alerts", "Error Handling"],
      scenarios_list: ["Execution logging", "DataFlow error capture", "Failure email alerts", "Pipeline run history"]
    },
    {
      id: "05",
      title: "API Integration Pipeline",
      subtitle: "External Connectivity",
      color: "#A855F7",
      accent: "#7C3AED",
      icon: "🔗",
      scenarios: 3,
      description: "Integrate ADF with external systems using REST APIs and OAuth2 — trigger pipelines programmatically and connect to secured third-party endpoints.",
      tags: ["REST API", "OAuth2", "Web Activity", "Secure Auth"],
      scenarios_list: ["Pipeline-to-pipeline REST trigger", "API-based execution", "OAuth2 auth flow"]
    },
    {
      id: "06",
      title: "Control Flow Automation",
      subtitle: "Advanced Logic",
      color: "#FF7043",
      accent: "#E64A19",
      icon: "⚙️",
      scenarios: 10,
      description: "Advanced expression logic and iteration patterns — array manipulation, date operations, loop control, and dynamic pipeline behavior.",
      tags: ["Expression Builder", "ForEach", "Arrays", "Date Logic"],
      scenarios_list: ["Sort & reverse arrays", "Break ForEach loop", "Date formatting", "Even/odd validation"]
    }
  ],
  fundamentals: [
    "Copy Data Activity", "Wildcard Filters", "Schema Mapping", "Fault Tolerance",
    "Session Logging", "Parameters vs Variables", "Compression", "Auto-Create Sink",
    "Partition Discovery", "Metadata Activity", "Delete After Move", "Copy Behaviour",
    "Monitor Activity", "List of Files", "Last Modified Filter", "Additional Columns",
    "Skip Line Count", "Max Rows Per File", "Custom Metadata", "Pipeline Architecture"
  ]
};

const Pipeline = ({ active }) => {
  const steps = ["Source", "ADF", "ADLS", "SQL", "Analytics"];
  return (
    <div style={{ display: "flex", alignItems: "center", gap: "0", justifyContent: "center", padding: "12px 0" }}>
      {steps.map((step, i) => (
        <div key={step} style={{ display: "flex", alignItems: "center" }}>
          <div style={{
            padding: "6px 14px",
            background: active ? "rgba(0,212,255,0.15)" : "rgba(255,255,255,0.05)",
            border: `1px solid ${active ? "rgba(0,212,255,0.5)" : "rgba(255,255,255,0.1)"}`,
            borderRadius: "4px",
            fontSize: "11px",
            fontFamily: "'Space Mono', monospace",
            color: active ? "#00D4FF" : "rgba(255,255,255,0.4)",
            transition: "all 0.5s ease",
            transitionDelay: `${i * 0.1}s`,
            letterSpacing: "0.05em"
          }}>
            {step}
          </div>
          {i < steps.length - 1 && (
            <div style={{
              width: "30px",
              height: "1px",
              background: active ? "linear-gradient(90deg, rgba(0,212,255,0.8), rgba(0,102,255,0.8))" : "rgba(255,255,255,0.1)",
              position: "relative",
              transition: "all 0.5s ease",
              transitionDelay: `${i * 0.1 + 0.05}s`
            }}>
              <div style={{
                position: "absolute",
                right: "-3px",
                top: "-3px",
                width: "6px",
                height: "6px",
                borderTop: `1px solid ${active ? "rgba(0,212,255,0.8)" : "rgba(255,255,255,0.1)"}`,
                borderRight: `1px solid ${active ? "rgba(0,212,255,0.8)" : "rgba(255,255,255,0.1)"}`,
                transform: "rotate(45deg)"
              }} />
            </div>
          )}
        </div>
      ))}
    </div>
  );
};

const ProjectCard = ({ project, index }) => {
  const [hovered, setHovered] = useState(false);
  const [visible, setVisible] = useState(false);
  const ref = useRef();

  useEffect(() => {
    const obs = new IntersectionObserver(([e]) => { if (e.isIntersecting) setVisible(true); }, { threshold: 0.1 });
    if (ref.current) obs.observe(ref.current);
    return () => obs.disconnect();
  }, []);

  return (
    <div ref={ref} onMouseEnter={() => setHovered(true)} onMouseLeave={() => setHovered(false)}
      style={{
        background: hovered ? `linear-gradient(135deg, rgba(255,255,255,0.06) 0%, rgba(${project.color === '#00D4FF' ? '0,212,255' : project.color === '#00FFB2' ? '0,255,178' : project.color === '#FFB800' ? '255,184,0' : project.color === '#FF4D6D' ? '255,77,109' : project.color === '#A855F7' ? '168,85,247' : '255,112,67'},0.08) 100%)` : "rgba(255,255,255,0.02)",
        border: `1px solid ${hovered ? project.color + "40" : "rgba(255,255,255,0.07)"}`,
        borderRadius: "12px",
        padding: "28px",
        cursor: "pointer",
        transition: "all 0.4s cubic-bezier(0.23, 1, 0.32, 1)",
        transform: visible ? (hovered ? "translateY(-4px)" : "translateY(0)") : "translateY(24px)",
        opacity: visible ? 1 : 0,
        transitionDelay: `${index * 0.08}s`,
        position: "relative",
        overflow: "hidden"
      }}>
      <div style={{
        position: "absolute", top: 0, left: 0, right: 0, height: "2px",
        background: `linear-gradient(90deg, transparent, ${project.color}, transparent)`,
        opacity: hovered ? 1 : 0, transition: "opacity 0.3s"
      }} />

      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: "16px" }}>
        <div>
          <div style={{ fontFamily: "'Space Mono', monospace", fontSize: "11px", color: project.color, letterSpacing: "0.15em", marginBottom: "4px", opacity: 0.8 }}>
            PROJECT_{project.id}
          </div>
          <h3 style={{ fontSize: "17px", fontFamily: "'Syne', sans-serif", fontWeight: 700, color: "#fff", margin: 0, lineHeight: 1.2 }}>
            {project.title}
          </h3>
          <div style={{ fontSize: "11px", color: "rgba(255,255,255,0.4)", marginTop: "3px", fontFamily: "'Space Mono', monospace" }}>
            {project.subtitle}
          </div>
        </div>
        <div style={{
          width: "44px", height: "44px", borderRadius: "10px",
          background: `${project.color}15`, border: `1px solid ${project.color}30`,
          display: "flex", alignItems: "center", justifyContent: "center", fontSize: "20px",
          flexShrink: 0
        }}>
          {project.icon}
        </div>
      </div>

      <p style={{ fontSize: "13px", color: "rgba(255,255,255,0.5)", lineHeight: 1.7, margin: "0 0 18px", fontFamily: "'DM Sans', sans-serif" }}>
        {project.description}
      </p>

      <div style={{ display: "flex", flexWrap: "wrap", gap: "6px", marginBottom: "18px" }}>
        {project.tags.map(tag => (
          <span key={tag} style={{
            padding: "3px 10px", borderRadius: "4px", fontSize: "10px",
            fontFamily: "'Space Mono', monospace", letterSpacing: "0.05em",
            background: `${project.color}12`, color: project.color,
            border: `1px solid ${project.color}25`
          }}>{tag}</span>
        ))}
      </div>

      <div style={{ borderTop: "1px solid rgba(255,255,255,0.06)", paddingTop: "14px" }}>
        <div style={{ fontSize: "10px", fontFamily: "'Space Mono', monospace", color: "rgba(255,255,255,0.3)", marginBottom: "8px", letterSpacing: "0.1em" }}>
          SCENARIOS INCLUDED
        </div>
        {project.scenarios_list.map((s, i) => (
          <div key={i} style={{ fontSize: "12px", color: "rgba(255,255,255,0.45)", padding: "3px 0", display: "flex", alignItems: "center", gap: "8px", fontFamily: "'DM Sans', sans-serif" }}>
            <div style={{ width: "4px", height: "4px", borderRadius: "50%", background: project.color, opacity: 0.6, flexShrink: 0 }} />
            {s}
          </div>
        ))}
      </div>
    </div>
  );
};

export default function App() {
  const [activeSection, setActiveSection] = useState("overview");
  const [pipelineActive, setPipelineActive] = useState(false);

  useEffect(() => {
    const t = setTimeout(() => setPipelineActive(true), 800);
    return () => clearTimeout(t);
  }, []);

  const totalScenarios = portfolioData.projects.reduce((a, p) => a + p.scenarios, 0);

  return (
    <div style={{
      minHeight: "100vh",
      background: "#080B14",
      color: "#fff",
      fontFamily: "'DM Sans', sans-serif"
    }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Space+Mono:wght@400;700&family=DM+Sans:wght@300;400;500&display=swap');
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { background: #080B14; }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #080B14; }
        ::-webkit-scrollbar-thumb { background: #1a2040; border-radius: 2px; }
        @keyframes pulse { 0%,100% { opacity:1 } 50% { opacity:0.4 } }
        @keyframes flow { 0% { transform: translateX(-100%) } 100% { transform: translateX(200%) } }
        @keyframes fadeUp { from { opacity:0; transform:translateY(30px) } to { opacity:1; transform:translateY(0) } }
        .stat-card:hover { background: rgba(255,255,255,0.06) !important; }
      `}</style>

      {/* Grid background */}
      <div style={{
        position: "fixed", inset: 0, pointerEvents: "none",
        backgroundImage: `
          linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
          linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px)
        `,
        backgroundSize: "60px 60px"
      }} />

      {/* Glow orbs */}
      <div style={{ position: "fixed", top: "-200px", right: "-200px", width: "600px", height: "600px", borderRadius: "50%", background: "radial-gradient(circle, rgba(0,102,255,0.08) 0%, transparent 70%)", pointerEvents: "none" }} />
      <div style={{ position: "fixed", bottom: "-200px", left: "-200px", width: "500px", height: "500px", borderRadius: "50%", background: "radial-gradient(circle, rgba(0,212,255,0.05) 0%, transparent 70%)", pointerEvents: "none" }} />

      <div style={{ maxWidth: "1100px", margin: "0 auto", padding: "0 24px" }}>

        {/* Header */}
        <header style={{ padding: "40px 0 20px", animation: "fadeUp 0.8s ease both" }}>
          <div style={{ display: "flex", alignItems: "center", gap: "12px", marginBottom: "40px" }}>
            <div style={{ display: "flex", gap: "6px" }}>
              {["#FF4D6D", "#FFB800", "#00D4FF"].map(c => (
                <div key={c} style={{ width: "10px", height: "10px", borderRadius: "50%", background: c, animation: "pulse 2s ease infinite", animationDelay: c === "#FFB800" ? "0.3s" : c === "#00D4FF" ? "0.6s" : "0s" }} />
              ))}
            </div>
            <div style={{ fontFamily: "'Space Mono', monospace", fontSize: "11px", color: "rgba(255,255,255,0.3)", letterSpacing: "0.1em" }}>
              GITHUB.COM / azure-data-factory-pipeline-scenarios
            </div>
          </div>

          <div style={{ display: "grid", gridTemplateColumns: "1fr auto", gap: "40px", alignItems: "start" }}>
            <div>
              <div style={{ fontFamily: "'Space Mono', monospace", fontSize: "11px", color: "#00D4FF", letterSpacing: "0.2em", marginBottom: "12px", opacity: 0.8 }}>
                AZURE DATA ENGINEER PORTFOLIO
              </div>
              <h1 style={{ fontFamily: "'Syne', sans-serif", fontSize: "clamp(32px, 5vw, 52px)", fontWeight: 800, lineHeight: 1.1, marginBottom: "16px" }}>
                Azure Data Factory<br />
                <span style={{ background: "linear-gradient(135deg, #00D4FF, #0066FF)", WebkitBackgroundClip: "text", WebkitTextFillColor: "transparent" }}>
                  Pipeline Scenarios
                </span>
              </h1>
              <p style={{ fontSize: "15px", color: "rgba(255,255,255,0.5)", lineHeight: 1.7, maxWidth: "560px", fontWeight: 300 }}>
                Production-grade ADF pipelines for dynamic file ingestion, metadata monitoring,
                conditional processing, API orchestration, and pipeline observability.
              </p>
            </div>

            <div style={{ display: "flex", flexDirection: "column", gap: "10px", minWidth: "200px" }}>
              {[
                { label: "SCENARIO PROJECTS", value: "6", color: "#00D4FF" },
                { label: "TOTAL SCENARIOS", value: totalScenarios.toString(), color: "#00FFB2" },
                { label: "ADF FUNDAMENTALS", value: "25+", color: "#FFB800" }
              ].map(stat => (
                <div key={stat.label} className="stat-card" style={{
                  background: "rgba(255,255,255,0.03)", border: "1px solid rgba(255,255,255,0.08)",
                  borderRadius: "8px", padding: "12px 16px", transition: "background 0.2s",
                  display: "flex", justifyContent: "space-between", alignItems: "center"
                }}>
                  <span style={{ fontFamily: "'Space Mono', monospace", fontSize: "9px", color: "rgba(255,255,255,0.35)", letterSpacing: "0.1em" }}>{stat.label}</span>
                  <span style={{ fontFamily: "'Syne', sans-serif", fontSize: "22px", fontWeight: 800, color: stat.color }}>{stat.value}</span>
                </div>
              ))}
            </div>
          </div>
        </header>

        {/* Architecture Pipeline */}
        <div style={{
          background: "rgba(255,255,255,0.02)", border: "1px solid rgba(255,255,255,0.07)",
          borderRadius: "10px", padding: "16px 24px", marginBottom: "40px",
          animation: "fadeUp 0.8s ease 0.2s both"
        }}>
          <div style={{ fontFamily: "'Space Mono', monospace", fontSize: "10px", color: "rgba(255,255,255,0.3)", marginBottom: "10px", letterSpacing: "0.1em" }}>
            ARCHITECTURE OVERVIEW
          </div>
          <Pipeline active={pipelineActive} />
        </div>

        {/* Nav */}
        <nav style={{ display: "flex", gap: "4px", marginBottom: "36px", animation: "fadeUp 0.8s ease 0.3s both" }}>
          {["overview", "fundamentals"].map(s => (
            <button key={s} onClick={() => setActiveSection(s)} style={{
              padding: "8px 20px", borderRadius: "6px", border: "none", cursor: "pointer",
              fontFamily: "'Space Mono', monospace", fontSize: "11px", letterSpacing: "0.1em",
              textTransform: "uppercase", transition: "all 0.2s",
              background: activeSection === s ? "rgba(0,212,255,0.15)" : "transparent",
              color: activeSection === s ? "#00D4FF" : "rgba(255,255,255,0.4)",
              outline: activeSection === s ? "1px solid rgba(0,212,255,0.3)" : "1px solid transparent"
            }}>
              {s === "overview" ? "Scenario Projects" : "ADF Fundamentals"}
            </button>
          ))}
        </nav>

        {/* Projects Grid */}
        {activeSection === "overview" && (
          <div style={{
            display: "grid",
            gridTemplateColumns: "repeat(auto-fill, minmax(320px, 1fr))",
            gap: "16px",
            marginBottom: "60px"
          }}>
            {portfolioData.projects.map((project, i) => (
              <ProjectCard key={project.id} project={project} index={i} />
            ))}
          </div>
        )}

        {/* Fundamentals */}
        {activeSection === "fundamentals" && (
          <div style={{ marginBottom: "60px", animation: "fadeUp 0.5s ease both" }}>
            <div style={{ marginBottom: "24px" }}>
              <h2 style={{ fontFamily: "'Syne', sans-serif", fontSize: "22px", fontWeight: 700, marginBottom: "8px" }}>
                ADF Fundamentals Coverage
              </h2>
              <p style={{ color: "rgba(255,255,255,0.4)", fontSize: "13px", fontFamily: "'DM Sans', sans-serif" }}>
                Core ADF concepts demonstrated across scenario projects and dedicated documentation.
              </p>
            </div>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(220px, 1fr))", gap: "10px" }}>
              {portfolioData.fundamentals.map((f, i) => (
                <div key={f} style={{
                  background: "rgba(255,255,255,0.02)", border: "1px solid rgba(255,255,255,0.07)",
                  borderRadius: "8px", padding: "14px 16px",
                  display: "flex", alignItems: "center", gap: "10px",
                  animation: "fadeUp 0.4s ease both",
                  animationDelay: `${i * 0.03}s`,
                  transition: "border-color 0.2s"
                }}
                  onMouseEnter={e => e.currentTarget.style.borderColor = "rgba(0,212,255,0.3)"}
                  onMouseLeave={e => e.currentTarget.style.borderColor = "rgba(255,255,255,0.07)"}
                >
                  <div style={{ width: "6px", height: "6px", borderRadius: "2px", background: "#00D4FF", opacity: 0.7, flexShrink: 0 }} />
                  <span style={{ fontSize: "13px", color: "rgba(255,255,255,0.65)", fontFamily: "'DM Sans', sans-serif" }}>{f}</span>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* Footer */}
        <footer style={{
          borderTop: "1px solid rgba(255,255,255,0.07)",
          padding: "28px 0",
          display: "flex", justifyContent: "space-between", alignItems: "center",
          flexWrap: "wrap", gap: "12px"
        }}>
          <div style={{ fontFamily: "'Space Mono', monospace", fontSize: "11px", color: "rgba(255,255,255,0.25)", letterSpacing: "0.05em" }}>
            azure-data-factory-pipeline-scenarios · Production-style portfolio
          </div>
          <div style={{ display: "flex", gap: "16px" }}>
            {["Azure Data Factory", "ADLS Gen2", "Azure SQL", "Log Analytics"].map(tech => (
              <span key={tech} style={{ fontSize: "11px", fontFamily: "'Space Mono', monospace", color: "rgba(255,255,255,0.2)", letterSpacing: "0.05em" }}>{tech}</span>
            ))}
          </div>
        </footer>
      </div>
    </div>
  );
}
