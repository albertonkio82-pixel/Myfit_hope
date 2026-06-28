import React, { useState, useEffect, useMemo } from "react";
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
  ReferenceLine,
} from "recharts";
import {
  Flame,
  Plus,
  Settings,
  TrendingDown,
  TrendingUp,
  Minus,
  Trash2,
  X,
  Scale,
  Dumbbell,
  CheckCircle2,
  Circle,
  ChevronDown,
  ChevronUp,
} from "lucide-react";

// ---------------------------------------------------------------------------
// Token di stile (palette "energia/metabolismo": carbone caldo + brace)
// ---------------------------------------------------------------------------
const C = {
  bg: "#15140F",
  surface: "#1E1C16",
  surfaceAlt: "#211F18",
  border: "#2C2A22",
  borderSoft: "#252319",
  text: "#F4F0E8",
  textMuted: "#9C9685",
  textFaint: "#6E6A5C",
  ember: "#FF7A30",
  emberSoft: "#3A2618",
  sage: "#8FB996",
  sageSoft: "#202A1F",
  amber: "#E0A23B",
  amberSoft: "#2E2417",
  red: "#E2574C",
  redSoft: "#2E1A18",
};

const ACTIVITY_OPTIONS = [
  { id: "sedentario", label: "Sedentario", hint: "poco o nessun esercizio", mult: 1.2 },
  { id: "leggero", label: "Leggero", hint: "1-3 giorni/sett.", mult: 1.375 },
  { id: "moderato", label: "Moderato", hint: "3-5 giorni/sett.", mult: 1.55 },
  { id: "intenso", label: "Intenso", hint: "6-7 giorni/sett.", mult: 1.725 },
];

const RANGE_OPTIONS = [
  { id: 7, label: "7g" },
  { id: 30, label: "30g" },
  { id: 90, label: "90g" },
  { id: 0, label: "Tutto" },
];

const STORAGE_KEY = "metabolismo-tracker-data";

const WORKOUT_PLAN = [
  {
    id: "A",
    title: "Giorno A — Forza",
    subtitle: "Manubri + parallettes",
    exercises: [
      "Squat con manubri — 3x12",
      "Affondi alternati con manubri — 3x10/lato",
      "Piegamenti sui parallettes — 3x10-15",
      "Rematore curvo con manubri — 3x12",
      "Plank sui parallettes — 3x30-45s",
    ],
  },
  {
    id: "B",
    title: "Giorno B — Circuito metabolico",
    subtitle: "4 giri · 30s lavoro / 15s recupero, 60s tra giri",
    exercises: [
      "Squat con manubri",
      "Mountain climber sui parallettes",
      "Affondi con manubri",
      "Piegamenti sui parallettes",
      "Jumping jack",
    ],
  },
  {
    id: "C",
    title: "Giorno C — Forza 2",
    subtitle: "Manubri + parallettes",
    exercises: [
      "Stacco da terra con manubri — 3x12",
      "Shoulder press con manubri — 3x10",
      "Pike push-up sui parallettes — 3x8-10",
      "Affondi laterali con manubri — 3x10/lato",
      "Russian twist con manubri — 3x20",
    ],
  },
];

function getWeekKey(date = new Date()) {
  const d = new Date(date);
  const day = d.getDay();
  const diffToMonday = day === 0 ? -6 : 1 - day;
  d.setDate(d.getDate() + diffToMonday);
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}-${String(
    d.getDate()
  ).padStart(2, "0")}`;
}

// ---------------------------------------------------------------------------
// Utility di calcolo
// ---------------------------------------------------------------------------
function todayISO() {
  const d = new Date();
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}-${String(
    d.getDate()
  ).padStart(2, "0")}`;
}

function formatDateShort(iso) {
  const [y, m, d] = iso.split("-").map(Number);
  const date = new Date(y, m - 1, d);
  return date.toLocaleDateString("it-IT", { day: "numeric", month: "short" });
}

function calcBMI(weightKg, heightCm) {
  const h = heightCm / 100;
  return weightKg / (h * h);
}

function bmiCategory(bmi) {
  if (bmi < 18.5) return { label: "Sottopeso", color: C.red, soft: C.redSoft };
  if (bmi < 25) return { label: "Normopeso", color: C.sage, soft: C.sageSoft };
  if (bmi < 30) return { label: "Sovrappeso", color: C.amber, soft: C.amberSoft };
  return { label: "Obesità", color: C.red, soft: C.redSoft };
}

function calcBMR(weightKg, heightCm, age, sex) {
  const base = 10 * weightKg + 6.25 * heightCm - 5 * age;
  return sex === "M" ? base + 5 : base - 161;
}

function idealWeightRange(heightCm) {
  const h = heightCm / 100;
  return { min: 18.5 * h * h, max: 24.9 * h * h };
}

function getActivity(id) {
  return ACTIVITY_OPTIONS.find((a) => a.id === id) || ACTIVITY_OPTIONS[2];
}

// ---------------------------------------------------------------------------
// Componente principale
// ---------------------------------------------------------------------------
export default function MetabolismoTracker() {
  const [loading, setLoading] = useState(true);
  const [profile, setProfile] = useState(null);
  const [entries, setEntries] = useState([]);
  const [workouts, setWorkouts] = useState({});
  const [expandedDay, setExpandedDay] = useState(null);
  const [showSettings, setShowSettings] = useState(false);
  const [range, setRange] = useState(30);
  const [weightInput, setWeightInput] = useState("83");
  const [saveError, setSaveError] = useState("");

  // Form di setup / impostazioni
  const [formHeight, setFormHeight] = useState("178");
  const [formAge, setFormAge] = useState("35");
  const [formSex, setFormSex] = useState("M");
  const [formActivity, setFormActivity] = useState("moderato");

  useEffect(() => {
    (async () => {
      try {
        const res = await window.storage.get(STORAGE_KEY, false);
        if (res && res.value) {
          const parsed = JSON.parse(res.value);
          setProfile(parsed.profile || null);
          setEntries(parsed.entries || []);
          setWorkouts(parsed.workouts || {});
          if (parsed.profile) {
            setFormHeight(String(parsed.profile.height));
            setFormAge(String(parsed.profile.age));
            setFormSex(parsed.profile.sex);
            setFormActivity(parsed.profile.activity);
          }
        }
      } catch (e) {
        // nessun dato salvato ancora: si parte da zero
      } finally {
        setLoading(false);
      }
    })();
  }, []);

  async function persist(nextProfile, nextEntries, nextWorkouts) {
    try {
      const ok = await window.storage.set(
        STORAGE_KEY,
        JSON.stringify({ profile: nextProfile, entries: nextEntries, workouts: nextWorkouts }),
        false
      );
      if (!ok) setSaveError("Salvataggio non riuscito, riprova.");
      else setSaveError("");
    } catch (e) {
      setSaveError("Errore di salvataggio, riprova.");
    }
  }

  function handleSaveProfile() {
    const next = {
      height: parseFloat(formHeight),
      age: parseInt(formAge, 10),
      sex: formSex,
      activity: formActivity,
    };
    if (!next.height || !next.age) return;
    setProfile(next);
    setShowSettings(false);
    persist(next, entries, workouts);
  }

  function handleAddEntry() {
    const w = parseFloat(weightInput.replace(",", "."));
    if (!w || w <= 0) return;
    const today = todayISO();
    const existingIdx = entries.findIndex((e) => e.date === today);
    let next;
    if (existingIdx >= 0) {
      next = [...entries];
      next[existingIdx] = { ...next[existingIdx], weight: w };
    } else {
      next = [...entries, { id: `${today}-${Date.now()}`, date: today, weight: w }];
    }
    next.sort((a, b) => (a.date > b.date ? 1 : -1));
    setEntries(next);
    persist(profile, next, workouts);
  }

  function handleDeleteEntry(id) {
    const next = entries.filter((e) => e.id !== id);
    setEntries(next);
    persist(profile, next, workouts);
  }

  function handleReset() {
    if (!window.confirm("Eliminare tutti i dati salvati? L'operazione non è reversibile.")) return;
    setProfile(null);
    setEntries([]);
    setWorkouts({});
    persist(null, [], {});
    setShowSettings(false);
  }

  function toggleWorkoutDay(dayId) {
    const weekKey = getWeekKey();
    const currentWeek = workouts[weekKey] || {};
    const nextWeek = { ...currentWeek, [dayId]: !currentWeek[dayId] };
    const next = { ...workouts, [weekKey]: nextWeek };
    setWorkouts(next);
    persist(profile, entries, next);
  }

  const latest = entries.length ? entries[entries.length - 1] : null;
  const weekAgoEntry = useMemo(() => {
    if (!latest) return null;
    const latestDate = new Date(latest.date);
    let best = null;
    for (const e of entries) {
      const d = new Date(e.date);
      const diffDays = (latestDate - d) / (1000 * 60 * 60 * 24);
      if (diffDays >= 6 && diffDays <= 8) best = e;
    }
    if (!best) {
      // fallback: la voce più vecchia disponibile entro 10 giorni
      best = entries.find((e) => {
        const diffDays = (latestDate - new Date(e.date)) / (1000 * 60 * 60 * 24);
        return diffDays > 0 && diffDays <= 10;
      });
    }
    return best || null;
  }, [entries, latest]);

  const bmi = profile && latest ? calcBMI(latest.weight, profile.height) : null;
  const bmiCat = bmi ? bmiCategory(bmi) : null;
  const ideal = profile ? idealWeightRange(profile.height) : null;
  const bmr = profile && latest ? calcBMR(latest.weight, profile.height, profile.age, profile.sex) : null;
  const activity = profile ? getActivity(profile.activity) : null;
  const tdee = bmr && activity ? bmr * activity.mult : null;
  const deltaWeek =
    latest && weekAgoEntry ? +(latest.weight - weekAgoEntry.weight).toFixed(1) : null;

  const chartData = useMemo(() => {
    let filtered = entries;
    if (range > 0 && entries.length) {
      const lastDate = new Date(entries[entries.length - 1].date);
      filtered = entries.filter((e) => {
        const diff = (lastDate - new Date(e.date)) / (1000 * 60 * 60 * 24);
        return diff <= range;
      });
    }
    return filtered.map((e) => ({ ...e, label: formatDateShort(e.date) }));
  }, [entries, range]);

  // BMR su scala indicativa 1000-2400 kcal per il "gauge" visivo
  const bmrGaugePct = bmr ? Math.min(100, Math.max(4, ((bmr - 1000) / (2400 - 1000)) * 100)) : 0;

  const currentWeekKey = getWeekKey();
  const currentWeekWorkouts = workouts[currentWeekKey] || {};
  const completedCount = WORKOUT_PLAN.filter((d) => currentWeekWorkouts[d.id]).length;

  const fontImport = `
    @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600;700&display=swap');
    * { font-family: 'Inter', system-ui, sans-serif; }
    .num { font-family: 'Space Grotesk', 'Inter', system-ui, sans-serif; font-feature-settings: 'tnum'; }
    .ember-pulse { animation: emberPulse 2.6s ease-in-out infinite; }
    @keyframes emberPulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.72; }
    }
    @media (prefers-reduced-motion: reduce) {
      .ember-pulse { animation: none; }
    }
    input:focus, button:focus-visible { outline: 2px solid ${C.ember}; outline-offset: 2px; }
  `;

  if (loading) {
    return (
      <div
        style={{ background: C.bg, color: C.textMuted, minHeight: "100vh" }}
        className="flex items-center justify-center text-sm"
      >
        <style>{fontImport}</style>
        Caricamento…
      </div>
    );
  }

  // ---- Setup iniziale (nessun profilo salvato) ---------------------------
  if (!profile || showSettings) {
    return (
      <div style={{ background: C.bg, minHeight: "100vh" }} className="flex items-center justify-center p-4">
        <style>{fontImport}</style>
        <div
          style={{ background: C.surface, border: `1px solid ${C.border}` }}
          className="w-full max-w-sm rounded-2xl p-5"
        >
          <div className="flex items-center justify-between mb-1">
            <h1 style={{ color: C.text }} className="text-lg font-semibold">
              {profile ? "Impostazioni" : "Imposta il tuo profilo"}
            </h1>
            {profile && (
              <button onClick={() => setShowSettings(false)} aria-label="Chiudi">
                <X size={20} color={C.textMuted} />
              </button>
            )}
          </div>
          <p style={{ color: C.textFaint }} className="text-xs mb-4">
            Servono per calcolare BMI, metabolismo basale (BMR) e fabbisogno calorico (TDEE).
          </p>

          <div className="flex flex-col gap-3">
            <Field label="Altezza (cm)">
              <input
                type="number"
                value={formHeight}
                onChange={(e) => setFormHeight(e.target.value)}
                style={inputStyle}
              />
            </Field>
            <Field label="Età">
              <input
                type="number"
                value={formAge}
                onChange={(e) => setFormAge(e.target.value)}
                style={inputStyle}
              />
            </Field>
            <Field label="Sesso">
              <div className="flex gap-2">
                {["M", "F"].map((s) => (
                  <button
                    key={s}
                    onClick={() => setFormSex(s)}
                    style={{
                      background: formSex === s ? C.ember : C.surfaceAlt,
                      color: formSex === s ? "#15140F" : C.textMuted,
                      border: `1px solid ${formSex === s ? C.ember : C.border}`,
                    }}
                    className="flex-1 rounded-lg py-2 text-sm font-medium transition-colors"
                  >
                    {s === "M" ? "Uomo" : "Donna"}
                  </button>
                ))}
              </div>
            </Field>
            <Field label="Livello di attività">
              <div className="flex flex-col gap-2">
                {ACTIVITY_OPTIONS.map((a) => (
                  <button
                    key={a.id}
                    onClick={() => setFormActivity(a.id)}
                    style={{
                      background: formActivity === a.id ? C.emberSoft : C.surfaceAlt,
                      border: `1px solid ${formActivity === a.id ? C.ember : C.border}`,
                    }}
                    className="rounded-lg px-3 py-2 text-left transition-colors"
                  >
                    <div style={{ color: C.text }} className="text-sm font-medium">
                      {a.label}
                    </div>
                    <div style={{ color: C.textFaint }} className="text-xs">
                      {a.hint}
                    </div>
                  </button>
                ))}
              </div>
            </Field>
          </div>

          <button
            onClick={handleSaveProfile}
            style={{ background: C.ember, color: "#15140F" }}
            className="w-full rounded-lg py-2.5 mt-5 text-sm font-semibold"
          >
            {profile ? "Salva modifiche" : "Inizia"}
          </button>

          {profile && (
            <button
              onClick={handleReset}
              style={{ color: C.red }}
              className="w-full text-xs mt-3 underline"
            >
              Elimina tutti i dati
            </button>
          )}
        </div>
      </div>
    );
  }

  // ---- Dashboard -----------------------------------------------------------
  return (
    <div style={{ background: C.bg, minHeight: "100vh" }} className="p-4 pb-10">
      <style>{fontImport}</style>
      <div className="max-w-md mx-auto flex flex-col gap-4">
        {/* Header */}
        <div className="flex items-center justify-between">
          <div className="flex items-center gap-2">
            <Flame size={20} color={C.ember} />
            <h1 style={{ color: C.text }} className="text-base font-semibold">
              Energia Metabolica
            </h1>
          </div>
          <button onClick={() => setShowSettings(true)} aria-label="Impostazioni">
            <Settings size={19} color={C.textMuted} />
          </button>
        </div>

        {/* Stat cards: peso + BMI */}
        <div className="grid grid-cols-2 gap-3">
          <Card>
            <div className="flex items-center gap-1.5 mb-1">
              <Scale size={13} color={C.textFaint} />
              <span style={{ color: C.textFaint }} className="text-xs">
                Peso attuale
              </span>
            </div>
            <div className="flex items-baseline gap-1">
              <span className="num" style={{ color: C.text }}>
                <span className="text-2xl font-semibold">{latest ? latest.weight : "—"}</span>
              </span>
              <span style={{ color: C.textFaint }} className="text-xs">
                kg
              </span>
            </div>
            {deltaWeek !== null && (
              <div className="flex items-center gap-1 mt-1">
                {deltaWeek < 0 ? (
                  <TrendingDown size={13} color={C.sage} />
                ) : deltaWeek > 0 ? (
                  <TrendingUp size={13} color={C.red} />
                ) : (
                  <Minus size={13} color={C.textFaint} />
                )}
                <span
                  style={{ color: deltaWeek < 0 ? C.sage : deltaWeek > 0 ? C.red : C.textFaint }}
                  className="text-xs num"
                >
                  {deltaWeek > 0 ? "+" : ""}
                  {deltaWeek} kg / 7g
                </span>
              </div>
            )}
          </Card>

          <Card>
            <span style={{ color: C.textFaint }} className="text-xs">
              BMI
            </span>
            <div className="flex items-baseline gap-1 mt-1">
              <span className="num text-2xl font-semibold" style={{ color: C.text }}>
                {bmi ? bmi.toFixed(1) : "—"}
              </span>
            </div>
            {bmiCat && (
              <span
                style={{ color: bmiCat.color, background: bmiCat.soft }}
                className="inline-block text-xs font-medium rounded-full px-2 py-0.5 mt-1.5"
              >
                {bmiCat.label}
              </span>
            )}
          </Card>
        </div>

        {/* Gauge BMR/TDEE — elemento distintivo */}
        <Card>
          <div className="flex items-center justify-between mb-2">
            <div className="flex items-center gap-1.5">
              <Flame size={14} color={C.ember} className="ember-pulse" />
              <span style={{ color: C.textFaint }} className="text-xs">
                Metabolismo basale (BMR)
              </span>
            </div>
            <span className="num text-lg font-semibold" style={{ color: C.ember }}>
              {bmr ? Math.round(bmr) : "—"}{" "}
              <span style={{ color: C.textFaint }} className="text-xs font-normal">
                kcal/die
              </span>
            </span>
          </div>
          <div style={{ background: C.surfaceAlt, height: 8 }} className="rounded-full overflow-hidden">
            <div
              className="ember-pulse"
              style={{
                width: `${bmrGaugePct}%`,
                background: `linear-gradient(90deg, ${C.amber}, ${C.ember})`,
                height: "100%",
              }}
            />
          </div>
          <div className="flex items-center justify-between mt-3 pt-3" style={{ borderTop: `1px solid ${C.borderSoft}` }}>
            <span style={{ color: C.textFaint }} className="text-xs">
              Fabbisogno totale (TDEE)
            </span>
            <span className="num text-sm font-medium" style={{ color: C.text }}>
              {tdee ? Math.round(tdee) : "—"} kcal/die
            </span>
          </div>
          <div className="flex items-center justify-between mt-1.5">
            <span style={{ color: C.textFaint }} className="text-xs">
              Target per perdere ~0,5 kg/sett.
            </span>
            <span className="num text-sm font-medium" style={{ color: C.sage }}>
              {tdee ? Math.round(tdee - 500) : "—"} kcal/die
            </span>
          </div>
          {ideal && (
            <div className="flex items-center justify-between mt-1.5">
              <span style={{ color: C.textFaint }} className="text-xs">
                Peso ideale stimato
              </span>
              <span className="num text-sm font-medium" style={{ color: C.text }}>
                {i  emberSoft: "#3A2618",
  sage: "#8FB996",
  sageSoft: "#202A1F",
  amber: "#E0A23B",
  amberSoft: "#2E2417",
  red: "#E2574C",
  redSoft: "#2E1A18",
};

const ACTIVITY_OPTIONS = [
  { id: "sedentario", label: "Sedentario", hint: "poco o nessun esercizio", mult: 1.2 },
  { id: "leggero", label: "Leggero", hint: "1-3 giorni/sett.", mult: 1.375 },
  { id: "moderato", label: "Moderato", hint: "3-5 giorni/sett.", mult: 1.55 },
  { id: "intenso", label: "Intenso", hint: "6-7 giorni/sett.", mult: 1.725 },
];

const RANGE_OPTIONS = [
  { id: 7, label: "7g" },
  { id: 30, label: "30g" },
  { id: 90, label: "90g" },
  { id: 0, label: "Tutto" },
];

const STORAGE_KEY = "metabolismo-tracker-data";

// ---------------------------------------------------------------------------
// Utility di calcolo
// ---------------------------------------------------------------------------
function todayISO() {
  const d = new Date();
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}-${String(
    d.getDate()
  ).padStart(2, "0")}`;
}

function formatDateShort(iso) {
  const [y, m, d] = iso.split("-").map(Number);
  const date = new Date(y, m - 1, d);
  return date.toLocaleDateString("it-IT", { day: "numeric", month: "short" });
}

function calcBMI(weightKg, heightCm) {
  const h = heightCm / 100;
  return weightKg / (h * h);
}

function bmiCategory(bmi) {
  if (bmi < 18.5) return { label: "Sottopeso", color: C.red, soft: C.redSoft };
  if (bmi < 25) return { label: "Normopeso", color: C.sage, soft: C.sageSoft };
  if (bmi < 30) return { label: "Sovrappeso", color: C.amber, soft: C.amberSoft };
  return { label: "Obesità", color: C.red, soft: C.redSoft };
}

function calcBMR(weightKg, heightCm, age, sex) {
  const base = 10 * weightKg + 6.25 * heightCm - 5 * age;
  return sex === "M" ? base + 5 : base - 161;
}

function idealWeightRange(heightCm) {
  const h = heightCm / 100;
  return { min: 18.5 * h * h, max: 24.9 * h * h };
}

function getActivity(id) {
  return ACTIVITY_OPTIONS.find((a) => a.id === id) || ACTIVITY_OPTIONS[2];
}

// ---------------------------------------------------------------------------
// Componente principale
// ---------------------------------------------------------------------------
export default function MetabolismoTracker() {
  const [loading, setLoading] = useState(true);
  const [profile, setProfile] = useState(null);
  const [entries, setEntries] = useState([]);
  const [showSettings, setShowSettings] = useState(false);
  const [range, setRange] = useState(30);
  const [weightInput, setWeightInput] = useState("83");
  const [saveError, setSaveError] = useState("");

  // Form di setup / impostazioni
  const [formHeight, setFormHeight] = useState("178");
  const [formAge, setFormAge] = useState("35");
  const [formSex, setFormSex] = useState("M");
  const [formActivity, setFormActivity] = useState("moderato");

  useEffect(() => {
    (async () => {
      try {
        const res = await window.storage.get(STORAGE_KEY, false);
        if (res && res.value) {
          const parsed = JSON.parse(res.value);
          setProfile(parsed.profile || null);
          setEntries(parsed.entries || []);
          if (parsed.profile) {
            setFormHeight(String(parsed.profile.height));
            setFormAge(String(parsed.profile.age));
            setFormSex(parsed.profile.sex);
            setFormActivity(parsed.profile.activity);
          }
        }
      } catch (e) {
        // nessun dato salvato ancora: si parte da zero
      } finally {
        setLoading(false);
      }
    })();
  }, []);

  async function persist(nextProfile, nextEntries) {
    try {
      const ok = await window.storage.set(
        STORAGE_KEY,
        JSON.stringify({ profile: nextProfile, entries: nextEntries }),
        false
      );
      if (!ok) setSaveError("Salvataggio non riuscito, riprova.");
      else setSaveError("");
    } catch (e) {
      setSaveError("Errore di salvataggio, riprova.");
    }
  }

  function handleSaveProfile() {
    const next = {
      height: parseFloat(formHeight),
      age: parseInt(formAge, 10),
      sex: formSex,
      activity: formActivity,
    };
    if (!next.height || !next.age) return;
    setProfile(next);
    setShowSettings(false);
    persist(next, entries);
  }

  function handleAddEntry() {
    const w = parseFloat(weightInput.replace(",", "."));
    if (!w || w <= 0) return;
    const today = todayISO();
    const existingIdx = entries.findIndex((e) => e.date === today);
    let next;
    if (existingIdx >= 0) {
      next = [...entries];
      next[existingIdx] = { ...next[existingIdx], weight: w };
    } else {
      next = [...entries, { id: `${today}-${Date.now()}`, date: today, weight: w }];
    }
    next.sort((a, b) => (a.date > b.date ? 1 : -1));
    setEntries(next);
    persist(profile, next);
  }

  function handleDeleteEntry(id) {
    const next = entries.filter((e) => e.id !== id);
    setEntries(next);
    persist(profile, next);
  }

  function handleReset() {
    if (!window.confirm("Eliminare tutti i dati salvati? L'operazione non è reversibile.")) return;
    setProfile(null);
    setEntries([]);
    persist(null, []);
    setShowSettings(false);
  }

  const latest = entries.length ? entries[entries.length - 1] : null;
  const weekAgoEntry = useMemo(() => {
    if (!latest) return null;
    const latestDate = new Date(latest.date);
    let best = null;
    for (const e of entries) {
      const d = new Date(e.date);
      const diffDays = (latestDate - d) / (1000 * 60 * 60 * 24);
      if (diffDays >= 6 && diffDays <= 8) best = e;
    }
    if (!best) {
      // fallback: la voce più vecchia disponibile entro 10 giorni
      best = entries.find((e) => {
        const diffDays = (latestDate - new Date(e.date)) / (1000 * 60 * 60 * 24);
        return diffDays > 0 && diffDays <= 10;
      });
    }
    return best || null;
  }, [entries, latest]);

  const bmi = profile && latest ? calcBMI(latest.weight, profile.height) : null;
  const bmiCat = bmi ? bmiCategory(bmi) : null;
  const ideal = profile ? idealWeightRange(profile.height) : null;
  const bmr = profile && latest ? calcBMR(latest.weight, profile.height, profile.age, profile.sex) : null;
  const activity = profile ? getActivity(profile.activity) : null;
  const tdee = bmr && activity ? bmr * activity.mult : null;
  const deltaWeek =
    latest && weekAgoEntry ? +(latest.weight - weekAgoEntry.weight).toFixed(1) : null;

  const chartData = useMemo(() => {
    let filtered = entries;
    if (range > 0 && entries.length) {
      const lastDate = new Date(entries[entries.length - 1].date);
      filtered = entries.filter((e) => {
        const diff = (lastDate - new Date(e.date)) / (1000 * 60 * 60 * 24);
        return diff <= range;
      });
    }
    return filtered.map((e) => ({ ...e, label: formatDateShort(e.date) }));
  }, [entries, range]);

  // BMR su scala indicativa 1000-2400 kcal per il "gauge" visivo
  const bmrGaugePct = bmr ? Math.min(100, Math.max(4, ((bmr - 1000) / (2400 - 1000)) * 100)) : 0;

  const fontImport = `
    @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600;700&display=swap');
    * { font-family: 'Inter', system-ui, sans-serif; }
    .num { font-family: 'Space Grotesk', 'Inter', system-ui, sans-serif; font-feature-settings: 'tnum'; }
    .ember-pulse { animation: emberPulse 2.6s ease-in-out infinite; }
    @keyframes emberPulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.72; }
    }
    @media (prefers-reduced-motion: reduce) {
      .ember-pulse { animation: none; }
    }
    input:focus, button:focus-visible { outline: 2px solid ${C.ember}; outline-offset: 2px; }
  `;

  if (loading) {
    return (
      <div
        style={{ background: C.bg, color: C.textMuted, minHeight: "100vh" }}
        className="flex items-center justify-center text-sm"
      >
        <style>{fontImport}</style>
        Caricamento…
      </div>
    );
  }

  // ---- Setup iniziale (nessun profilo salvato) ---------------------------
  if (!profile || showSettings) {
    return (
      <div style={{ background: C.bg, minHeight: "100vh" }} className="flex items-center justify-center p-4">
        <style>{fontImport}</style>
        <div
          style={{ background: C.surface, border: `1px solid ${C.border}` }}
          className="w-full max-w-sm rounded-2xl p-5"
        >
          <div className="flex items-center justify-between mb-1">
            <h1 style={{ color: C.text }} className="text-lg font-semibold">
              {profile ? "Impostazioni" : "Imposta il tuo profilo"}
            </h1>
            {profile && (
              <button onClick={() => setShowSettings(false)} aria-label="Chiudi">
                <X size={20} color={C.textMuted} />
              </button>
            )}
          </div>
          <p style={{ color: C.textFaint }} className="text-xs mb-4">
            Servono per calcolare BMI, metabolismo basale (BMR) e fabbisogno calorico (TDEE).
          </p>

          <div className="flex flex-col gap-3">
            <Field label="Altezza (cm)">
              <input
                type="number"
                value={formHeight}
                onChange={(e) => setFormHeight(e.target.value)}
                style={inputStyle}
              />
            </Field>
            <Field label="Età">
              <input
                type="number"
                value={formAge}
                onChange={(e) => setFormAge(e.target.value)}
                style={inputStyle}
              />
            </Field>
            <Field label="Sesso">
              <div className="flex gap-2">
                {["M", "F"].map((s) => (
                  <button
                    key={s}
                    onClick={() => setFormSex(s)}
                    style={{
                      background: formSex === s ? C.ember : C.surfaceAlt,
                      color: formSex === s ? "#15140F" : C.textMuted,
                      border: `1px solid ${formSex === s ? C.ember : C.border}`,
                    }}
                    className="flex-1 rounded-lg py-2 text-sm font-medium transition-colors"
                  >
                    {s === "M" ? "Uomo" : "Donna"}
                  </button>
                ))}
              </div>
            </Field>
            <Field label="Livello di attività">
              <div className="flex flex-col gap-2">
                {ACTIVITY_OPTIONS.map((a) => (
                  <button
                    key={a.id}
                    onClick={() => setFormActivity(a.id)}
                    style={{
                      background: formActivity === a.id ? C.emberSoft : C.surfaceAlt,
                      border: `1px solid ${formActivity === a.id ? C.ember : C.border}`,
                    }}
                    className="rounded-lg px-3 py-2 text-left transition-colors"
                  >
                    <div style={{ color: C.text }} className="text-sm font-medium">
                      {a.label}
                    </div>
                    <div style={{ color: C.textFaint }} className="text-xs">
                      {a.hint}
                    </div>
                  </button>
                ))}
              </div>
            </Field>
          </div>

          <button
            onClick={handleSaveProfile}
            style={{ background: C.ember, color: "#15140F" }}
            className="w-full rounded-lg py-2.5 mt-5 text-sm font-semibold"
          >
            {profile ? "Salva modifiche" : "Inizia"}
          </button>

          {profile && (
            <button
              onClick={handleReset}
              style={{ color: C.red }}
              className="w-full text-xs mt-3 underline"
            >
              Elimina tutti i dati
            </button>
          )}
        </div>
      </div>
    );
  }

  // ---- Dashboard -----------------------------------------------------------
  return (
    <div style={{ background: C.bg, minHeight: "100vh" }} className="p-4 pb-10">
      <style>{fontImport}</style>
      <div className="max-w-md mx-auto flex flex-col gap-4">
        {/* Header */}
        <div className="flex items-center justify-between">
          <div className="flex items-center gap-2">
            <Flame size={20} color={C.ember} />
            <h1 style={{ color: C.text }} className="text-base font-semibold">
              Energia Metabolica
            </h1>
          </div>
          <button onClick={() => setShowSettings(true)} aria-label="Impostazioni">
            <Settings size={19} color={C.textMuted} />
          </button>
        </div>

        {/* Stat cards: peso + BMI */}
        <div className="grid grid-cols-2 gap-3">
          <Card>
            <div className="flex items-center gap-1.5 mb-1">
              <Scale size={13} color={C.textFaint} />
              <span style={{ color: C.textFaint }} className="text-xs">
                Peso attuale
              </span>
            </div>
            <div className="flex items-baseline gap-1">
              <span className="num" style={{ color: C.text }}>
                <span className="text-2xl font-semibold">{latest ? latest.weight : "—"}</span>
              </span>
              <span style={{ color: C.textFaint }} className="text-xs">
                kg
              </span>
            </div>
            {deltaWeek !== null && (
              <div className="flex items-center gap-1 mt-1">
                {deltaWeek < 0 ? (
                  <TrendingDown size={13} color={C.sage} />
                ) : deltaWeek > 0 ? (
                  <TrendingUp size={13} color={C.red} />
                ) : (
                  <Minus size={13} color={C.textFaint} />
                )}
                <span
                  style={{ color: deltaWeek < 0 ? C.sage : deltaWeek > 0 ? C.red : C.textFaint }}
                  className="text-xs num"
                >
                  {deltaWeek > 0 ? "+" : ""}
                  {deltaWeek} kg / 7g
                </span>
              </div>
            )}
          </Card>

          <Card>
            <span style={{ color: C.textFaint }} className="text-xs">
              BMI
            </span>
            <div className="flex items-baseline gap-1 mt-1">
              <span className="num text-2xl font-semibold" style={{ color: C.text }}>
                {bmi ? bmi.toFixed(1) : "—"}
              </span>
            </div>
            {bmiCat && (
              <span
                style={{ color: bmiCat.color, background: bmiCat.soft }}
                className="inline-block text-xs font-medium rounded-full px-2 py-0.5 mt-1.5"
              >
                {bmiCat.label}
              </span>
            )}
          </Card>
        </div>

        {/* Gauge BMR/TDEE — elemento distintivo */}
        <Card>
          <div className="flex items-center justify-between mb-2">
            <div className="flex items-center gap-1.5">
              <Flame size={14} color={C.ember} className="ember-pulse" />
              <span style={{ color: C.textFaint }} className="text-xs">
                Metabolismo basale (BMR)
              </span>
            </div>
            <span className="num text-lg font-semibold" style={{ color: C.ember }}>
              {bmr ? Math.round(bmr) : "—"}{" "}
              <span style={{ color: C.textFaint }} className="text-xs font-normal">
                kcal/die
              </span>
            </span>
          </div>
          <div style={{ background: C.surfaceAlt, height: 8 }} className="rounded-full overflow-hidden">
            <div
              className="ember-pulse"
              style={{
                width: `${bmrGaugePct}%`,
                background: `linear-gradient(90deg, ${C.amber}, ${C.ember})`,
                height: "100%",
              }}
            />
          </div>
          <div className="flex items-center justify-between mt-3 pt-3" style={{ borderTop: `1px solid ${C.borderSoft}` }}>
            <span style={{ color: C.textFaint }} className="text-xs">
              Fabbisogno totale (TDEE)
            </span>
            <span className="num text-sm font-medium" style={{ color: C.text }}>
              {tdee ? Math.round(tdee) : "—"} kcal/die
            </span>
          </div>
          <div className="flex items-center justify-between mt-1.5">
            <span style={{ color: C.textFaint }} className="text-xs">
              Target per perdere ~0,5 kg/sett.
            </span>
            <span className="num text-sm font-medium" style={{ color: C.sage }}>
              {tdee ? Math.round(tdee - 500) : "—"} kcal/die
            </span>
          </div>
          {ideal && (
            <div className="flex items-center justify-between mt-1.5">
              <span style={{ color: C.textFaint }} className="text-xs">
                Peso ideale stimato
              </span>
              <span className="num text-sm font-medium" style={{ color: C.text }}>
                {ideal.min.toFixed(0)}–{ideal.max.toFixed(0)} kg
              </span>
            </div>
          )}
        </Card>

        {/* Grafico andamento */}
        <Card>
          <div className="flex items-center justify-between mb-2">
            <span style={{ color: C.textFaint }} className="text-xs">
              Andamento peso
            </span>
            <div className="flex gap-1">
              {RANGE_OPTIONS.map((r) => (
                <button
                  key={r.id}
                  onClick={() => setRange(r.id)}
                  style={{
                    background: range === r.id ? C.emberSoft : "transparent",
                    color: range === r.id ? C.ember : C.textFaint,
                    border: `1px solid ${range === r.id ? C.ember : "transparent"}`,
                  }}
                  className="text-xs rounded-md px-1.5 py-0.5"
                >
                  {r.label}
                </button>
              ))}
            </div>
          </div>
          {chartData.length > 1 ? (
            <div style={{ width: "100%", height: 160 }}>
              <ResponsiveContainer>
                <LineChart data={chartData} margin={{ top: 5, right: 8, bottom: 0, left: -20 }}>
                  <CartesianGrid stroke={C.borderSoft} vertical={false} />
                  <XAxis
                    dataKey="label"
                    tick={{ fill: C.textFaint, fontSize: 11 }}
                    axisLine={{ stroke: C.border }}
                    tickLine={false}
                  />
                  <YAxis
                    tick={{ fill: C.textFaint, fontSize: 11 }}
                    axisLine={false}
                    tickLine={false}
                    width={36}
                  />
                  {ideal && (
                    <>
                      <ReferenceLine y={ideal.min} stroke={C.sage} strokeDasharray="3 3" />
                      <ReferenceLine y={ideal.max} stroke={C.sage} strokeDasharray="3 3" />
                    </>
                  )}
                  <Tooltip
                    contentStyle={{ background: C.surfaceAlt, border: `1px solid ${C.border}`, borderRadius: 8 }}
                    labelStyle={{ color: C.textMuted }}
                    itemStyle={{ color: C.text }}
                  />
                  <Line type="monotone" dataKey="weight" stroke={C.ember} strokeWidth={2.5} dot={{ r: 3, fill: C.ember }} />
                </LineChart>
              </ResponsiveContainer>
            </div>
          ) : (
            <div style={{ color: C.textFaint }} className="text-xs text-center py-8">
              Aggiungi almeno due pesate per vedere il grafico.
            </div>
          )}
        </Card>

        {/* Aggiungi pesata */}
        <Card>
          <span style={{ color: C.textFaint }} className="text-xs mb-2 block">
            Pesata di oggi
          </span>
          <div className="flex gap-2">
            <input
              type="number"
              inputMode="decimal"
              value={weightInput}
              onChange={(e) => setWeightInput(e.target.value)}
              style={{ ...inputStyle, flex: 1 }}
              placeholder="kg"
            />
            <button
              onClick={handleAddEntry}
              style={{ background: C.ember, color: "#15140F" }}
              className="rounded-lg px-4 flex items-center gap-1 text-sm font-semibold"
            >
              <Plus size={16} />
              Salva
            </button>
          </div>
          {saveError && (
            <span style={{ color: C.red }} className="text-xs block mt-2">
              {saveError}
            </span>
          )}
        </Card>

        {/* Storico */}
        {entries.length > 0 && (
          <Card>
            <span style={{ color: C.textFaint }} className="text-xs mb-2 block">
              Storico
            </span>
            <div className="flex flex-col gap-1 max-h-56 overflow-y-auto">
              {[...entries]
                .slice()
                .reverse()
                .map((e) => (
                  <div
                    key={e.id}
                    className="flex items-center justify-between py-1.5"
                    style={{ borderBottom: `1px solid ${C.borderSoft}` }}
                  >
                    <span style={{ color: C.textMuted }} className="text-xs">
                      {formatDateShort(e.date)}
                    </span>
                    <span className="num text-sm" style={{ color: C.text }}>
                      {e.weight} kg
                    </span>
                    <button onClick={() => handleDeleteEntry(e.id)} aria-label="Elimina">
                      <Trash2 size={14} color={C.textFaint} />
                    </button>
                  </div>
                ))}
            </div>
          </Card>
        )}
      </div>
    </div>
  );
}

// ---------------------------------------------------------------------------
// Sottocomponenti
// ---------------------------------------------------------------------------
function Card({ children }) {
  return (
    <div style={{ background: C.surface, border: `1px solid ${C.border}` }} className="rounded-2xl p-4">
      {children}
    </div>
  );
}

function Field({ label, children }) {
  return (
    <div className="flex flex-col gap-1">
      <span style={{ color: C.textMuted }} className="text-xs">
        {label}
      </span>
      {children}
    </div>
  );
}

const inputStyle = {
  background: C.surfaceAlt,
  border: `1px solid ${C.border}`,
  color: C.text,
  borderRadius: 8,
  padding: "8px 10px",
  fontSize: 14,
  width: "100%",
};
